# 06. Create types with Semigroups
[Video Link](https://egghead.io/lessons/javascript-combining-things-with-semigroups)

An introduction to concatting items via the formal Semi-group interface. Semi-groups are simply a type with a concat method that are associative. We define three semigroup instances and see them in action.

`Semigroup` 為一個 group，且能滿足封閉性與結合律

```Javascript
// 字串 concat
const res = 'a'.concat('b').concat('c')

// 陣列 concat
const res = [1, 2].concat([3, 4]).concat([5, 6])
```
### `Sum` semigroup
那要如何讓 `Sum` 這個 semigroup 也能達到類似的運算?

```Javascript
// ???
const res = Sum(1).concat(Sum(2))
```
我們可以設計出如下這種形式:
* `o` 代表 others，可能是 function 或 type 或其它任何東西
* `o` 在此例子代表是被 concat 的另一個 `Sum`，因此要用 `o.x` 把它裡面的 `x` 取出來
* `concat` 這個 method 後面必須回傳另一個 `Sum`
```Javascript
const Sum = x =>
({
  x,
  concat: o => Sum(x + o.x)
})
```
另外，還可以加上 `inspect` method 以便來檢視內部結構
```Javascript
const Sum = x =>
({
  x,
  concat: o => Sum(x + o.x),
  inspect: () => `Sum(${x})`
})
```
有一個 `Destructuring` 的用法為
```Javascript
const { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz	// "aaa"
```
可以讓 `Sum` 的結構更為簡潔
```Javascript
const Sum = x =>
({
  x,
  concat: ({ x: y }) => Sum(x + y),
  inspect: () => `Sum(${x})`
})
```

### `All` semigroup
```Javascript
true && false // false
true && true  // true
```
稍微改變結構
```Javascript
const All = x =>
({
  x,
  concat: ({ x: y }) => All(x && y),
  inspect: () => `All(${x})`
})

const res = All(true).concat(Sum(true))
console.log(res)	// All(false)
```

### `First` semigroup
那個，該如何達到只保留第一個部分的效果呢？
```Javascript
const res = First('blah').concat(First('ice cream'))
console.log(res)	// First('blah)
```
可以將 `concat` 後面的參數省略，只接回傳原先的 `First(x)`
```Javascript
const First = x =>
({
  x,
  concat: _ => First(x),
  inspect: () => `First(${x})`
})
```
