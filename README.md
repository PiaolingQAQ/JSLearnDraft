

# JavaScript学习笔记

## Chapter 6 对象

### 6.1 Introduction to Objects

​	1、对象是一个复合值，并允许按照名称存储和获取这些值。属性名通常是字符串，对象将字符串映射到值。

​	2、对象除了维护自己的属性，对象还继承另一个对象的属性，即他的“原型”。对象的方法通常是继承的属性。

​	3、j s的对象属性可以动态添加和删除。

​	4、JavaScript 中任何不是字符串、数字、Symbol 或 true、false、null 或 undefined 的值都是对象。

​	5、除了名称之外，每个对象属性还有三个属性属性：

- The writable attribute specifies whether the value of the property can be set.
- The enumerable attribute specifies whether the property name is returned by a for/in loop.
- The configurable attribute specifies whether the property can be deleted and whether its attributes can be altered.

------

- writable 属性指定是否可以设置属性的值。
- enumerable 属性指定 for/in 循环是否返回属性名称。
- 可配置属性指定该属性是否可以删除，是否可以修改其属性。

### 6.2 创建对象

​	对象可以用对象字面量【object literals】创建，也可以用new和Object.create()创建

```javascript
let empty = {};                          // An object with no properties
let point = { x: 0, y: 0 };              // Two numeric properties
let p2 = { x: point.x, y: point.y+1 };   // More complex values
let book = {
    "main title": "JavaScript",          // These property names include spaces,
    "sub-title": "The Definitive Guide", // and hyphens, so use string literals.
    for: "all audiences",                // for is reserved, but no quotes.
    author: {                            // The value of this property is
        firstname: "David",              // itself an object.
        surname: "Flanagan"
    }
};
```



#### 6.2.1 对象字面量【object literals】

​	尾随逗号是合法的。

#### 6.2.2 new关键字

​	let object = new class()

#### 6.2.3 原型【Prototypes】

​	1、j s对象都和另一个对象相关联。每一个对象都从原型继承属性。

​	2、所有通过对象字面量创建的对象都具有同一个原型对象，并可以通过 JavaScript 代码 Object.prototype 获得对原型对象的引用。通过关键字 new 和构造函数调用创建的对象的原型就是构造函数的 prototype 属性的值。

​	3、Object.prototype没有原型，不继承任何属性。一系列的继承关系形成了一条原型链。

#### 6.2.4 Object.create(0)

​	1、Object.create() 创建一个新的对象，用第一个实参作为它的原型：

```javascript
let o1 = Object.create({x: 1, y: 2});     // o1 inherits properties x and y.
o1.x + o1.y                               // => 3
```

​	2、可以传参null。创建出的对象不会继承任何东西。

​	3、使用如下代码创建新对象：

```javascript
let o3 = Object.create(Object.prototype); // o3 is like {} or new Object().
```

​	4、可以通过任意原型创建对象。

### 6.3 查询并设置属性

​	1、可以通过 '.' 和 '[ ]' 来对对象属性进行获取和复制。中括号内为计算结果为字符串的表达式。

#### 6.3.1 对象作为关联数组

​	1、js对象形式类似于哈希散列或字典。

​	2、js对象属性不需要提前规定好。可以在访问对象时动态添加。

​	3、使用中括号访问对象的属性时，字符串可以进行修改，如下：

```javascript
let addr = "";
for(let i = 0; i < 4; i++) {
    addr += customer[`address${i}`] + "\n";
} //读取address1、address2、address3、address4并链接
```

​	4、可以使用for/in循环，使用一个存放了对象属性名的字符串数组来对对象的属性进行遍历。

#### 6.3.2 继承

​	1、原型继承链：

