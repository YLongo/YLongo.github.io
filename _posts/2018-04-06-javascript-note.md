---
layout: post
title: JavaScript 笔记
date: 2018-04-06 00:30
---

**《JavaScript 高级程序设计(第三版)》**

<!-- more -->

### 5.2.3 栈方法

`push()` 方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后的数组长长度。

`pop()` 方法从数据末尾移除最后一项，减少数组的 length 值，然后返回移除的项。

```javascript
var colors = new Array();
var count = colors.push("red", "green");
count = colors.pop();
```

### 5.2.4 队列方法

`shift()` 方法移除数组中的第一个项并返回该项，同时将数据长度减 1。

`unshift()` 在数组前端添加任意个项并返回新数组的长度。

```javascript
var color = new Array();
var count = colors.unshift("red", "green");
```

### 5.5.4 函数内部属性

arguments 对象有一个名叫 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。

```javascript
function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * factorial(num - 1);
    }
}
```

如上代码所示，在函数有名字，而且名字以后不会变的情况下，这样定义没有问题。但是这个函数的执行与函数名   factorial 紧紧耦合在一起，为了消除这种紧密耦合的现象，可以这样来使用 arguments.callee。

```javascript
function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num - 1);
    }
}
```

在这个重写的 factorial() 函数的函数体内，没有再引用函数名 factorial。所以，无论引用函数时使用的是什么名字，都可以保证正常的执行。

```javascript
var trueFactorial = factorial;
factorial = function() {
    return 0;
}
alert(trueFactorial(5)); // 120
alert(factorial(0)); // 0
```



### 6.2.3 原型模式

使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。

1. 理解原型对象

   在默认情况下，所有原型对象都会自动获得一个 constructor 属性，这个属性包含一个指向 prototype 属性所在函数的指针。

   > 例：
   >
   > Person.prototype.constructor 指向 Person

   使用 Object.getPrototypeOf() 可以方便的取得一个对象的原型。

   ```javascript
   alert(Object.getPrototypeOf(person1) == Person.prototype); // true
   alert(Object.getPrototypeOf(person1).name); // Volong
   ```


每当代码读取某个对象的某个属性时，都会执行一次搜索。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。如果在原型对象中找到了这个属性，则返回该属性的值。

可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。

   > 例：PrototypePatternExample02.html

   ```javascript
   function Person() {

   }

   Person.prototype.name = "Volong";
   Person.prototype.age = 29;
   Person.prototype.job = "Sorftware Engineer";
   Person.prototype.sayName = function() {
       alert(this.name);
   }

   var person1 = new Person();
   var person2 = new Person();

   person1.name = "yihailong";

   alert(person1.name); // "yihailong" - 来自实例
   alert(person2.name); // "Volong" - 来自原型
   ```

当为对象添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性。添加这个属性只是会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个属性设置为 null，也只会在实例中设置这个属性，而不会恢复指向原型的链接。但是，使用 delete 操作符可以完全删除实例属性，重新访问原型中的属性。

   ```javascript
   delete person1.name;
   alert(person1.name); // "Volong" - 来自原型
   ```

使用 `hasOwnProperty()`（从 `Object` 继承而来）方法可以检测一个属性是存在实例中，还是存在于原型中。只有给定的属性存在于对象实例中，才会返回 true。

   ```javascript
   alert(person1.hasOwnProperty("name"));

   person1.name = "yihailong";

   alert(person1.name); // "yihailong" - 来自实例

   alert(person1.hasOwnProperty("name"));

   alert(person2.name); // "Volong" - 来自原型

   alert(person2.hasOwnProperty("name"));
   delete person1.name;

   alert(person1.name); // "Volong" - 来自原型
   alert(person1.hasOwnProperty("name"));
   ```

