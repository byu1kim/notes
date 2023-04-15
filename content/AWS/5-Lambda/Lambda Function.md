+++
title = "Lambda Function"
weight = 1
pre = "- &nbsp"
+++

### Lambda function in VSC

Install

```
npm i pg
npm init -y
```

Add Module to `package.json`

```js
"type" : "module"
```

everything (node module, json files) has to be one file, same path as index.js

### Configure `index.js`

#### DB Setup in index.js

connectionString will be added in environment setup on AWS

```
import pg from "pg";
const { Pool } = pg;

let pool;

if (!pool) {
    const connectionString = process.env.DATABASE_URL;
    pool = new Pool({
    connectionString,
    application_name: "",
    max: 1,
    });
}
```

#### Handler

`event.requestContext.http.method` : distinguitsh what request is

```
export const handler = async (event) => {
  switch (event.requestContext.http.method) {
    case "GET":
      return await getWords(event);
    case "POST":
      return await postWord(event);
    case "PUT":
      return await putWord(event);
    case "DEL":
      return await deleteWord(event);
    default:
      return {
        statusCode: 405,
        body: "Method Not Allowed",
      };
  }
};
```

#### Get example

{{% notice note %}}
`JSON.parse()` and `JSON.stringify()` to convert the data
{{% /notice %}}

```
const getWords = async (event) => {
  const body = JSON.parse(event.body);
  const result = await ...SQL query...
  const res = {
    statusCode: 200,
    body: JSON.stringify(result),
  };
  return res;
};
```

#### Delete example

```
let id = event?.pathParameters?.id
let intId = parseInt(id)
... sql query ...
const response = {
statusCode: 200,
body: JSON.stringify(books),
};
```

#### Database SQL query

Use it inside each request function or create a separate `database.js` file

```
// GET
const getLocations = async (event) => {
  const res = await pool.query(`
    SELECT * FROM locations JOIN weather ON locations.id = weather.location_id;
    `);
  const response = {
    statusCode: 200,
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(res.rows),
  };
  return response;
};

// POST
const createLocation = async (event) => {
  const newLocation = JSON.parse(event.body);
  const userId = event.requestContext.authorizer.jwt.claims.sub;

  await pool.query("BEGIN");

  const locationQuery = {
    text: 'INSERT INTO locations (name, timezone, user_uuid VALUES ($1, $2, $3) RETURNING id',
    values: [newLocation.name, newLocation.timezone, userId],
  };
  const locationResult = await pool.query(locationQuery);

  const weatherQuery = {
    text: 'INSERT INTO weather (location_id, timestamp, temperature, description, user_uuid) VALUES ($1, $2, $3, $4, $5)',
    values: [
      locationResult.rows[0].id,
      new Date(),
      newLocation.weather.temperature,
      newLocation.weather.description,
      userId,
    ],
  };
  await pool.query(weatherQuery);
  await pool.query("COMMIT");

  const response = {
    statusCode: 201,
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(locations),
  };
  return response;
};

```

- Check if user is valid : use can check this with 'sub' in the token

### Upload to Lambda

- Compress files (not folder) : package.json, node_modules, index.js
- Back to Lambda > Upload from > .zip file

### Lambda configuration in AWS

- Key : DATABASE_URL
- Value : postgresql://byul:[PASSWORD]@absurd-beast-2237.g95.cockroachlabs.cloud:26257/defaultdb?sslmode=verify-full

- Edit Lambda function
- Add Environment variables in AWS > Lambda > Edit environment variables as
  DATABASE_URL / 'from cockroach connection'

### Test API

Lambda Test or Postman or
Curl in Terminal :

```
curl --request POST \ -- header "content-type:"application/json"  \ --data '{"name" : "White Chicks}' \ https://url for the post
```

---

### React app

{{% notice note %}}
`JSON.stringify()` when sending the data
{{% /notice %}}

```js
const token = await getAccessToken();
const newData = {
  name: name,
  timezone: timezone,
  weather: { temperature: temperature, description: description },
};
const resres = await fetch("https://el7eechsu5.execute-api.ca-central-1.amazonaws.com/weather", {
  method: "POST",
  body: JSON.stringify(newData),
  headers: {
    "Content-Type": "application/json",
    Authorization: token,
  },
}).then((res) => res.json());
```

\* internal server error : usually error in server code

### Deploy in Netlify

- folder : lowercase
- file : uppercase
- npm run build > ci=~~로 변경
- env variables (react: .env, REACT_APP_NAME
- public > \_redirects > and add below
  /\* /index.html 200

\*\* S3 & API Gateway
https://aws.amazon.com/premiumsupport/knowledge-center/api-gateway-upload-image-s3/

### Railway DB

```
npm i mysql2
```

Railway DB Configuration

```
import mysql from 'mysql2'

const pool = mysql
.createPool({
host: process.env.MYSQLHOST,
user: process.env.MYSQLUSER,
password: process.env.MYSQLPASSWORD,
database: process.env.MYSQLDATABASE,
port: process.env.MYSQLPORT || 3306,
})
.promise()
```
