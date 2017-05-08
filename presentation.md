# Sweetening Objects

Demystifying Class Syntax Sugar



## Who I Am

* <!-- .element: class="fragment" data-fragment-index="1" --> James Kruth - owner of [Ars Mentis](https://www.arsmentis.com) Custom Software and IT Consulting
* Hesitantly starting using JavaScript (ES3) in 2001 <!-- .element: class="fragment" data-fragment-index="2" -->
* Determined to understand how the magic works! <!-- .element: class="fragment" data-fragment-index="3" -->



## JavaScript Objects in 1 Minute or Less


## In the beginning, there were prototypes!

* ...and no one understood them. <!-- .element: class="fragment" data-fragment-index="1" -->
* Nearly every mainstream language uses classes because Smalltalk (and C++ and Java) won <!-- .element: class="fragment" data-fragment-index="2" -->
* JavaScript is based on Self (prototypal Smalltalk)  <!-- .element: class="fragment" data-fragment-index="3" -->


![XKCD Sandwich](img/sandwich.png)


## Prototypes vs Classes (sandwich style!)

* Prototypes: Here's a sandwich - make me one just like it <!-- .element: class="fragment" data-fragment-index="1" -->
* Classes: Here's a recipe for a sandwich - do what it says <!-- .element: class="fragment" data-fragment-index="2" -->
* Everything else is (mostly) the same <!-- .element: class="fragment" data-fragment-index="3" -->


## Warning: Opinions Incoming


## The Problem

* Prototypes aren't the problem <!-- .element: class="fragment" data-fragment-index="1" -->
* Lack of language infrastructure is the problem <!-- .element: class="fragment" data-fragment-index="2" -->
* Lots of power - no direction <!-- .element: class="fragment" data-fragment-index="3" -->


## With great power comes great responsiblity

* Too many ways to do simple things <!-- .element: class="fragment" data-fragment-index="1" -->
* There are literally hundreds (go ahead, search) of different takes on pre-ES2015 JS inheritance patterns <!-- .element: class="fragment" data-fragment-index="2" -->



## The Solution: ES2015 Classes

```
class Foo { }

class Bar extends Foo { }

b = new Bar()
```

Suddenly the world makes sense again! <!-- .element: class="fragment" data-fragment-index="1" -->


## But how does this work?

* It's just syntactical sugar! No new language features (mostly)! <!-- .element: class="fragment" data-fragment-index="1" -->
* Old objects remain compatible <!-- .element: class="fragment" data-fragment-index="2" -->


## An ES5-style "Class"

```
var Point = function (x, y) {
  this.x = x;
  this.y = y;
};

Point.prototype.toString = function () {
  return "(" + this.x + ", " + this.y + ")";
};
```


## ...and in ES2015

```
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return `(${this.x}, ${this.y})`;
    }
}
```


## What does Babel.js say?


```
var Point = function () {
    function Point(x, y) {
        _classCallCheck(this, Point);
        this.x = x;
        this.y = y;
    }
    _createClass(Point, [{
        key: "toString",
        value: function toString() {
            return "(" + this.x + ", " + this.y + ")";
        }
    }]);
    return Point;
}();
```


## Let's sort that out a bit...

* <!-- .element: class="fragment" data-fragment-index="1" --> Despite some clever helper functions (`_createClass`, I'm looking at you), the output is very similar
* <!-- .element: class="fragment" data-fragment-index="2" --> `Point` ends up being a function that constructs a new object
* <!-- .element: class="fragment" data-fragment-index="3" --> `_classCallCheck` simply makes sure the function is invoked with `new`
* <!-- .element: class="fragment" data-fragment-index="4" --> `_createClass` assigns the properties passed to the constructor function's prototype


## But what about inheritance?


## ES5-style inheritance

```
var Point3d = function (x, y, z) {
  Point.call(this, x, y);
  this.z = z;
};

Point3d.prototype = Object.create(Point.prototype);
```


## Wat?

* That's just one way to do it - there are lots more! <!-- .element: class="fragment" data-fragment-index="1" -->
* <!-- .element: class="fragment" data-fragment-index="2" --> That way depends on an ES5.1 feature (`Object.create`)
* <!-- .element: class="fragment" data-fragment-index="3" --> Does *anyone* find this code to be obvious?


## ES2015 inheritance

```
class Point3d extends Point {
  constructor(x, y, z) {
    super(x, y);
    this.z = z;
  }
}
```


## But what about Babel.js?

```
var Point3d = function (_Point) {
    _inherits(Point3d, _Point);

    function Point3d(x, y, z) {
        _classCallCheck(this, Point3d);

        var _this2 = _possibleConstructorReturn(this, (Point3d.__proto__ || Object.getPrototypeOf(Point3d)).call(this, x, y));

        _this2.z = z;
        return _this2;
    }

    return Point3d;
}(Point);
```


## Simpler than it looks

* <!-- .element: class="fragment" data-fragment-index="1" --> `_inherits` is just bulletproof version of our ES5 call to `Object.create`
* <!-- .element: class="fragment" data-fragment-index="2" --> `_possibleConstructorReturn` is an ultra compatible way of calling the parent's constructor
* <!-- .element: class="fragment" data-fragment-index="3" --> The rest matches the last example



## Why Classes are a Good Thing(tm)

* Languages need idioms <!-- .element: class="fragment" data-fragment-index="1" -->
* Now there's one obvious way to do it (thanks Python!) <!-- .element: class="fragment" data-fragment-index="2" -->
* I can stop worrying about object creation and work on more interesting things <!-- .element: class="fragment" data-fragment-index="3" -->


## Disadvantages, or nothing's perfect

* Classes hide the fact that it's prototypes all the way down <!-- .element: class="fragment" data-fragment-index="1" -->
* Single inheritance only (this might be a feature) <!-- .element: class="fragment" data-fragment-index="2" -->
* <!-- .element: class="fragment" data-fragment-index="3" --> Mandatory `new`



# Questions?



# Thanks!

Some handy references:

* [Exploring ES6](http://exploringjs.com/es6/ch_classes.html)
* [BabelJS](http://babeljs.io)
* [ECMAScript Standard](https://tc39.github.io/ecma262/#sec-class-definitions)