```javascript
let o = {};               // o inherits object methods from Object.prototype
o.x = 1;                  // and it now has an own property x.
let p = Object.create(o); // p inherits properties from o and Object.prototype
p.y = 2;                  // and has an own property y.
let q = Object.create(p); // q inherits properties from p, o, and...
q.z = 3;                  // ...Object.prototype and has an own property z.
let f = q.toString();     // toString is inherited from Object.prototype
q.x + q.y                 // => 3; x and y are inherited from o and p

```

​	q<==p<==o

​	2、属性赋值操作检查原型链只是判断是否允许赋值操作。例如，如果 o 继承自一个只读属性 x，那么赋值操作是不允许的。 如果允许属性赋值操作，它也总是在原始对象上创建属性或对已有的属性赋值，而不会去修改原型链。在 JavaScript 中，只有在查询属性时才会体会到继承的存在，而设置属性则和继承无关。

​	3、属性赋值要么失败，要么创建一个属性，要么在原始对象中设置属性。但有一个例外，如果 o 继承自属性 x，而这个属性是一个具有 setter 方法的存取器属性，那么这时将调用 setter 方法而不是给 o 创建一个属性 x。需要注意的是，setter 方法是由对象 o 调用的，而不是定义这个属性的原型对象调用的。因此如果 setter 方法定义任意属性，这个操作只是针对 o 本身，并不会修改原型链。

#### 6.3.3 属性访问错误

​	1、查询不存在的属性并不会报错，而是会返回undefined。

​	2、如果查询不存在的对象的属性会报错。

​	3、ES2020支持:

```javascript
let surname = book?.author?.surname;
```

​	4、有些对象不允许新增属性。有些属性是只读的。在严格模式下会报错，否则通常没有任何反馈。

### 6.4 删除属性

​	1、删除运算符可以删除对象中的属性，操作的是属性的属性：

```javascript
delete book.author;          // The book object now has no author property.
delete book["main title"];   // Now it doesn't have "main title", either.
```

​	2、delete运算符只删除自身属性，不删除继承属性。

​	3、删除成功delete返回true。作用于无用代码时也会返回true

​	4、delete 不能删除那些可配置性为 false 的属性。某些内置对象的属性是不可配置的，比如通过变量声明和函数声明创建的全局对象的属性。在严格模式中，删除一个不可配置属性会报一个类型错误。在非严格模式中，在这些情况下的 delete 操作会返回 false。

​	5、当在非严格模式中删除全局对象的可配值属性时，可以省略对全局对象的引用，直接在 delete 操作符后跟随要删除的属性名即可。

### 6.5 检测属性

​	1、可以用 in 运算符、hasOwnPreperty() 和 propertyIsEnumerable() 方法来判断某个属性是否存在于某个对象中。

```javascript
let o = { x: 1 };
"x" in o         // => true: o has an own property "x"
"y" in o         // => false: o doesn't have a property "y"
"toString" in o  // => true: o inherits a toString property
```

​	2、hasOwnPreperty()判断是否是对象的自有属性。如果是返回true

​	3、propertyIsEnumerable() 是 hasOwnProperty() 的增强版。只有检测到是自有属性且这个属性的可枚举性为 true 时它才返回 true。某些内置属性是不可枚举的。通常由 JavaScript 代码创建的属性都是可枚举的。

### 6.6 枚举属性

​	1、 for/in 循环可以在循环体中遍历指定对象中所有可枚举的属性（包括自有属性和继承的属性），把属性名称赋值给循环变量。对象继承的 内置方法不可枚举的，但在代码中给对象添加的属性都是可枚举的。例如：

```javascript
let o = {x: 1, y: 2, z: 3};          // Three enumerable own properties
o.propertyIsEnumerable("toString")   // => false: not enumerable
for(let p in o) {                    // Loop through the properties
    console.log(p);                  // Prints x, y, and z, but not toString
}
```

​	2、为了防止for/in枚举到继承属性，可以添加检查：

```javascript
for(let p in o) {
    if (!o.hasOwnProperty(p)) continue;       // Skip inherited properties
}

for(let p in o) {
    if (typeof o[p] === "function") continue; // Skip all methods
}
```

