+++
title = "Flexbox"
weight = 1
pre = "- &nbsp"
+++

## Flexbox

Single dimension. Direct children become flex items

##### Parent

```css
.parent {
  display: flex;
}
```

##### Align center (Parent container)

```css
.parent {
    display: flex;
    justify-content: center // main axis
    align-items: center; // cross axis
}
```

## Flex `Parent` Properties

#### Flex Direction

```
flex-direction: row;
```

- Values : column (vertical), row-reverse, column-revers
- Main axis : row (left to right), column (top to bottom)

#### Flex Wrap

Default

```
flex-wrap: nowrap; // 한 줄에 쭉 정렬하는거, 공간 없으면 shrink
```

Values : wrap (다음 줄로 넘어감), wrap-reverse (새로운 아이템이 추가 되면 cross axis 거꾸로 추가)

#### Flex Flow

Shorthand for combination of flex-direction and flex-wrap

```
flex-flow: column wrap; // flex-direction, flex-wrap
```

#### Flex Gaps

```
row-gap: 10%;
column-gap: 10%;
gap: (row column);
```

## Flex Child Properties

#### Flex Grow

How much the item takes ‘extra space’ (after wrapping if wrap is enabled), along the main axis

```
flex-grow: 0 // Unitless
```

#### Flex Shrink

How much the item shrink when there is ‘not enough’ space along the main axis

```
flex-shrink: 1 // Unitless
```

If you don’t want item to shrink flex-shrink: 0;

#### Flex Basis

To set the initial minimum size along the main axis (minimum width or height)

```
flex-basis: auto
```

- Unit: em,px, %, etc
  Ex) flex-basis: 200px;

#### Flex Shorthand

Grow + Shrink + Basis

- Value 1개: flex: (flex-grow) or (flex-basis)px; // 유닛 있으면 그로우, 아니면 베이시스
- Value 2개: flex: (flex-grow) (flex-shrink); (flex-grow) (flex-basis)px;
- Value 3개: flex: (flex-grow) (flex-shrink) (flex-basis);

#### Ordering Flex Items

Changing the orders.
Screen reader 머시기 땜시 HTML에서 바꾸는게 더 낫긴 함 아래 참고
https://webaim.org/blog/flexbox-and-the-screen-reader-experience/

```
.container : nth-child(n) { order: 3; }
```

## Flex Alignment and Justification Properties

#### justify-content (main axis, parent)

- Parent, main axis
- flex-start, flex-end, center
- space-between: space out between items, outside items go to edges
- space-around: space but yes space before border
- space-evenly : even spaces

#### align-items

- Parent, cross axis
  flex-start / flex-end / center / baseline

#### align-self

Child에서, align-items랑 속성은 같음

#### align-content

Align wrapped content (flex-wrap: wrap일 때)
flex-start / flex-end / center / space-between / space-around / space-evenly / stretch

#### place-self

justify-content + align-self

## Practice

Flexbox Froggy
https://flexboxfroggy.com/

Cheat sheet
https://yoksel.github.io/flex-cheatsheet/
