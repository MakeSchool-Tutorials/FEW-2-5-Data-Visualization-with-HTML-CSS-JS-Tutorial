# Data Viz with HTML CSS and JS

This tutorial is about creating bar graphs with HTML, CSS and JS. 

It covers the following concepts: 

- Normalizing data
- Finding the maximum value in a dataset
- Converting values in one unit to another
- Creating elements with JS
- Setting CSS styles with JS
- Using Flexbox to position elements
- Using Absolute Position

## Goals 

This tutorial illustrates creating a data visualization with HTML, CSS, and JS.

## Why?

While there are libraries to draw graphs learning to draw elements to the screen and style them is a good skill to have in your toolbelt. This can empower you to build things that may not be able to build using a library, or to even build your own libraries!

## Getting started

Create a new html file.

You can show simple values using boxes. Each box can be represented by a DIV. The size and other properties of a box, like it's color, can be defined with CSS.

Make a div

`<div></div>`

Let's give this a width, height, and color. 

`<div style="width: 50%; height: 40px; background-color: #000"></div>`

NOTE! The syntax is important. 

- Attributes and values are seprated with a `=`. Style is the attribute here. 
  - `<div style="...">...</div>`
- Attribute values are always quoted. 
  - `style="width: 50%; ..."`
- For the style attribute the value needs to be written as valid CSS. 
  - Use standard CSS property names: `width`, `height`, `background-color`
  - Use standard CSS values, values should always have a unit (with a few rare exceptions)
    - `width:100%` % is the unit
    - `height: 40px` px is the unit
  - Property value pairs must be separated by a semicolon. 
    - `width: 50%; height: 40px; ...; ...; etc.`

Add a second bar. 

```HTML
<div style="width: 50%; height: 40px; background-color: #000"></div>
<div style="width: 100%; height: 40px; background-color: #000"></div>
```

Add some space between the two bars by adding `margin-bottom`. 

```HTML
<div style="width: 50%; height: 40px; background-color: #000; margin-bottom: 2px"></div>
<div style="width: 100%; height: 40px; background-color: #000;  margin-bottom: 2px"></div>
```

## Do it with JS