2. 原型与 in操作符

   有两种方式使用 `in` 操作符：单独使用与在 for-in 循环中使用。

   在单独使用时，`in` 操作符会在通过对象能够访问给定属性时返回 true，无论该属性存在于实例中还是原型中。

   ```javascript
   alert("name" in person1);
   person1.name = "yihailong";
   alert("name" in person1);

   alert("name" in person2);

   delete person1.name;
   alert("name" in person1);
   ```

   在使用 for-in 循环时，返回的是所有能够通过对象访问的、可枚举的属性。

   要取得对象上所有可枚举的实例属性，可以使用 `Object.keys()` 方法。该方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。

   > 例：ObjectKeysExample01.html

   ```javascript
   function Person() {

   }

   Person.prototype.name = "Volong";
   Person.prototype.age = 29;
   Person.prototype.job = "Software Engineer";
   Person.prototype.sayName = function() {
       alert(this.name);
   }

   var keys = Object.keys(Person.prototype);
   alert(keys); // name,age,job,sayName

   var p1 = new Person();
   p1.name = "yihailong";
   p1.age = 18;
   var p1Keys = Object.keys(p1);
   alert(p1Keys); // name,age
   ```

   如果想要得到所有实例属性，无论是否可枚举，都可以使用 `Object.getOwnPropertyNames()` 方法。

   ```javascript
   var keys = Object.getOwnPropertyNames(Person.prototype);
   alert(keys); // constructor,name,age,job,sayName
   ```

3. 更简单的原型语法

   之前的例子中，没添加一个属性和方法就要敲一遍 Person.prototype。为了减少不必要的输入，可以用一个包含所有属性和方法的对象字面量来重写整个原型对象。

   > PrototypePatternExample07.html

   ```javascript
   function Person() {

   }

   Person.prototype = {
       name: "Volong",
       age: 29,
       job: "Software Engineer",
       sayName: function () {
           alert(this.name);
       }
   };
   ```

   但是这种做法会导致 constructor 属性不再指向 Person（指向 Object 构造函数）。此时，通过 `instanceof` 操作符可以返回正确的结果，但是通过 `constructor` 无法确定对象的类型。

   ```javascript
   var friend = new Person();

   alert(friend instanceof Object); // true
   alert(friend instanceof Person); // true

   alert(friend.constructor == Person); // false
   alert(friend.constructor == Object); // true
   ```

   如果 constructor 的值真的很重要，可以这样设置：

   ```javascript
   Person.prototype = {
       constructor: Person,
       name: "Volong",
       age: 29,
       job: "Software Engineer",
       sayName: function () {
           alert(this.name);
       }
   };
   ```

### 6.2.4 组合使用构造函数模式和原型模式

构造函数用于定义实例属性，而原型模式用于定义方法和共享的属性。每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度的节省了内存。

   > HybridPatternExample01.html

   ```javascript
   function Person(name, age, job) {
       this.name = name;
       this.age = age;
       this.job = job;
       this.friends = ["yi", "hai", "long"];
   }

   Person.prototype = {
       constructor: Person,
       sayName: function() {
           alert(this.name);
       }
   }

   var person1 = new Person("volong", 29, "Software Engineer");
   var person2 = new Person("yihailong", 18, "Doctor");

   person1.friends.push("Van");

   alert(person1.friends);
   alert(person2.friends);
   alert(person1.friends === person2.friends);
   alert(person1.sayName === person2.sayName);
   ```

   ### 6.2.5 动态原型模式

把所有信息都封装在构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。换句话说：可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

   > DynamicPrototypeExample01.html

   ```javascript
   function Person(name, age, job) {
       this.name = name;
       this.age = age;
       this.job = job;

       if (typeof this.sayName != "fucntion") {
           Person.prototype.sayName = function() {
               alert(this.name);
           };
       }
   }

   var friend = new Person("volong", 29, "Software Engineer");
   friend.sayName();	
   ```

使用动态原型模式时，不能使用对象字面量重写原型。如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。

   ### 6.2.6 寄生构造函数模式

