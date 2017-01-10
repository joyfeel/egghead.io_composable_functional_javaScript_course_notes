# 01. Create linear data flow with container style types (Box)
[Video Link](https://egghead.io/lessons/javascript-linear-data-flow-with-container-style-types-box)

We'll examine how to unnest function calls, capture assignment, and create a linear data flow with a type we call Box. This is our introduction to working with the various container-style types.


**Original:**
```Javascript
const nextCharForNumberString = str => {
  const trimmed = str.trim()
  const number = parseInt(trimmed)
  const nextNumber = number + 1
  return String.fromCharCode(nextNumber)
}

const result = nextCharForNumberString('    64    ')
```

**After:**
```Javascript
const Box = x =>
({
  map: f => Box(f(x)),
  fold: f => f(x),
  inspect: () => `Box${x}`
})

const nextCharForNumberString = str =>
  Box(str)              
  .map(s => s.trim())   
  .map(r => new Number(r))
  .map(i => i + 1)
  .map(i => String.fromCharCode(i))
  .fold(c => c.toLowerCase())

const result = nextCharForNumberString('      64     ')
````
