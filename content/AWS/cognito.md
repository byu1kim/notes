+++
title = "Cognito"
weight = 4
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Cognito in AWS

Authentication with Server-less. Auth zero is the most populate one that takes care of UI

{{< hbox green >}}
AWS > Cognito > User pools > Create User Pool
{{< /hbox >}}

##### Step 1

- Cognito user pool sign-in options : Username, email (Phone number gets complicated)
- User name requirement : Allow users to sign in with a preferred user name > Next

##### Step 2

- Password policy mode : Cognito defaults or Custom > select the options
- Multi-factor Authentication > No MFA
- User account Recovery : Enable self-service account recovery, Delivery method : Email only > Next

##### Step 3

- Self registration : enable
- Attribute verification : Allow Cognito to send a message, send email, verify email > Next
- Active attribute values when an update is pending (like identity stuff.. something saved in token) > Next

##### Step4

- Email Provider : Send email with Cognito
- From email address > Next

##### Step5

- Uncheck Use the cogito hosted UI
- App type : Public client
- Don't generate a client secret > Review and Create

##### Save the User pool ID

##### Click the pool > App Integration > Copy the Client Id (at bottom)

---

## React APP

### Setup

```
npm i amazon-cognito-identity-js
```

download cognito.js file, add your userPoolId and ClientId in this file
in Vite, update [ vite.config.js ]

```
export default defineConfig({
  plugins: [react()],
  define: {
    global: {}, }, })
```

### Signup

```
import {signUp} from './cognito'
... handleSubmit = async (e) => { ...
await signup({username, email, password}) }
```

```
import { useNavigate } from "react-router-dom";
...
  const navigate = useNavigate();
..
 const handleSubmit = async (e) => {
    e.preventDefault();
    if (password !== passwordConfirm) {
      setError("Password doesn't match");
    } else {
      try {
        await signUp({ username, email, password });
        setError("Check your email");
        navigate(`/confirm?username=${username}`);
      } catch (e) {
        setError(e.message);
        console.log(e.message);
      }
    }
  };
```

### Confirm Email

```
try {
   await confirmUser({username, code})
} catch (e) {
   setError(e.message) }
```

### Login

```
await signIn({username, code})
```

### Logout

```
signOut() // clear localStorage
```

### Get user profile

```
getCurrentUser()
```

### Reset Password

Forgot password

```
await forgotPassword({username})
```

Reset password

```
await resetPassword({username, code, newPassword})
```

### Authorize Requests

```
const token = await userToken()
const imagesResult = await axios.get("whatever your url is", {
  headers: {
    Authorization: token
  }
 })

```

- api with authorization > lambda > authorizaiton > create authorizer.... so only login user can acces this endpoint

https://aws.amazon.com/blogs/security/how-to-secure-api-gateway-http-endpoints-with-jwt-authorizer/

In the API gateway, add a JWT Authorizer and under Authorizer settings enter the following details:

- Name: JWTAuth
- Identity source: $request.header.Authorization
- Issuer URL: https://cognito-idp.us-east-1.amazonaws.com/<your_userpool_id>
- Audience: <app_client_id_of_userpool>

- how to add env in React

### Cognito.js