创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象。

   > HybridFactoryPatternExample01.html

   ```javascript
   function Person(name, age, job) {
       var o = new Object();
       o.name = name;
       o.age = age;
       o.job = job;
       o.sayName = function() {
           alert(this.name);
       };
       return o;
   }

   var friend = new Person("Volong", 29, "Software Engineer");
   friend.sayName();
   ```

这个模式可以在特殊的情况下用来为对象创建构造函数。假设我们想创建一个具有额外方法的特殊数组。由于不能直接修改 Array 构造函数，因此可以使用这个模式。

   > HybridFactoryPatternExample02.html

   ```javascript
   function SpecialArray(argument) {
       var values = new Array();
       values.push.apply(values, arguments);
       values.toPipeString = function() {
           return this.join("|");
       };

       return values.;
   }

   var colors = new SpecialArray("red", "blue", "green");
   alert(colors.toPipeString());
   ```

### 6.3.1 原型链

   4. **原型链的问题**

      在通过原型来实现继承时，原型实际上会变成另一个类型的实例。于是，原先的实例属性会变成现在的原型属性。包含引用类型值的原型属性会被所有实例共享。

      > PrototypeChainingExample04.html

      ```javascript
      function SuperType() {
          this.colors = ["red", "blue", "green"];
      }

      function SubType() {

      }

      SubType.prototype = new SuperType();

      var instance1 = new SubType();
      instance1.colors.push("black");
      alert(instance1.colors); // red, blue, green, black

      var instance2 = new SubType();
      alert(instance2.colors); // red, blue, green, black
      ```

### 6.3.2 借用构造函数

可以解决原型中包含引用类型值所带来的问题。这种方法的基本思想是：在子类型构造函数的内部调用超类型构造函数。

   > ConstructorStealingExample01.html

   ```javascript
   function SuperType() {
       this.colors = ["red", "blue", "green"];
   }

   function SubType() {
       SuperType.call(this);
   }

   var instance1 = new SubType();
   instance1.colors.push("black");
   alert(instance1.colors); // red,blue,green,black

   var instance2 = new SubType();
   alert(instance2.colors); // red,blue,green
   ```

### 6.3.3 组合继承

使用原型链实现对原型属性和方法的继承，而通过构造函数来实现对实例属性的继承。既通过在原型上定义方法实现了函数复用，又能保证每个实例都有它自己的属性。

   > CombinationInheritanceExample01.html

   ```javascript
   function SuperType(name) {
       this.name = name;
       this.colors = ["red", "blue", "green"];
   }

   SuperType.prototype.sayName = fucntion() {
       alert(this.name);
   }

   function SubType(name, age) {
       SuperType.call(this, name);
       this.age = age;
   }

   SubType.prototype = new SuperType();

   SubType.prototype.sayAge = function() {
       alert(this.age);
   };

   var instance1 = new SubType("Volong", 29);
   instance1.colors.push("black");
   alert(instance1.colors); // red,blue,green,black

   instance1.sayName(); // Volong 
   instance1.sayAge(); // 29

   var instance2 = new SubType("yihailong", 27);
   alert(instance2.colors); // red,blue,green
   instance2.sayName(); // yihailong
   instance2.sayAge(); // 27
   ```

# 第 7 章 函数表达式   

定义函数的方式有两种：一种是函数声明，一种是函数表达式。

函数声明的一个重要特征是：函数声明提升。即在执行代码之前会先读取函数声明，也就是说可以把函数声明放在调用它的语句后面。

   ```javascript
   sayHi();
   function sayHi() {
       alert("Hi");
   }
   ```

函数表达式最常见的形式为：

   ```javascript
   var functionName = function(arg0, arg1, arg2) {
       // 函数体
   }
   ```

这种方式创建的函数叫做匿名函数。既然叫做函数表达式，那么与其他表达式一样，在使用前必须先赋值。

