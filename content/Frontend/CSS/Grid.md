+++
title = "Grid"
weight = 2
pre = "- &nbsp"
+++

{{% notice info %}}
Length units for grid : **px**, **ex**(relative to parent), **rem**(relative to html), **fr**(fractional)
{{% /notice %}}

## Grid Layouts

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
}
```

Shorthand

```
grid-template: rows / columns;
```

Grid container won’t have any effect until the columns and rows are defined.

## Repeat

```
grid-template-columns: repeat(n, size);
```

## Auto

- Explicit grid : when you specify grid-template-columns, grid-template-rows together
- Implicit : auto generated. Grid-auto-rows: grid height is determined by the tallest item

```
grid-auto-rows: 15rem; auto size for rows, repeat(2,1fr)은 row가 무수히 많을 때 의미가 없으므로
```

```
grid-auto-columns: n; grid-flow가 column 일 때 사용
```

```
grid-auto-flow: row or column; grid의 direction (=flex-direction)

```

```
auto-fill; exact size(value)
```

```
auto-fit; stretch(value)
```

## minmax()

```
grid-auto-rows: minmax(minsize, maxsize)
```

- min-content
- max-content

## Responsive = repeat + auto + minmax

```
grid-template-columns:repeat(auto-fit, minmax(10px, 1fr));
```

## Gap

- Unit: px, rem, em, % (fr x), in Parent

```
column-gap : 2px;
row-gap: 1rem;
gap: (column) (row);
```

## Start/End

```
grid-column-start: 1
grid-column-end: 4 (= grid-column-end: span 3; or -1;)
grid-column : 1 / 4; (same as start + end)
grid-column: span 4 (grid item takes 3 spaces)
```

## Grid Template Areas

(parent)

```
grid-template-columns: 1fr 1fr;
grid-template-rows: 1fr 1fr 1fr;
grid-template-areas : “ header header “ “side content” “footer footer”
```

- area는 columns, rows에 알맞게 배열

(child)

```
grid-area: name;
```

- name에 “” 안붙임

## Alignment

(parent)

- justify-content
- align-content
- place-content

(child)

- justify-items
- align-items
- place-items

\* grid child does not layer or overlap each other

## Practices

https://gridbyexample.com/

https://www.quackit.com/css/grid/examples/css_grid_website_layout_examples.cfm