```js
import { CognitoUserPool, CognitoUser, AuthenticationDetails, CognitoUserAttribute } from "amazon-cognito-identity-js";

function promisify(func) {
  return function () {
    return new Promise((resolve, reject) => {
      func.apply(this, [
        ...Array.from(arguments),
        (err, result) => {
          if (err) {
            reject(err);
            return;
          }
          resolve(result);
        },
      ]);
    });
  };
}

// Add VITE_USER_POOL_ID, and VITE_USER_POOL_CLIENT_ID to a .env.local file in your project root
const poolData = {
  UserPoolId: process.env.REACT_APP_USER_POOL_ID,
  ClientId: process.env.REACT_APP_USER_POOL_CLIENT_ID,
};

const userPool = new CognitoUserPool(poolData);

async function getUserSession() {
  const user = getCurrentUser();
  if (!user) {
    throw "User not logged in";
    return;
  }
  const getSession = promisify(user.getSession).bind(user);

  const session = await getSession();
  return session;
}

/**
 * Get the JWT needed to authenticate with the API
 * @returns {Promise<string>}
 */
export async function getAccessToken() {
  const session = await getUserSession();
  const jwt = session?.accessToken?.jwtToken;
  return jwt;
}

/**
 * Get the currently logged in user
 * @returns {CognitoUser}
 */
export function getCurrentUser() {
  return userPool.getCurrentUser();
}

/**

This function signs up a user with their email, username and password.
It creates an array of CognitoUserAttributes for the user's email and calls
the signUp function from the userPool object.
@param {Object} userData - An object containing the user's email, username and password.
@param {string} userData.username - The username to sign up with.
@param {string} userData.email - The email to sign up with.
@param {string} userData.password - The password to sign up with.
@returns {Promise} - A Promise that resolves to the signUp result object.
*/
export async function signUp({ username, email, password }) {
  const attributeList = [new CognitoUserAttribute({ Name: "email", Value: email })];

  const cognitoSignUp = promisify(userPool.signUp).bind(userPool);
  const result = await cognitoSignUp(username, password, attributeList, null);
  return result;
}

/**

This function confirms a user's registration using their username, email and code.
It creates a CognitoUser object and calls the confirmRegistration function on it.
@param {Object} userData - An object containing the user's username, email and confirmation code.
@param {string} userData.username - The username of the user to confirm.
@param {string} userData.email - The email of the user to confirm.
@param {string} userData.code - The confirmation code to use for confirmation.
@returns {Promise} - A Promise that resolves to the confirmRegistration result object.
*/
export async function confirmUser({ username, email, code }) {
  const userData = {
    Username: username,
    Pool: userPool,
  };

  const cognitoUser = new CognitoUser(userData);
  const confirm = promisify(cognitoUser.confirmRegistration).bind(cognitoUser);
  const result = await confirm(code, false);
  return result;
}

/**

This function signs in a user using their username and password.
It creates a CognitoUser object and calls the authenticateUser function on it.
@param {Object} userData - An object containing the user's username and password.
@param {string} userData.username - The username of the user to sign in.
@param {string} userData.password - The password of the user to sign in.
@returns {Promise} - A Promise that resolves to the authenticateUser result object.
*/
export async function signIn({ username, password }) {
  return new Promise((resolve, reject) => {
    const authData = {
      Username: username,
      Password: password,
    };

    const authDetails = new AuthenticationDetails(authData);

    const userData = {
      Username: username,
      Pool: userPool,
    };

    const cognitoUser = new CognitoUser(userData);
    cognitoUser.authenticateUser(authDetails, {
      onSuccess: (result) => {
        resolve(result);
      },
      onFailure: (error) => reject(error),
    });
  });
}

/**

This function signs out the currently signed in user.
It retrieves the current user using the getCurrentUser function and calls the signOut function on it.
It also removes the access_token and id_token from localStorage.
@returns {void}
*/
export function signOut() {
  const user = getCurrentUser();
  user?.signOut();
  localStorage.removeItem("access_token");
  localStorage.removeItem("id_token");
}

/**

This function sends a forgot password email to the user's email address associated with their username.
It creates a CognitoUser object and calls the forgotPassword function on it.
@param {Object} userData - An object containing the user's username.
@param {string} userData.username - The username of the user to send the forgot password email to.
@returns {Promise} - A Promise that resolves to the forgotPassword result object.
*/
export async function forgotPassword({ username }) {
  const cognitoUser = new CognitoUser({
    Username: username,
    Pool: userPool,
  });
  return new Promise((resolve, reject) => {
    cognitoUser.forgotPassword({
      onSuccess: (data) => {
        resolve(data);
      },
      onFailure: (err) => {
        reject(err);
      },
    });
  });
}

/**
This function resets a user's password using their username, code and new password.
It creates a CognitoUser object and calls the confirmPassword function on it.
@param {Object} userData - An object containing the user's username, confirmation code and new password.
@param {string} userData.username - The username of the user to reset the password for.
@param {string} userData.code - The confirmation code to use for resetting the password.
@param {string} userData.newPassword - The new password to set for the user.
@returns {Promise} - A Promise that resolves to the confirmPassword result object.
*/
export async function resetPassword({ username, code, newPassword }) {
  const cognitoUser = new CognitoUser({
    Username: username,
    Pool: userPool,
  });
  return new Promise((resolve, reject) => {
    cognitoUser.confirmPassword(code, newPassword, {
      onSuccess: (data) => {
        resolve(data);
      },
      onFailure: (err) => {
        reject(err);
      },
    });
  });
}

/**
This function attempts to refresh the access token for the currently signed in user.
It retrieves the user session using the getUserSession function and calls the refreshSession function on the current user object.
If no session is found, it throws an error.
@returns {Promise} - A Promise that resolves to the refreshed user session.
*/
export async function attemptTokenRefresh() {
  let session = await getUserSession();
  if (!session) {
    throw "No session";
  }
  const user = getCurrentUser();
  var refreshToken = session.getRefreshToken();
  const refreshSession = promisify(user.refreshSession).bind(user);
  session = await refreshSession(refreshToken);
  return session;
}
```
