+++
title = "HTML"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Form

#### Form Data Flow

1. User filles the inputs
2. User submits the form
3. User inputs are validate
4. Data from the form is sent to a processing script(JS or PHP)
5. Form data is processed (client side or server side)
6. Processor(JS) returns a status and response data
7. Visual feedback given to the user (option)

#### Form Element Attributes

- action: URL of the form processing endpoint (file)
- method : GET, POST
- name : sent to the processor

#### Form Methods

1. GET

- Data is sent to the query parameters in the URL (Don’t send sensitive info)
- GET URL can be bookmarked

2. POST

- Data is transmitted as part of the HTTP request body. (sent to the processor)

#### Input : Attributes

- type
- name : for the server
- id : for CSS/JS
- disabled : visible, does not send the data to the server
- readonly : visible, simmilar to disable but send the date to the server
- hidden : invisible, date is sent
- required
- placeholder : prompt text in text inputs
- pattern : data validation using regex with javascript

#### Input : Types

```html
<input type="“types”" />
```

1. text

- attributes : name, id, minlength, maxlength, size, value, required

2. textarea

- long form text
- no value attribute
- no self closing

```html
<textarea> </textarea>
```

- attributes : row=“height”, cols=“equivalent of size for text input”

3. password

- single-line text field

4. checkbox

- Select multiple options
- input is self closing so they are grouped via the name attribute
- should have a value attribute
- checked =“true”: if you want to make it initially checked

```html
<input type="“checkbox”" name="“css”" value="“css”" /> <label for="“css”">CSS</label>
```

5. radio

- Single selection
- Group with same name
- checked=“true” : initial check

```html
<input type="“radio”" name="“aa”" value="“a”" />
<label for="“a”">
  <input type="“radio”" name="“aa”" value="“b”" />
  <label for="“b”"></label
></label>
```

6. select

- name: when form is submitted
- id: for label
- multiple : if you want to select more than one
- option : should have value
- selected : initial value
- group : optgroup

```html
<select multiple>
  <optgroup label="“Person”">
    <option value="“”" selected></option>
  </optgroup>
</select>
```

7. submit

- displayed as button, you can submit the form hitting enter (submit is not required)
- Submit : button
- Reset : button, clears the forms

```html
<input type="“submit”" value="“button”" />
```

8. ETC : email, tel, number, color

#### Label Element

```html
<label for="“id" of the input”>NAME</label>
```

#### Fieldset and Legend Elements

- fieldset : grouping of inputs, labels
- legend : caption(label) of the fieldset

```html
<fieldset>
  <legend>Acount info</legend>
  <label><input> * n
</fieldset>
```

#### Placeholder Attribute Accessibility Issues

- Do not omit label, if you want them to hide, use css below

```css
  .visually-hidden {
    position: absolute !important;
    width: 1px !important;
    height: 1px !important;
    padding: 0 !important;
    margin: -1px !important;
    overflow: hidden !important;
    clip: rect(0, 0, 0, 0) !important;
    white-space: nowrap !important;
    border: 0 !important; }
```

#### Styling Placeholder Text

- since placeholder is a pseudo-element from a CSS perspective, use below
  input::placeholder { color: blue }

#### Input :focus

- when click the input, you can style it too using pseudo selector
  .form input:focus { color: blue}

#### HTML5 Form Validation

- HTML5 does simple input validation
- required attribute is one of it
- type=“email” : html5 validate if it is correct email format
- style …:valid { } / …:invalid { }

#### Pattern Attribute

- Using regex
- Attributes : use title, aria-label together
- Style: ..:invalid { }

```html
<input type="“”" … pattern="“REGEX”" title="“error" message” aria-label="“error" message” />
```

- HTML regex: https://www.html5pattern.com/

## Responsive Images

#### Web Image Formats

JPEG, PNG, GIF, SVG, WebP

#### Image attributes : srcset, sizes

- HTML img attributes to handle responsive images
- It is about which image is loaded when the page is rendered. (when changing screen size, img doesn’t change).

#### Srcset

- Developer : provide a set of images and resolution information
- Browser : choose the most appropriate one based on the screen dimensions
- srcset=“1) image path 2) space 3) image’s real size in pixel + w”

```html
<img src="“image”" alt="“alt”" srcset="“img_path" 2400w, img_path 1200w, …” />
```

#### Sizes

- Developer : specify what size of the image under media condition
- 1. A media condition : media query. (Generally same as css media query breakpoint)
- 2. Image size : when the media condition is true, the image size (not percentage, px, vw only)
- 3. Default image size when the condition is not met. (after last comma)

```html
<img .. size="”(max-width:" 1260px) calc(100vw - 60px), 1200px” />
```

#### Testing srcset & sizes

Inspect > network > disable cache > refresh(f5)

#### Picture Element

- Source tells the browser to load a certain image (srcset) at a certain screen width (media).
- Img inside of <picture> is default. When you don’t specify, img doesn’t show up.
- Flow : find media in picture > srcset, size > display appropriate img> if none, display img

```html
<picture>
  <source srcset="images/vancouver-skyline-winter-retina.jpg 1600w, sizes="(max-width: 960px) calc(100vw - 40px), 920px"
  media="(max-width: 600px)" <img src=“image" />
</picture>
```

## IMAGE SOFTWARE AND NOTES

#### Image Editing Software

Photopea

https://www.photopea.com/

Vectr

https://vectr.com/

Pixlr

https://pixlr.com/

Toast.ui Image Editor

https://github.com/nhn/tui.image-editor

#### Free Images

Unsplash

https://github.com/nhn/tui.image-editor

Pixabay

https://pixabay.com/

CC Search

https://wordpress.org/openverse/?referrer=creativecommons.org

Wikimedia Commons

https://commons.wikimedia.org/wiki/Main_Page

Google Images with a filter by license