​	4、作为使用 for/in 循环的替代方法，通常使用 for/of 循环遍历易获取对象的属性名称数组。可以使用四个函数获取属性名称数组：

​		（1）Object.keys() 返回对象的可枚举自有属性名称数组集合。数组内不包含不可枚举属性、继承属性或属性名称是 Symbol的属性

​		（2）Object.getOwnPropertyNames() 用起来和 Object.keys() 类似，但是它返回数组中也包含不可迭代的自有属性，只要它们的名称是字符串。

​		（3）Object.getOwnPropertySymbols() 返回名称是 Symbol 的自有属性，无论它们是否可枚举

​		（4）Reflect.ownKeys() 返回所有的自由属性名称，包括可枚举和不可枚举类型，也包括字符串和 Symbol

#### 6.6.1 属性枚举次序

​	首先列出名称为非负整数的字符串属性，按从最小到最大的数字顺序列出。此规则意味着数组和数组类对象将按顺序枚举其属性。列出所有看起来像数组索引的属性后，将列出所有具有字符串名称的剩余属性（包括看起来像负数或浮点数字的属性）。这些属性按添加到对象的顺序列出。对于在对象字面量中定义的属性，此顺序与它们在文本中显示的顺序相同。最后，其名称为 Symbol 对象的属性按添加到对象的顺序列出。for/in 循环的枚举顺序不像枚举函数那样严格指定，但实现通常按刚才描述的顺序枚举自己的属性，然后向上移动原型链按相同顺序枚举每个原型对象的属性。但是请注意，如果已枚举具有相同名称的属性，或者不可枚举属性已经被检测过再次枚举到相同名称的属性，都不会再次枚举。

### 6.7 Extending Objects

​	1、Object.assign() 需要两个或多个对象作为其实参。它修改并返回第一个实参，即目标对象，但不会改变第二个或任何后续参数，这些参数是源对象。对于每个源对象，它将该对象的可枚举自有属性（包括名称为 Symbol 的属性）复制到目标对象中。它按源对象在实参列表顺序中的顺序处理，所以第一个源对象中的属性会重写在目标对象中的同名属性，然后以第二个源对象中的同名属性（如果有第二个源对象）再次重写第一个源对象重写后的属性。

​	2、Object.assign() 通过普通属性的 get 和 set 操作复制属性，因此，如果源对象具有 getter 方法或目标对象具有 setter 方法，则将在复制期间调用它们，但不会复制方法本身。

### 6.8 对象序列化

​	1、对象序列化（serialization）是指将对象的状态转换为字符串，也可将字符串还原为对象。函数 JSON.stringify() 和 JSON.parse() 用来序列化和还原 JavaScript 对象。这些方法都使用 JSON 作为数据交换格式，JSON 的全称是“JavaScript Object Notation”——JavaScript 对象表示法。

### 6.9  对象方法

#### 6.9.1 toString()

​	1、返回一个表示调用这个方法的对象值的字符串。使用+连接对象和字符串以及希望使用字符串的方法中使用对象会调用同String()。

​	2、默认的 toString() 方法的返回值带有的信息量很少，例如：

```javascript
let s = { x: 1, y: 1 }.toString();  // s == "[object Object]"
```

​	3、很多类带有自定义的toString()。比如：

```javascript
let point = {
    x: 1,
    y: 2,
    toString: function() { return `(${this.x}, ${this.y})`; }
};
String(point)    // => "(1, 2)": toString() is used for string conversions

```

#### 6.9.2 toLocaleString()

​	1、除了toString()方法，对象都包含toLocaleString()方法，这个方法返回一个表示这个对象的本地化字符串。如：

```javascript
let point = {
    x: 1000,
    y: 2000,
    toString: function() { return `(${this.x}, ${this.y})`; },
    toLocaleString: function() {
        return `(${this.x.toLocaleString()}, ${this.y.toLocaleString()})`;
    }
};
point.toString()        // => "(1000, 2000)"
point.toLocaleString()  // => "(1,000, 2,000)": note thousands separators

```

