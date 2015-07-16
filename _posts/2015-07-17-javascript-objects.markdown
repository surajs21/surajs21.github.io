---
layout:     post
title:      "JavaScript Objects"
subtitle:   "The big words on JS Objects"
date:       2015-07-17 12:00:00
author:     "Suraj"
header-img: "img/747.jpg"
---

{% highlight javascript %}

var o;

// Both are identical
o = {};
o = Object.create(Object.prototype);
// Object.create creates an object with prototoype as Object.prototype.
// Its same as o.__proto__ = Object.prototype
// PS: typeof o.prototype === 'undefined' // true

o = Object.create(null);
// creates an object with null as its prototype

// Creating Objects with some properties.
o = Object.create(Object.prototype, {
	someProperty : {
		value: 12,
		enumerable: false, // should be shown in for..in loop or not
		writable: false,
		configurable: false,
		set: function(val){},
		get: function(){ return 10;}
	}
})


// creating objects using Constructor functions
function SomeConstructorFn (){
	this.someProperty = 12;
};

SomeConstructorFn.prototype.someFunction = function(){
	console.log('ping');
};

// PS: Every function has a prototype 'property'. Best practise is to add only 
// the behaviours i.e. functions to a function's prototype.
// Doing this has one more benefit, all these behavious will be created once in 
// memory and will be shared among all the child objects.

o = new SomeConstructorFn();
o = Object.create(SomeConstructorFn.prototype);
// but this will not have any initializations done in the constructor function
// typeof o.someProperty === 'undefined' // true
// typeof o.someFunction === 'undefined' // false

// so to fix this and get all the declerations made in SomeConstructorFn -
SomeConstructorFn.call(o);
// typeof o.someProperty === 'undefined' // false
// o.someProperty // 12



// Inheritence - Vanilla Flavour 

// Child: Foo
function Foo() {

}

Foo.prototype = new SomeConstructorFn();

// Inheritence - Strawberry Flavour 

function Foo() {
	// call super constructor to get all the initializations made 
	// in SomeConstructorFn function.
	SomeConstructorFn.call(this); 
}

Foo.prototype = Object.create(SomeConstructorFn.prototype);

// Both the above approach has one small glitch. Their constructor property 
// is wrongly set.
o = new Foo();
o.constructor // SomeConstructorFn

// So to fix it, 
Foo.prototype.constructor = Foo;


// PS: The instanceof operator tests presence of constructor.prototype 
// in object's prototype 'chain'.

// true: Object.getPrototypeOf(o) === Foo.prototype
o instanceof Foo // true
// true: Object.getPrototypeOf(Foo.prototype) === SomeConstructorFn.prototype
o instanceof SomeConstructorFn // true
// true: Object.getPrototypeOf(SomeConstructorFn.prototype) === Object.prototype
o instanceof Object // true


// Inheritence - Orange Flavour
// By using node's util.inherits

// How util.inherit works??
// It inherits the prototype methods from one constructor into another
exports.inherits = function(ctor, superCtor) {
  // As an additional convenience, superConstructor will be accessible
  // through the constructor.super_ property.
  ctor.super_ = superCtor; 
  ctor.prototype = Object.create(superCtor.prototype, {
    constructor: {
      value: ctor,
      enumerable: false,
      writable: true,
      configurable: true
    }
  });
};

function Foo(){
	// add all the initializations done in SomeConstructorFn; namely someProperty
	SomeConstructorFn.call(this); 
}

 // copies all the behaviours-/ethods/functions from SomeConstructorFn
 // constructos's prototype; namely someFunction
util.inherits(Foo, SomeConstructorFn);

o = new Foo();
o.constructor // Foo


// A word on Mixins
// To inherit from multiple objects, mixins are a possibility.
// The mixin function would copy the functions from the superclass prototype to
//  the subclass prototype,
// the mixin function needs to be supplied by the user. An example of a mixin 
//  like function would be jQuery.extend()

// Why not to use Mixins



// References - 
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof
// https://developer.mozilla.org/en-US/docs/Glossary/Mixin
{% endhighlight %}