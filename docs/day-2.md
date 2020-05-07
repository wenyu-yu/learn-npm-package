## 1. map-array模块

> Map object keys and values into an array

## 1.1 Usage

```javascript
const const mapArray = require('map-array');
const obj = {
  giorgio: 'Bianchi',
  gino: 'Rossi'
};
console.log(mapArray(obj, (key, value) => key + ' ' + value)); 
// ['giorgio Bianchi', 'gino Rossi']
```



## 2. in-array模块

> Return true if a value exists in an array. Faster than using indexOf and won't blow up on null values.

### 2.1 Usage

```javascript
var inArray = require('in-array');
console.log(inArray(['a', 'b', 'c'], 'a')); //=> true

console.log(inArray(null, 'a')); //=> false

console.log(inArray(null)); //=> false
```

### 2.2 Source Code

```javascript
module.exports = function inArray (arr, val) {
  arr = arr || [];
  var len = arr.length;
  var i;

  for (i = 0; i < len; i++) {
    if (arr[i] === val) {
      return true;
    }
  }
  return false;
};
```



## 3. unordered-array-remove模块

> Efficiently remove an element from an unordered array without doing a splice

### 3.1 Usage 

```javascript
var remove = require('unordered-array-remove')

var list = ['a', 'b', 'c', 'd', 'e']
remove(list, 2) // remove 'c'
console.log(list) // returns ['a', 'b', 'e', 'd'] (no 'c')
```

> This works by popping the last element (which is fast because it doesn't need shift all array elements) and overwriting the removed index with this element.

## 3.2 Source Code

```javascript
function remove (arr, i) {
  if (i >= arr.length || i < 0) return
  var last = arr.pop()
  if (i < arr.length) {
    var tmp = arr[i]
    arr[i] = last
    return tmp
  }
  return last
}
```



## 4. swap-array模块

>  Swap position of two items in array without changing the state of the passed array.

### 4.1 Usage

```javascript
import swapArray from 'swap-array';

var someArray = ['thats','cool','dude'];

swapArray(someArray, 0, 2); // ['dude','thats','cool'];
```



## 5. group-array模块

> Group array of objects into lists.

## 5.1 Usage

```javascript
var groupArray = require('group-array');
var arr = [
  {tag: 'one', content: 'A'},
  {tag: 'one', content: 'B'},
  {tag: 'two', content: 'C'},
  {tag: 'two', content: 'D'},
  {tag: 'three', content: 'E'},
  {tag: 'three', content: 'F'}
];

// group by the `tag` property
groupArray(arr, 'tag');

{
  one: [
    {tag: 'one', content: 'A'},
    {tag: 'one', content: 'B'}
  ],
  two: [
    {tag: 'two', content: 'C'},
    {tag: 'two', content: 'D'}
  ],
  three: [
    {tag: 'three', content: 'E'},
    {tag: 'three', content: 'F'}
  ]
}
```



## 今日总结

+ map-array模块
+ in-array模块
+ unordered-array-remove模块
+ swap-array模块
+ group-array模块