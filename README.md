# numberer [![Build Status](https://secure.travis-ci.org/safareli/numberer.png?branch=master)](http://travis-ci.org/safareli/numberer) [![NPM version](https://badge-me.herokuapp.com/api/npm/numberer.png)](http://badges.enytc.com/for/npm/numberer) [![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/safareli/numberer/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

> describe numbers with other numbers and make them reactive

## introduction
Install the module with: `npm install numberer`

```javascript
var N = require('numberer');
function Particle(){
    // create numberer objects by passing value as an argument
    this.totalLife = new N(100);
    // numberer returns a new object with the same value
    this.life = this.totalLife.clone();
    // Numberer.div is static function and it returns
    // new Numberer whose value would be the division of all arguments' values
    // plus(a, b, c) ==> a + b + c
    // minus(a, b, c) ==> a - b - c
    // div(a, b, c) ==> a / b / c
    // mult(a, b, c) ==> a * b * c
    // mod(a, b, c) ==> a % b % c
    this.alpha = N.div(this.life, this.totalLife);
    this.color = {
        r: N.mult(this.alpha, 255),
        // you can call other math functions on numberer instances
        // and they will change its value
        g: N.mult(this.alpha, 130).plus(50),
        b: N.mult(this.alpha, 50).plus(100)
    };
};
var p = new Particle();
 
console.log(p.alpha.get()) // 1
console.log(p.color.r.get()) //255
console.log(p.color.g.get()) //180
console.log(p.color.b.get()) //150
 
// after changing the life value when we get value
// of other depended numberers they will be changed too
p.life.set(50);
 
console.log(p.alpha.get())   //0.5
console.log(p.color.r.get()) // 125.5
console.log(p.color.g.get()) // 115
console.log(p.color.b.get()) // 125
```

## Documentation

#### Numberer(value) `constructor`
**Parameter**: `value`
**Type**: `*`
value could be anything but when at the time I was writing it I thought it will be great for numbers and function which return numbers.

How to use this method
```javascript
var width = N(function(){return window.innerWidth;});
var offset = N(150);
```
#### .get()`prototype`
if the value is a function its result will be returned otherwise value will be returned.

How to use this method
```javascript
var foo = new N(null), width = N(function(){return window.innerWidth;});

console.log(foo.get()); // null;
console.log(width.get()); // 1024;
```

#### .set(value)`prototype`
**Parameter**: `value`
**Type**: `*`
it is like a constructor, it just sets the value. it although returns `this` so you can chain on it if you want.

How to use this method
```javascript
var foo = new N(null);

console.log(foo.get()); // null;
foo.set(function(){return window.innerWidth;})
console.log(foo.get()); // 1024;
```

#### .clone()`prototype`
it returns a new numberer with the same value

How to use this method
```javascript
var foo = new N(10);
var baz = foo.clone();
console.log(foo.get()); // 10;
console.log(baz.get()); // 10;
```

### math
there are to types of math functions:

 - `Numberer.mathName`
 - `Numberer.prototype.mathName`

#### Numberer.mathName
They **return new Numberer** whose value is a function and this function is doing reduce over the arguments array with specific operation depending on the function (they will be discussed in a moment)

#### Numberer.prototype.mathName
They **change the value** of `this` object and return it so it is possible to chain some of the functions and after that `this` value will be changed to a function which does prepends its value to the arguments array and does reduce over it



#### .plus(values...)
**Type**: `Number|Numberer`

How to use this method
```javascript
var baz = new N(7);
var c = 9;
var foo = new N(function(){return window.innerWidth;}); //1024

//1024 + 7 + 9 + 4
var boss = N.plus(foo, baz, c, 4);
console.log(boss.get()); // 1044
// 1024 + 7 + 9 + 4 + 30 + 1024 +2
boss.plus(30, foo).plus(2);
console.log(boss.get()); // 2100
```

#### .minus(values...)
**Type**: `Number|Numberer`

How to use this method
```javascript
var baz = new N(1000);
// 1000 - 500 + 100 - 200
baz.minus(500).plus(100).minus(200);
// 1000 - 500 + 100 - 200 - 100 + 1
var boss = N.minus(baz, 100).plus(1);
console.log(boss.get()); // 301
```

#### .div(values...)
**Type**: `Number|Numberer`

How to use this method
```javascript
var boss = new N(1024);
//1024 / 2 /4 /8 /16 - 1
boss.div(2).div(4).div(8).div(16).minus(1);
console.log(boss.get()); // 0
```

#### .mult(values...)
**Type**: `Number|Numberer`

How to use this method
```javascript
var boss = new N(1);
// (2 * 4 * 8 + 1 ) * 16 / 4 - 259
boss.mult(2).mult(4).mult(8).plus(1).mult(16).div(4).minus(259);
console.log(boss.get()); // 1
```

#### .mod(values...)
**Type**: `Number|Numberer`

How to use this method
```javascript
var boss = new N(107882345727419);//*
//(107882345727419 % 434 % 488 % 578 % 768 % 1150 - 17)/60*0.5

boss.mod(434).mod(488).mod(578).mod(768).mod(1150).minus(17).div(60).mult(0.5);
console.log(boss.get()); // 1

//* 107882345727419 = ((((433 * 487 + 1 ) * 577 + 1 ) * 769 + 1 ) * 1153 + 1 )
```


## Contributing

Please submit all issues and pull requests to the [safareli/numberer](http://github.com/safareli/numberer) repository!

## Support
If you have any problem or suggestion please open an issue [here](https://github.com/safareli/numberer/issues).

## License 

The MIT License

Copyright (c) 2014, Irakli Safareli

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
