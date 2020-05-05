# 前言

昨天逛Node社区，看到狼叔在Github写的一篇如何学好Node.js的文章，深受启发，总结一下无非是先会用，再自己写工具，最后看源码甚至参与开源项目，Node 用途那么多，如果开始能直接上web应用，那就直接写web应用，如果没有机会就从前端构建，工具等方面开始做，慢慢引入更潮更酷的前端技术，自然就把Node引入进来了，当你用了一段之后，你会对Node.js的运行机制好奇，为啥呢？这时候去读朴大的《深入浅出Node.js》一书就能够解惑。原因很简单，九浅一深一书是偏向底层实现原理的书，从操作系统，并发原理，node源码层层解读。如果是新手读，难免会比较郁闷，另外狼叔也提到了一个**迷茫时学习Node.js最好的方法**：**每天看10个npm模块**，对于学习Node.js必经之路，一定是要掌握很多模块用法，一方面可以从中汲取技巧、思路、设计思想的，还有可以提升自己的大局观，有利于以后技术选型和自己写开源项目，今天尝试看一些npm模块，确实有新的体验，以前都是碰到问题再去查，现在有了阅读npm模块的意识，更能了解npm社区的活跃度，没想到一个小功能都能单独写成一个npm包，学到了



## 如何看npm包

先看一个github库从这开始看，之后我们可以从一个npm模块的依赖项依次开始看，逐步积累，地址如下：



https://github.com/parro-it/awesome-micro-npm-packages



## 1. is-sorted模块

### 1.1 Usage

```javascript
let sorted = require('is-sorted');

sorted([1,2,3]) // true
sorted([3,2,1]) // false
sorted([3,2,1]),function (a,b) { return b - a }); // true
```

### 1.2 Source Code

```javascript
function defaultComparator(a,b) {
    return a - b;   // 默认判断是不是升序的;
}

module.exports = function (array, comparator) {
    if(!Array.isArray(array)) {
        throw TypeError(`Except a Array, but get a ${typeof array} type`);
    }
    comparator = comparator ?? defaultComparator; // comparator 判断是不是降序的
    for(let i = 1,len = array.length; i < len;i++) {
        if(comparator(array[i-1,array[i]) > 0) return false;
    }
    return true;
}
```



## 2. is-number模块

> Returns true if the value is a finite number.

### 2.1 Usage

```javascript
const isNumber = require('is-number');
isNumber(5e3);               // true
isNumber(0xff);              // true
isNumber(-1.1);              // true
isNumber(0);                 // true
isNumber(100);               // true
isNumber('-1.1');            // true
isNumber('012');             // true
isNumber('0xff');            // true
isNumber('1');               // true
isNumber('5e3');             // true
isNumber(parseInt('012'));   // true
isNumber(parseFloat('012')); // true
```

### 2.2 Source Code

```javascript
module.exports = function(num) {
    if(typeof num === 'number') {
        return true;
    }
    if(typeof num === 'string' && num.trim() !== '') { // 判断非空字符串
        // + 是把字符串转换为数字
        return Number.isFinite ? Number.isFinite(+num) : isFinite(+num);
    }
    return false;
}
```



## 3. array-slice模块

> Array-slice method. Slices `array` from the `start` index up to, but not including, the `end` index.

### 3.1 Usage

```javascript
let slice = require('array-slice');
let arr = ['a', 'b', 'd', 'e', 'f', 'g', 'h', 'i', 'j'];

slice(arr, 3, 6); //=> ['e', 'f', 'g']
```

### 3.2 Source Code

> This function is used instead of `Array#slice` to support node lists in IE < 9 and to ensure dense arrays are returned. This is also faster than native slice in some cases.

```javascript
module.exports = function slice(arr, start, end) {
  var len = arr.length;
  var range = [];

  start = idx(len, start);    // 获取初始索引
  end = idx(len, end, len);   // 获取末尾索引

  while (start < end) {
    range.push(arr[start++]);
  }
  return range;
};

function idx(len, pos, end) {
  if (pos == null) { 
    pos = end || 0;  // 如果没指定初始索引，则为从0开始
  } else if (pos < 0) {  // 如果初始索引为负数，则从 Array.length - pos 位置开始
    pos = Math.max(len + pos, 0);
  } else {
    pos = Math.min(pos, len);
  }

  return pos;
}
```



## 4. array-first模块

> Get the first element or first n elements of an array.

### 4.1 Usage

```javascript
let first = require('array-first');

first(['a', 'b', 'c', 'd', 'e', 'f']);     // => 'a'

first(['a', 'b', 'c', 'd', 'e', 'f'], 1);  // => 'a'

first(['a', 'b', 'c', 'd', 'e', 'f'], 3);  // => ['a', 'b', 'c']
```

### 4.2 Source Code

```javascript
let isNumber = require('is-number');
let slice = require('array-slice');

module.exports = function(arr,num) {
    if(!Array.isArray(arr)) {
        throw TypeError(`Except a Array, but got a ${typeof arr}`);
    }
    if(array.length === 0) {
        return null;
    }
    let first = slice(arr,0, isNumber(num)? num : 1);
    if(+num === 1 || num === null) {
       // 如果num=1,则first为arr[0],但是形式是数组，我们要取他的值
       // 如果num=null.则first为整个数组，我们要取的也是第一个值;
       return first[0]; 
    }
    return fitst;  // 其他情况直接返回
}
```



