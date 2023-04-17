+++
title = "Fetch Data"
 
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Fetch

```js
fetch("/api/login", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ email, password }),
}).then((res) => res.json());
```

## Axios

```js
axios.get("/api/pokemons").then((res) => {
setUsers(res.data.pokemons);
setIsLoading(true);}).catch((err) => { console.log(err); });}, []);
```

- axios return : return.data
