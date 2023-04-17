+++
title = "Lambda Function"
menuTitle="Lambda"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

### Lambda

Serverless, event-driven compute service

### Serverless

- Cloud-native development model.
- Applications without having to manage severs. (Pay what you use)

### Lambda Setup

{{< vbox pink >}}
AWS > Lambda > Function > Create function
{{< /vbox >}}

- Function name : get/post/put etc
- Runtime : Lambda function language (Node.js 18.x)
- Architecture : x86_64 (arm64 is for mobile)

### Lambda Function

- mjs : Moduled Javascript (ES6, same as javascript file with type:module in package.json)
- Hit Deploy > Test
- Every lambda is triggered by something. Here, test btn is the trigger
- API Gateway is one of trigger (it is kind of nginx, accept http request)
- Check console.log : Monitor > CloudWatch

# API Gateway

### API Gateway Setup

{{< vbox blue >}}
AWS > API Gateway > HTTP API > Build (or Create API) > API name > Review and Create
{{< /vbox>}}

- API Gateway > Develop > Routes : Create endpoints (get, post, etc)
- Dynamic ID ex) get, /book/{id}
- Click Route > Route details > Attach integration
- Integration target > Lambda function > Select lambda function you created it for
- URL : API > invoke URL

### API Routes example

- GET /todos
- GET /todos/{id}
- POST /todos
- PUT /todos/{id}
- DELETE /todos/{id}

### CORS

- API Gateway > Develop > CORS
- access-control-allow-origin : \* (only for the get request)
- access-control-allow-headers : \* => not safe way but just use it for now
- credentials : no

### Add Authorizer in API Gateway (after Cognito)

- API Gateway > Routes > Route details > Attach authorization
- Create and attach an authorizer > JWT (IAM is more advanced)
- Issuer URL :

```
https://cognito-idp.<region>.amazonaws.com/<userpoolid>
```

- Audience : Client ID from Cognito
- Check : Gateway > Develop > Authorization

{{% notice warning %}}
if you have no authorization with this endpoint, you have 401 error
{{% /notice%}}

In your lambda function, you can get the user id like this

````

const handler = async (event) => {
// Get the currently logged in user
const userId = event.requestContext.authorizer.jwt.claims.sub
location.userId = userId;

```

When making a request to one of the authoried endpoints, you have to send the JWT in the header like this: (from react)

```

const token = await userToken()
const imagesResult = await axios.get("whatever your url is", {
headers: {
Authorization: token
}
})

```

# Cockroach DB

### Cockroach DB

Serverless Database

### Connection String in Terminal

```

cockroach sql --url "postgresql://byul@absurd-beast-2237.g95.cockroachlabs.cloud:26257/[DB_name]?sslmode=verify-full"

```

### Schema Example

```

CREATE TABLE locations (
id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
user_uuid UUID,
name STRING,
timezone STRING );

CREATE TABLE weather (
id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
location_id UUID REFERENCES locations (id),
user_uuid UUID,
timestamp TIMESTAMP,
temperature FLOAT,
description STRING
);

```

```
````