​	2、本方法在国际化中十分有用。

#### 6.9.3 valueOf()

​	alueOf() 方法和 toString() 方法非常类似，但往往当 JavaScript 需要将对象转换为某种原始值而非字符串的时候才会调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，JavaScript 就会自动调用这个方法。默认的 valueOf() 方法不足为奇，但有些内置类自定义了 valueOf() 方法. Date 类定义 valueOf() 将日期转化成数值型，并且这允许 Date 对象使用 < 和 > 按时间先手顺序比较。

#### 6.9.4 toJSON()

​	1、Object.prototype没有定义 toJSON() 方法，但对于需要执行序列化的对象来说，JSON.stringify() 方法）会调用 toJSON() 方法。如果在待序列化的对象中存在这 个方法，则调用它，返回值即是序列化的结果，而不是原始的对象：

```javascript
let point = {
    x: 1,
    y: 2,
    toString: function() { return `(${this.x}, ${this.y})`; },
    toJSON: function() { return this.toString(); }
};
JSON.stringify([point])   // => '["(1, 2)"]'

```

### 6.10 Extended Object Literal Syntax

#### 6.10.1 Shorthand Properties

​	1、假设值存储在变量 x 和 y 中，并且想要创建具有名为 x 和 y 的属性的对象，这些属性包含这些值。使用基本对象字面量语法，最终会重复每个标识符两次：

```javascript
let x = 1, y = 2;
let o = {
    x: x,
    y: y
};
```

​	2、ES6之后可以改为：

```javascript
let x = 1, y = 2;
let o = { x, y };
o.x + o.y  // => 3
```

#### 6.10.2 Computed Property Names

​	1、当对象需要的属性名称储存在变量中时，需要实例化后再添加，而ES6之后可以直接指定变量为属性名：

```javascript
const PROPERTY_NAME = "p1";
function computePropertyName() { return "p" + 2; }

let p = {
    [PROPERTY_NAME]: 1,
    [computePropertyName()]: 2
};

p.p1 + p.p2 // => 3
```

#### 6.10.3 将symbol作为属性名

```javascript
const extension = Symbol("my extension symbol");
let o = {
    [extension]: { /* extension data stored in this object */ }
};
o[extension].x = 0; // This won't conflict with other properties of o
```

#### 6.10.4 展开运算符

​	1、在 ES2018 之后，可以使用展开运算符 … 将现有的对象中的属性复制到新的对象中：

```javascript
let position = { x: 0, y: 0 };
let dimensions = { width: 100, height: 75 };
let rect = { ...position, ...dimensions };
rect.x + rect.y + rect.width + rect.height // => 175
```

​	2、如果展开的对象和源对象中有相同的名称，则该属性的值是位于后面的值。

#### 6.10.5 Shorthand Methods

​	1、在 ES6 中，对象字面量语法已扩展成允许省略函数关键字和冒号的快捷方式，可以写成这样的代码：

```javascript
let square = {
    area() { return this.side * this.side; },
    side: 10
};
square.area() // => 100

```

#### 6.10.6 Property Getters and Setters

​	1、JavaScript 还支持存取器属性，这些属性没有单个值，而是具有一个或两个存取器方法：getter 和 setter。

​	2、当程序查询存取器属性的值时，JavaScript 调用 getter 方法（无实参）。这个方法的返回值就是属性存取表达式的值。当程序设置一个存取器属性的值时， JavaScript 调用 setter 方法，将赋值表达式右侧的值当做参数传入 setter。从某种意义上讲，这个方法负责“设置”属性值。可以忽略 setter 方法的返回值。

​	3、如果属性同时有 getter 和 setter 方法，则它是一个可读写属性。如果它只含有 getter 方法，它是一个只读属性。如果它只有 setter 方法，它是一个只可写属性（这对一个数据属性来说是不可能的），如果尝试去读它，计算结果永远是 undefined。