能够创建函数再赋值给变量，也就可以把函数作为其它函数的返回值返回。

   ```javascript
   function createComparisonFunction(propertyName) {
       return function(object1, object2) {
           var value1 = object1[propertyName];
           var value2 = object2[propertyName];
           
           return value1 - value2;
       };
   }
   ```


## 7.2 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。

### 7.2.1 闭包与变量

闭包只能取得包含函数中任何变量的最后一个值。因为闭包所保存的是整个变量对象，而不是某个特殊的变量。

> ClosureExample01.html

```javascript
function createFunctions() {
    var result = new Array();

    for (var i = 0; i < 10; i++) {
        result[i] = function() {
            return i;
        };
    }
    return result;
}

var s = createFunctions();
for (var i = 0; i < s.length; i++) {
    alert(s[i]());
}
```

在这个例子中，这个函数会返回一个函数数组，但是每个函数都会返回 10，因为每个函数的作用域链中都保存着 createFunctions() 函数的活动对象，所以它们引用的都是同一个变量 i。当 createFunctions() 函数返回后，变量 i 的值是 10，此时每个函数都引用着保存变量 i 的同一个变量对象，所以在每个函数内部 i 的值都是 10。

但是我们可以通过创建另一个匿名函数强制让闭包的行为符合预期：

> ClosureExample02.html

```java
function createFunctions() {
    var result = new Array();

    for (var i = 0; i < 10; i++) {
        result[i] = function(num) {
            return function() {
                return num;
            };
        }(i);
    }

    return result;
}

var s = createFunctions();
for (var i = 0; i < s.length; i++) {
    console.log(s[i]());
}
```

在这个例子中，定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组。这里的匿名函数有一个参数 num，也就是最终的函数要返回的值。

## 7.3 模仿块级作用域

JavaScript 没有块级作用域的概念，也就是说在块语句中定义的变量，实际上是在包含函数中而非语句中创建的。

```javascript
function outputNumbers(count) {
    for (var i = 0; i < count; i++) {
        alert(i);
    }
    alert(i);
}
```

如上所示，变量 i 定义在 outputNumbers() 的活动对象中，因此从它有定义开始，就可以在函数内部随处访问它。即使像下面这样错误地重新声明同一个变量，也不会改变它的值。

```javascript
function outputNumbers(count) {
    for (var i = 0; i < count; i++) {
        alert(i);
    }
    var i;
    alert(i);
}
```
JavaScript 不会告诉你是否多次声明了同一个变量。遇到这种情况时，它只会对后续的声明视而不见，但是，它会执行后续声明中的变量初始化。

匿名函数可以用来模仿块级作用域并避免这个问题。

块级作用域（私有作用域）的匿名函数语法如下：

```javascript
(function() {
    // 这里是块级作用域
})();
```

上述代码定义并立即调用了一个匿名函数。将函数声明包含在一对圆括号中，表示它实际上是一个函数表达式。而紧随其后的另一对圆括号会立即调用这个函数。

### 8.1.1 全局作用域

所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法。

```javascript
var age = 29;
function sayAge() {
    alert(this.age);
}
alert(window.age); // 29
sayAge(); // 29
window.sayAge(); // 29
```

定义全局变量与在 window 对象上直接定义属性的差别是：全局变量不能通过 delete 操作符删除，而直接在 window 对象上定义的属性可以。

```javascript
var age = 29;
window.color = "red";

// 在 IE < 9 时抛出错误，在其它所有浏览器中都返回 false
delete window.age; // return false

// 在 IE < 9 时抛出错误，在其它所有浏览器中都返回 true
delete window.color; // return true

alert(window.age); // 29
alert(window.color); // underfined
```

尝试访问未声明的变量会抛出错误，但是通过查询 window 对象，可以知道某个可能未声明的变量是否存在。

```javascript
// 这里会抛出错误，因为 oldValue 未定义
var newValue = oldValue;

// 这里不会抛出错误，因为这是一次属性查询
// newValue 的值是 undefined
var newValue = window.oldValue;
```



