+++
title = "CSS"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Gradients

```
linear-gradient(90deg, color unit, color unit)
```

- Properties : radial-gradients, conic-gradients, repeating-(type)-gradients
- Multiple gradients : separate by comma
- Cheat site : https://cssgradient.io/

https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Images/Using_CSS_gradients

## Border

```
border-radius: 50%; (circle)
border-radius: (top-left, top-right, bottom-right, bottom-left);
border-radius: 5rem / 10rem; (horizontal, vertical)
```

- Unit: px, em, %

## Opacity

```
Opacity: 0.5;
```

- Unit: 0 ~ 1
- Child element can only decrease the opacity

## RGBA

```
bakcground-color: rgba(0, 0, 0, 0.5);
```

- RGB color format + alpha(opacity) channel

## Box Shadow

```
box-shadow: inset(option) (x) (y) (blur-radius) (spread-radius) (color);
```

- Multiple box-shadow is seperate by comma : box-shadow 0 0 0 0 yellow, 0 0 0 0 black;

## Text Shadow

```
text-shadow: (offset-x) (offset-y) (blur) (color);
```

- Multiple : text-shadow: 1px 1px 1px gray, 2px 2px 2px yellow;

## Filter

```
filter: blur(%)
```

- Properties : hue-rotate(degree), saturate(%), brightness(%), contrast(%), grayscale(%), sepia(%)

https://developer.mozilla.org/en-US/docs/Web/CSS/filter#try_it

## Selector

```css
.class: nth-child(n) {
  ...;
}
```

- nth-child(even) or (odd)
- nth-child(n+1)
- nth-child(3n)