​	4、存储器属性可以通过表达式在对象字面量语法中定义：

```javascript
let o = {
    // An ordinary data property
    dataProp: value,

    // An accessor property defined as a pair of functions.
    get accessorProp() { return this.dataProp; },
    set accessorProp(value) { this.dataProp = value; }
};

```

​	5、存储器属性定义为名称与属性名称相同的一个或两个方法。使用 get 或 set 前缀。

​	6、存储器属性可以被继承。

## Chapter 7 数组

​	1、数组继承自 Array.prototype 中的属性，它定义了一套丰富的数组操作方法。

### 7.1 创建数组

​	1、数组字面量

​	2、可迭代数组 … 展开运算符

​	3、Array() 构造函数

​	4、Array.of() 和 Array.from() 工厂方法

#### 7.1.1 数组字面量

```javascript
let empty = [];                 // An array with no elements
let primes = [2, 3, 5, 7, 11];  // An array with 5 numeric elements
let misc = [ 1.1, true, "a", ]; // 3 elements of various types + trailing comma
```

#### 7.1.2 展开运算符

​	ES6之后：

```javascript
let a = [1, 2, 3];
let b = [0, ...a, 4];  // b == [0, 1, 2, 3, 4]
```

​	2、展开运算符可以作用于任何可迭代对象。字符串是可迭代对象，所以可以使用展开操作符将字符串转换成单个字符的数组。

```javascript
let digits = [..."0123456789ABCDEF"];
digits // => ["0","1","2","3","4","5","6","7","8","9","A","B","C","D","E","F"]

```

#### 7.1.3 Array()构造函数

```javascript
let a = new Array();
let a = new Array(10);
let a = new Array(5, 4, 3, 2, 1, "testing, testing");
```

#### 7.1.4 Array.of()

​	1、当 Array() 构造函数调用时有一个数值型实参，它会将实参作为数组的长度。但当调用时不止一个数值型实参时，它会将那些实参作为数组的元素创建。这意味着 Array() 构造函数不能创建只有一个数值型元素的数组。

​	2、在 ES6 中，Array.of() 函数修复了这个问题：它是一个将其实参值（无论有多少个实参）作为数组元素创建并返回一个新数组的工厂方法。

#### 7.1.5 Array.from()

​	1、Array.from 是 ES6 中另外一个数组工厂方法。它期望一个可迭代或类数组对象作为它的第一个实参，并返回一个包含对象中元素的新数组。使用一个可迭代实参，Array.from(iterable) 工作方式类似于展开运算符 […iterable]。它也可以简单的拷贝一个数组。

### 7.2 读写数组元素

​	1、使用 [] 运算符来访问数组中的一个元素。

​	2、数组会自动维护length属性，也就是说可以动态添加。

​	3、数组是对象的特殊形式。使用方括号访问数组元素就像用方括号访问对象的属性一样。

​	4、JavaScript 数组没有“越界”错误的概念。当试图查询任何对象中不存在的属性时，都不会报错，只会得到 undefined 值。

### 7.3 稀疏数组

​	1、稀疏数组就是包含从 0 开始的不连续索引的数组。通常，数组的 length 属性值代表数组中元素的个数。如果数组是稀疏的，length 属性值大于元素的个数。

​	2、注意，当在数组字面量中省略值时（像 [1,,3] 中使用重复的逗号）返回的是稀疏数组，省略掉的值是不存在的：

```javascript
let a1 = [,];           // This array has no elements and length 1
let a2 = [undefined];   // This array has one undefined element
0 in a1                 // => false: a1 has no element with index 0
0 in a2                 // => true: a2 has the undefined value at index 0
```

### 7.4 数组长度

​	1、数组有length属性使其区别于常规的对象。

### 7.5 添加和删除数组元素

​	1、方括号的形式赋值。

