+++
title = "Transform"
weight = 3
pre = "- &nbsp"
+++

## Transforms

Transforms work with box-model(display: block)

#### Translate

Alters position horizontally or vertically

```css
.box {
  transform: translate(x(가로) px, y(세로) px);
}
```

#### Rotate

Rotate in a clockwise (positive value) or counter-clockwise(negative value)

```css
 {
  transform: rotate(n deg);
}
```

#### Scale

Size of an element

```css
 {
  transform: scale(n);
}
or {
  transform: scaleX(n) scaleY(n);
}
```

#### Skew

```css
 {
  transform: skewX(n deg) skewY(n deg);
}
```

#### Transform-origin

It specify the point on the x, y axis (default: centre, 0 0 : top left)

```css
 {
  transform-origin: n n;
}
```

#### Multiple Transforms

when hover, you have to specify previous transform value as well

```css
 {
  transform: rotate(n deg) skewX(n deg);
}
```

## Transitions

#### Simple Transitions

animation between two (like hover). opacity, width, height, scale, colour.. except display

##### Transition-property

all or specific property (opacity, width … )

##### Transition-duration

how long it takes

##### Transition-timing-function

how the transition progress (cubic-bezier( )), ease, ease-in

https://cubic-bezier.com/ https://easings.net/

##### Transition-delay

how long to wait before starting

```css
{ transition : [property] [duration] [timing-function] [delay:생략가능] }
```

#### Multiple Transitions

Separate by comma,
Can control one transition after using delay like below (width happens after height)

```css
 {
  transition: height 1s ease, width 1s ease 1s;
}
```

## ANIMATIONS

#### Overview

- Keyframe: What changes occur
- Animation : detail about how the animation will run (similar to transition)

#### Animation Properties

##### animation-name

keyframe name (without quotes)

##### animation-duration

how long it should take

##### animation-timing-function

ease, linear and etc.

##### animation-iteration-count

how many time it will run

##### animation-delay

delay

##### animation-direction

alternate on subsequent runs / reset to the start point / repeat itself

##### animation-play-state

paused or running, it will resume from where it was

```css
 {
  animation: [name] [duration] [timing-f] [count] [delay] [direction];
}
```

#### Animation Keyframes

Syntax :

```css
@keyframes name {
}
```

It is defining animation, not applying until you use it animation property
You can control any of css properties
