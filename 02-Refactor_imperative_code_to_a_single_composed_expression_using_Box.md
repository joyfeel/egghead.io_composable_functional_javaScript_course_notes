# 02. Refactor imperative code to a single composed expression using Box
[Video Link](https://egghead.io/lessons/javascript-refactoring-imperative-code-to-a-single-composed-expression-using-box)

We refactor 3 functions, taking line by line imperative code to a single composed expression using Box container type.

**Before:**
```Javascript
const Box = x =>
({
  map: f => Box(f(x)),
  fold: f => f(x),
  inspect: () => `Box(${x})`,
})

const moneyToFloat = str =>
  parseFloat(str.replace(/\$/g, ''))

const percentToFloat = str => {
  const replaced = str.replace(/\%/g, '')
  const number = parseFloat(replaced)
  return number * 0.01
}

const applyDiscount = (price, discount) => {
  const cost = moneyToFloat(price)
  const savings = percentToFloat(discount)
  return cost - cost * savings
}

const result = applyDiscount('$5.00', '20%')
console.log(result)
```

**After:**
```Javascript
const Box = x =>
({
  map: f => Box(f(x)),
  fold: f => f(x),
  toString: () => `Box(${x})`
})

const moneyToFloat = str =>
  Box(str)
  .map(s => s.replace(/\$/g, ''))
  .map(r => parseFloat(r))

const percentToFloat = str =>
  Box(str.replace(/\%/g, ''))
  .map(replaced => parseFloat(replaced))
  .map(number => number * 0.01)

const applyDiscount = (price, discount) =>
  moneyToFloat(price)
  .fold(cost =>
    percentToFloat(discount)
    .fold(savings =>
      cost - cost * savings))

const result = applyDiscount('$5.00', '20%')
console.log(result)
```
