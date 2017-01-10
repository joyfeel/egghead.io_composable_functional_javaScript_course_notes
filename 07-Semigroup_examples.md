# 07. Semigroup examples
[Video Link](https://egghead.io/lessons/javascript-semigroup-examples)

A few examples of Semigroup definitions

現在有 2 個 object，要如何將兩者合而為一呢?
```Javascript
const acct1 = { name: 'Nico', isPaid: true, points: 10, friends: ['Franklin'] }
const acct2 = { name: 'Nico', isPaid: false, points: 2, friends: ['Gatsby'] }
```

這時可以採用 `semigroup` 的做法，把物件內個別的 property 轉為具 semigroup 的特性
```Javascript
const Sum = x =>
({
  x,
  concat: ({x: y}) =>
    Sum(x + y),
  inspect: () =>
    `Sum(${x})`
})

const All = x =>
({
  x,
  concat: ({x: y}) =>
    All(x && y),
  inspect: () =>
    `All(${x})`
})

const First = x =>
({
  x,
  concat: _ =>
    First(x),
  inspect: () =>
    `First(${x})`
})

const acct1 = { name: First('Nico'), isPaid: All(true), points: Sum(10), friends: ['Franklin'] }
const acct2 = { name: First('Nico'), isPaid: All(false), points: Sum(2), friends: ['Gatsby'] }
const res = acct1.concat(acct2) // but still not working
```
但是物件本身是不具備 concat 的性質的，所以需安裝特別的 library 來達到目的
* [immutable](https://github.com/facebook/immutable-js)
* [immutble-ext](https://github.com/DrBoolean/immutable-ext)
```shell
npm i -S immutable immutable-ext
````
對物件作 `Map`
```Javascript
const { Map } = require('immutable-ext')

const acct1 = Map({ name: First('Nico'), isPaid: All(true), points: Sum(10), friends: ['Franklin'] })
const acct2 = Map({ name: First('Nico'), isPaid: All(false), points: Sum(2), friends: ['Gatsby'] })
const res = acct1.concat(acct2)

console.log(res.toJS())    
```