While the code above does what we want it's pretty verbose and prone to error. It's definitely not [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

Delete the HTML you have and replace it with: 

```HTML
<div id="container"></div>

<script></script>
```

The first div#container will be a container element to which you will add more divs, like the ones you created earlier. The divs created this time with JS. 

You'll put all of your JS inside the `<script>` tag.

## Get a reference and style the container

To style an existing element get a refernce to that element with one of these:

- `document.getElementById()`
- `document.querySelector()`

Add the following to the script tag:

`const container = document.getElementById('container')`

From here you can use `container` to access the div#container above. 

## Set CSS styles via JS 

Add styles to an element through the `style` property. All of the CSS properties are available through JS with a [camelcase](https://en.wikipedia.org/wiki/Camel_case) version of the CSS property name, for example: 

- `background-color` becomes `backgroundColor`
- `font-size` becomes `fontSize`
- `margin-bottom` becomes `marginBottom`
- `width` doesn't change `width`
- `height` doesn't change `height`

Set some styles on the container. Add the following in the script tag. 

```JavaScript
const container = document.getElementById('container')
container.style.width = '600px'
container.style.height = '400px'
container.style.border = '1px solid'
container.style.borderRadius = '10px'
```

This should make a container 600px by 400px with a 1px black border with rounded corners. 

This will be the container for a bar graph.

## Add some bars

Add some bars via JS. Each bar is a div element. Add the following at the bottom of the script tag. 

```JavaScript
...
const bar_1 = document.createElement('div')

bar_1.style.width = '75%'
bar_1.style.height = '40px'
bar_1.style.backgroundColor = '#000'

container.appendChild(bar_1)
```

The first line creates a new element. The tag name will be div. 

The next three lines set CSS properties on the new element. 

The last line adds the new element as a child of container element. Without this last line the new div element would be created but never displayed because it has not been added to the DOM. 

## Adding many bars

More often than not you'll want to create a number of elements equal to the number of data points that you have. For this use a for loop. 

```JavaScript
const data = [33,66,26,77,88,74,42,58]
for (let i = 0; i < data.length; i++) {
  const el = document.createElement('div')
  container.appendChild(el)
  el.style.backgroundColor = '#000'
  el.style.marginBottom = '2px'
  el.style.height = '40px'
  el.style.width = `${data[i]}%`
}
```

The first line defines an array of data. These numbers will set the width of each bar. 

Next, the for loop loops through all of the data.

Inside the loop with each iteration a new element is created, then appended to container, styles are set for each element.

The last line sets the width of each element. The value is set to a %. 

This should create horizontal bars. Divs naturally stack and start on the left side. 

## Verical bars

To make vertical bars you'll need to get the divs you create to arrange themselves side by side. Normally divs stack vertically. To arrange them side by side you can use one of several methods:

- Flexbox
- Float
- Absolute Position
- CSS Grid

The code below uses Flexbox. Remember with flexbox it's the parent element that arranges the children.

```JavaScript
const data = [33,66,26,77,88,74,42,58]
const step = 600 / data.length

container.style.display = 'flex'
container.style.alignItems = 'flex-end'

for (let i = 0; i < data.length; i++) {
  const el = document.createElement('div')
  container.appendChild(el)
  el.style.backgroundColor = '#000'
  el.style.height = `${data[i]}%`
  el.style.width = `${step}px`
  el.style.marginRight = '1px'
  el.style.marginLeft = '1px'
}
```

  Notice the two lines above the for loop. These set styles on container that set it's display mode to Flex. Doing this causes all of the container's children to be arranged by the rules of flex box. 

  Setting `alignItems` to `flex-end` aligns the bars at the bottom of the container so the bars start at the bottom and reach upward. 

## Normalizing data

The data above was given as Percents. This translated well since percent is a valid unit we can use to style our elements. In other words percentages in data translate directly to the screen. 

More often data will be in units that don't translate to the screen. For example how tall is 554 million dollars? Or, how wide is resting heart rate of 87bpm? 

Nomalization is the process of converting values in one unit to value that can be translated to any other unit. For data visualization purposes you usually want to translate millions of dollars and beats per minute to pixels, points, percent, or another unit that translates to the screen. 

Normalize data by converting it into a range from 0.0 to 1.0. Think of this as 0% to 100%  but as a decimal value. Numbers in this range are easily converted to any other unit. 

**To convert a collection of values to decimal percent divide all of the values by largest value in your dataset.**

For example: 

88 / 100 is 0.88 that is 88 is 88% of 100

This works for any numbers. 

56 / 121 is 0.4628099174 in other words 56 is ~46% of 121. 

## Finding the max value

To find the max value in a dataset use `Math.max()`. Max takes a list of values and returns the largest value in the list. 

For example: 

`Math.max(2,8) // returns: 8`

or

`Math.max(2,4,5,3,7,8,1) // returns: 8`

If you have an array you can use the spread operator

```JavaScript
const data = [33,66,26,77,88,74,42,58]
// Find the max value in data
Math.max(...data) // returns: 88
```
## Using map to normalize a dataset

If you have an array you can loop through your array and normalize all of the values. You'll probably want to make a new array with these new values, rather than overwrite the original values. 

One method you can use for this is `Array.map()`. Map loops through an array, applies a callback function to each element in the array and returns a new array. 

Use Map to transform your data. In other words use map to turn an array of one type of values into an array of different type of values. 

For example:

```JavaScript
const data = [33,66,26,77,88,74,42,58]
// Find the max value in data
const max = Math.max(...data)
const dataNormalized = data.map(value => value / max)
console.log(dataNormalized)
```

`normalizedData` should look like this. 

```JavaScript
[
  0.375, 0.75, 
  0.29545454545454547, 
  0.875, 
  1, 
  0.8409090909090909, 
  0.4772727272727273, 
  0.6590909090909091
]
```

Notice the spot that was 88 in data is 1 in the normalized array. 

## Using px in place of %

Sometimes you'll want to use px in place % for sizing and position elements. 

Using Flexbox the browser arranges elements following the rules of Flexbox. You can arrange elements explicitly using absolute position. 

This example creates a set of bars that are arranged using absolute position with the heights set in px by normalizing the values from the data array.

```JavaScript 
const data = [33,66,26,77,88,74,42,58]
const max = Math.max(...data) // Find max
const dataNormalized = data.map(value => value / max) // normalize
const step = 600 / data.length // Find a horizontal step

container.style.position = 'relative' 

for (let i = 0; i < data.length; i++) {
  const el = document.createElement('div')
  container.appendChild(el)
  el.style.position = 'absolute'
  el.style.backgroundColor = '#000'
  el.style.height = `${dataNormalized[i] * 400}px`
  el.style.width = `${step - 1}px`
  el.style.left = `${step * i}px`
  el.style.bottom = '0'
}
```

It's important that the container be position `relative` this defines it as the context for it's absolutely positioned descendants. 

Step is used to divide the horizontal width of the container by the number of items.

The value for `el.style.left` sets the distance from the left edge of container. Setting `el.style.bottom = '0'` positions the element so that it's bottom is 0 pixels from the bottom of container. 

## Challenges 

- Create a `makegraph()` function
  - Write a function that generates a bargraph from an array of values. 
    - Parameters: array of numbers
    - returns: element
  - Add a second parameter that is an array of colors. 
    - colors should be strings that are valid CSS
      - `'#red', #f00, rgb(255, 0, 0), ...` etc.
    - Map each bar to the created to a color in the array
  - Add parameters for the width and height of the container
    