​	2、push()方法在数组末尾增加一个或多个元素。

​	3、可以使用delete运算符来删除数组元素：

```javascript
let a = [1,2,3];
delete a[2];   // a now has no element at index 2
2 in a         // => false: no array index 2 is defined
a.length       // => 3: delete does not affect array length
```

​	4、删除元素不会缩减空间，也不会变换其他元素的位置。如果从稠密数组中删除一个元素，他就变成了稀疏数组。

​	5、可以通过设计新的数组长度从数组尾部删除元素。

### 7.6 遍历数组

​	1、for/of循环，对于稀疏数组不存在的元素返回undefined。

```javascript
let letters = [..."Hello world"];  // An array of letters
let string = "";
for(let letter of letters) {
    string += letter;
}
string  // => "Hello world"; we reassembled the original text

```

​	2、forEach()提供数组遍历功能方法的数组方法。可以给数组的 forEach() 方法传递一个函数，forEach() 会对数组中每一个元素调用这个方法：

```javascript
let uppercase = "";
letters.forEach(letter => {  // Note arrow function syntax here
    uppercase += letter.toUpperCase();
});
uppercase  // => "HELLO WORLD"
```

​	3、forEach() 按顺序对数组进行计算，它将数组索引作为第二个实参传递到函数。与 for/of 循环不同，forEach() 能意识到稀疏数组，并且不会为不存在的元素调用函数。

### 7.7 多维数组

​	1、JavaScript不支持多维数组，可以用数组的数组来进行代替。

### 7.8  数组方法

#### 7.8.1 数组迭代器方法

​	1、map() map方法将调用数组的每个元素传递到定义好的函数，并返回一个函数返回值的数组：

```javascript
let a = [1, 2, 3];
a.map(x => x*x)   // => [1, 4, 9]: the function takes input x and returns x*x
```

​	2、filter() 返回一个数组，数组是满足参数条件的原数组元素的新数组：

```javascript
let a = [5, 4, 3, 2, 1];
a.filter(x => x < 3)         // => [2, 1]; values less than 3
a.filter((x,i) => i%2 === 0) // => [5, 3, 1]; every other value
```

​	3、filter会跳过稀疏数组的空元素并且返回的数组总是稠密数组。可以通过以下方法缩小稀疏数组的间距：

```javascript
let dense = sparse.filter(() => true);
a = a.filter(x => x !== undefined && x !== null);
```

​	4、find() 和 findIndex() 这两个方法在断言首次查找到元素时停止便利。find()返回元素，findIndex()返回元素的索引。

​	5、every()和some() every() 和 some() 方法是数组断言：它们将指定的断言函数应用于数组的元素，然后返回 true 或 false：

```javascript
let a = [1,2,3,4,5];
a.every(x => x < 10)      // => true: all values are < 10.
a.every(x => x % 2 === 0) // => false: not all values are even.
```

​	6、reduce() 和 reduceRight() reduce()需要两个实参，第一个是执行化简操作的函数。第二个是一个传递给函数的初始值：

```javascript
let a = [1,2,3,4,5];
a.reduce((x,y) => x+y, 0)          // => 15; the sum of the values
a.reduce((x,y) => x*y, 1)          // => 120; the product of the values
a.reduce((x,y) => (x > y) ? x : y) // => 5; the largest of the values
```

​	7、reduceRight() 的工作原理和 reduce() 一样，不同的是它按照数组索引从高到低（从右到左）处理数组，而不是从低到高。

#### 7.8.2 使用flat() 和 flatMap()展平数组

​	1、

```javascript
[1, [2, 3]].flat()    // => [1, 2, 3]
[1, [2, [3]]].flat()  // => [1, 2, [3]]
```

#### 7.8.3 使用concat()添加数组

​	1、concat() 方法创建并返回一个新数组，它的元素包括调用 concat() 的原始数组的元素和 concat() 的每个实参。返回的是一个新数组，而不是在原数组的基础上添加元素。

