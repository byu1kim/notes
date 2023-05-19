+++
title = "Data Fetching"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Fetch

#### Get

```js
await fetch(api, {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    Authorization: token,
  },
  body: JSON.stringify(dataObject),
}).then((res) => res.json());
```

#### Post/Put

```js
await fetch(api, {
  method: "POST", //or PUT
  headers: {
    "Content-Type": "application/json",
    Authorization: token,
  },
  body: JSON.stringify(dataObject),
}).then((res) => res.json());
```

#### Delete

```js
await fetch(api, {
  method: "DELETE",
  headers: {
    "Content-Type": "application/json",
    Authorization: token,
  },
  body: JSON.stringify(dataObject),
}).then((res) => res.json());
```

## Axios

#### Get

```js
await axios
  .get(api, {
    headers: {
      Authorization: token,
    },
    params: {
      param1,
    },
  })
  .then((res) => {
    setData(res.data);
  })
  .catch((err) => setError(err));
```

#### Post/Put

```js
await axios
  .put(api, dataObject, {
    headers: {
      "Content-Type": "application/json",
      Authorization: token,
    },
  })
  .then((res) => res.data)
  .catch((err) => setError(err));
```

#### Delete

Axios can't accept body. Use params to send the data with delete request

```js
await axios
  .delete(api, {
    headers: {
      Authorization: token,
    },
  })
  .then((res) => res.data)
  .catch((err) => setError(err));
```

- axios return : return.data
