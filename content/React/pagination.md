+++
title = "Pagination"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Terms

- `Total` : Total length of data
- `Limit` (pageSize) : number of items per page
- `Page` : current page number
- `Offset` : index of first item of the page (offset = (page - 1) \* limit)

## Pagination interacting with Backend

states : page, pageSize(or limit), total

add offset only when you not with database side pagination

#### Data Fetching

```js
axios.get(api, {
  headers: {
    Authorization: token,
  },
  params: {
    page: page,
    pageSize: pageSize,
    searchTerm: searchTerm,
  },
});
```

#### Backend

```js
const getData = async (event) => {
  const { page, pageSize, searchTerm } = event.queryStringParameters;

  const offset = (page - 1) * pageSize;
  const limit = parseInt(pageSize, 10);

  const result = await db.getData(userId, searchTerm, offset, limit);
  return result;
};
```

#### Database

```js
export const getAll = async (search, offset, limit) => {
  const query = {
    text: `
      SELECT * FROM table_name
      WHERE column_name ILIKE $1
      OFFSET $2
      LIMIT $3
    `,
    values: [`%${search}%`, offset, limit],
  };
  const result = await pool.query(query);
  return result;
};
```

#### Pagination component

```js
export default const Pagination = ({ total, limit, page, setPage }) => {
  const totalPages = Math.ceil(total / limit);

  return (
    <div className="pagination">
      <button className="arrow" onClick={() => setPage(1)} disabled={page === 1}>
        <<
      </button>
      <button className="arrow" onClick={() => setPage(page - 1)} disabled={page === 1}>
        <
      </button>
      <div className="pages">
        {Array(totalPages)
          .fill()
          .map((_, i) => (
            <button
              className="page"
              key={i + 1}
              onClick={() => setPage(i + 1)}
              aria-current={page === i + 1 ? "page" : null}
            >
              {i + 1}
            </button>
          ))}
      </div>
      <button className="arrow" onClick={() => setPage(page + 1)} disabled={page === totalPages}>
        >
      </button>
      <button className="arrow" onClick={() => setPage(totalPages)} disabled={page === totalPages}>
        >>
      </button>
    </div>
  );
};

Pagination.defaultProps = {
  total: 0,
  limit: 100,
  page: 1,
  setPage: 1,
};
```

## Static Data

```js
data.slice(offset, offset + pageSize).map()...
```

Slice the map (offset, offset + limit)
https://www.daleseo.com/react-pagination/
https://codesandbox.io/s/react-pagination-qqrdf?from-embed=&file=/src/Pagination.jsx