#### 7.8.4 栈和队列push() pop() shift() unshift()

​	1、push() 和 pop() 方法允许将数组当做栈来使用。push() 方法在数组的尾部添加一个或多个元素，并返回数组新的长度。pop() 方法则相反：它删除数组的最后一个元素，减小数组长度并返回它删除的值。组合使用 push() 和 pop() 能够用 JavaScript 数组实现先进后出的栈：

```javascript
let stack = [];       // stack == []
stack.push(1,2);      // stack == [1,2];
stack.pop();          // stack == [1]; returns 2
stack.push(3);        // stack == [1,3]
stack.pop();          // stack == [1]; returns 3
stack.push([4,5]);    // stack == [1,[4,5]]
stack.pop()           // stack == [1]; returns [4,5]
stack.pop();          // stack == []; returns 1
```

​	2、push() 方法不展平传入的数组，但如果想要将数组的元素全部压入另外一个数组，可以使用展开运算符...

​	3、unshift() 和 shift() 方法的行为非常类似于 push() 和 pop()，不一样的是前者是在数组的头部而非尾部进行元素的插入和删除操作。unshift() 在数组的头部添加一个或多个元素，并将已存在的元素移动到更高索引的位置来获得足够的空间，最后返回数组新的长度。shift() 删除数组的第一个元素并将其返回，然后把所有随后的元素下移一个位置来填补数组头部的空缺。他的效率低于push()和pop()。

#### 7.8.5 搜索和排序方法

​	1、indexOf() 和 lastIndexOf() 在数组中搜索具有指定值的元素，并返回找到的第一个元素的索引，如果未找到，则返回 -1。indexOf() 从头到尾搜索数组，lastIndexOf() 从尾到头搜索：

​	2、includes() 返回是否包含参数元素。

​	3、sort() 对数组的元素直接进行排序，并返回排序后的数组。

​	4、如果数组中包含 undefined 元素，它们会被放在数组的结尾。

​	5、若要将数组按字母顺序以外的顺序排序，必须将比较函数作为实参传递给 sort()。该函数决定了它的两个实参在排好序的数组中的先后顺序。假设第一个实参应该在前，比较函数应该返回一个小于 0 的数值。反之，假设第一个参数应该在后，函数应该返回一个大于 0 的数值。并且，假设两个值相等（也就是说，它们的顺序无关紧要），函数应该返回 0。例如，用数值大小而非字母表顺序进行数组排序，代码如下：

```javascript
let a = [33, 4, 1111, 222];
a.sort();               // a == [1111, 222, 33, 4]; alphabetical order
a.sort(function(a,b) {  // Pass a comparator function
    return a-b;         // Returns < 0, 0, or > 0, depending on order
});                     // a == [4, 33, 222, 1111]; numerical order
a.sort((a,b) => b-a);   // a == [1111, 222, 33, 4]; reverse numerical order

```

#### 7.8.6 数组到字符串的转换

​	1、join() 方法将数组的所有元素转换为字符串并连接它们，返回生成的字符串。可以指定一个可选字符串来分隔生成的字符串中的元素。如果未指定分隔符字符串，则使用逗号：

```javascript
let a = [1, 2, 3];
a.join()               // => "1,2,3"
a.join(" ")            // => "1 2 3"
a.join("")             // => "123"
let b = new Array(10); // An array of length 10 with no elements
b.join("-")            // => "---------": a string of 9 hyphens
```

​	2、数组与所有 JavaScript 对象一样，具有 toString() 方法。对于数组，此方法的工作方式与没有参数的 join() 方法相同：

```javascript
[1,2,3].toString()          // => "1,2,3"
["a", "b", "c"].toString()  // => "a,b,c"
[1, [2,"c"]].toString()     // => "1,2,c"
```

## Chapter 8 函数

​	1、JS的函数声明是函数名变成一个变量，这个变量的值是函数本身。