## 5. array-last模块

> Get the last or last n elements in an array.

### 5.1 Usage

```javascript
var last = require('array-last');

last(['a', 'b', 'c', 'd', 'e', 'f']);    //=> 'f'

last(['a', 'b', 'c', 'd', 'e', 'f'], 1); //=> 'f'

last(['a', 'b', 'c', 'd', 'e', 'f'], 3); //=> ['d', 'e', 'f']
```

### 5.2 Source Code

```javascript
let isNumber = require('is-number');

module.exports = function last(arr, n) {
  if (!Array.isArray(arr)) {
    throw new Error('expected the first argument to be an array');
  }

  let len = arr.length;
  if (len === 0) {
    return null;
  }

  n = isNumber(n) ? +n : 1;
  if (n === 1) {
    return arr[len - 1];
  }

  var res = new Array(n);
  while (n--) {
    res[n] = arr[--len];
  }
  return res;
};
```



## 6. arr-flatten模块

> Recursively flatten an array or arrays.

### 6.1 Usage

```javascript
let flatten = require('arr-flatten');

flatten(['a', ['b', ['c']], 'd', ['e']]);
```

### 6.2 Source Code

```javascript
module.exports = function (arr) {
  return flat(arr, []);
};

function flat(arr, res) {
  var i = 0, cur;
  var len = arr.length;
  for (; i < len; i++) {
    cur = arr[i];
    Array.isArray(cur) ? flat(cur, res) : res.push(cur);
  }
  return res;
}
```



## 7. dedupe模块

> removes duplicates from your array.(数组去重)

### 7.1 Usage

```javascript
let dedupe = require('dedupe')

// primitive types ======================
let a = [1, 2, 2, 3]
let b = dedupe(a)
console.log(b)  // [1, 2, 3]

// complex types ======================
let aa = [{a: 2}, {a: 1}, {a: 1}, {a: 1}]
let bb = dedupe(aa)
console.log(bb) // [{a: 2}, {a: 1}]

// complex types types with custom hasher
let aaa = [{a: 2, b: 1}, {a: 1, b: 2}, {a: 1, b: 3}, {a: 1, b: 4}]
let bbb = dedupe(aaa, value => value.a)
console.log(bbb)  // [{a: 2, b: 1}, {a: 1,b: 2}]
```

### 7.2 Source Code

```javascript
function dedupe (client, hasher) {
    hasher = hasher || JSON.stringify

    const clone = []
    const lookup = {}

    for (let i = 0; i < client.length; i++) {
        let elem = client[i]
        let hashed = hasher(elem)

        if (!lookup[hashed]) {
            clone.push(elem)
            lookup[hashed] = true
        }
    }

    return clone
}

module.exports = dedupe
```



## 8. array-range模块

> Tiny module to create a new dense array with the specified range.

### 8.1 Usage

```javascript
let range = require('array-range')
range(3)       // -> [ 0, 1, 2 ]
range(1, 4)    // -> [ 1, 2, 3 ]
range(5).map( x => x*x ) // -> [ 0, 1, 4, 9, 16 ]
range(2, 10).filter( x => x%2===0 ) // -> [ 2, 4, 6, 8 ]
```



## 9. arr-diff模块

> Returns an array with only the unique values from the first array, by excluding all values from additional arrays using strict equality for comparisons.

### 9.1 Usage

```javascript
let diff = require('arr-diff');

let a = ['a', 'b', 'c', 'd'];
let b = ['b', 'c'];

// Returns the difference between the first array and additional arrays
console.log(diff(a, b)) //=> ['a', 'd']
```



## 10. filled-array模块

> Returns an array filled with the specified input

### 10.1 Usage

```javascript
const filledArray = require('filled-array');

filledArray('x', 3); //=> ['x', 'x', 'x']

filledArray(0, 3); //=> [0, 0, 0]

filledArray(i => {
	return (++i % 3 ? '' : 'Fizz') + (i % 5 ? '' : 'Buzz') || i;
}, 15);
//=> [1, 2, 'Fizz', 4, 'Buzz', 'Fizz', 7, 8, 'Fizz', 'Buzz', 11, 'Fizz', 13, 14, 'FizzBuzz']
```

### 10.2 Source Code

```javascript
module.exports = function (item, n) {
	var ret = new Array(n);
	var isFn = typeof item === 'function';

	if (!isFn && typeof ret.fill === 'function') {
		return ret.fill(item);
	}

	for (var i = 0; i < n; i++) {
		ret[i] = isFn ? item(i, n, ret) : item;
	}

	return ret;
};
```



## 今日总结

+ is-sorted（https://github.com/dcousens/is-sorted）

+ is-number（https://github.com/jonschlinkert/is-number）
+ array-slice（https://github.com/jonschlinkert/array-slice）
+ array-first（https://github.com/jonschlinkert/array-first）
+ array-last（https://github.com/jonschlinkert/array-last）
+ arr-flatten（https://github.com/jonschlinkert/arr-flatten）
+ dedupe（https://github.com/seriousManual/dedupe）
+ array-range（https://github.com/mattdesl/array-range）
+ arr-diff（https://github.com/jonschlinkert/arr-diff）
+ filled-array（https://github.com/sindresorhus/filled-array）



## 我的npm学习记录

https://github.com/wenyu-yu/learn-npm-package