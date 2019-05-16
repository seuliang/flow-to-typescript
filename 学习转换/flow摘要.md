flow 文档
https://flow.org/en/docs/types/primitives/

javascirpt包装对象（wrapper object）
https://blog.csdn.net/u014357799/article/details/90267491


# Primitive Types

null
undefined (void in Flow types)


## Maybe types 
参数可选,兼容 type | null | undefined  
````
// @flow
function acceptsMaybeString(value: ?string) {
  // ...
}

acceptsMaybeString("bar");     // Works!
acceptsMaybeString(undefined); // Works!
acceptsMaybeString(null);      // Works!
acceptsMaybeString();          // Works!
````


## Optional object properties 
可选对象属性  
可以为 void ,不可以为 null
````
{ propertyName?: string }
````

## Optional function parameters
可选函数参数
````
function method(param?: string) { /* ... */ }
````
可以为 void 或空,不可以为 null

## Function parameters with defaults
函数默认参数
````
function method(value: string = "default") { /* ... */ }
````

# Mixed Types

## A single type
特定类型
````
function square(n: number) {
  return n * n;
}
````
## A group of different possible types:
多个类型
````
function stringifyBasicValue(value: string | number) {
  return '' + value;
}
````

## A type based on another type:
模板类型
````
function identity<T>(value: T): T {
  return value;
}
````
````
flow
function f<A: string>(a:A){}

Typescript
function f<A extends string>(a:A){}
````

# Variable Types

JavaScript has three ways of declaring local variables:

1. var - declares a variable, optionally assigning a value. (MDN)
2. let - declares a block-scoped variable, optionally assigning a value. (MDN)
3. const - declares a block-scoped variable, assigning a value that cannot be re-assigned. (MDN)

## Rest Parameters
参数数组, 前有 ... ,可以指定类型用 Array
````
function method(...args: Array<number>) {
  // ...
}
````

## Function Returns
异步函数隐式返回 Promise,所以返回类型必须为 Promise
````
// @flow
async function method(): Promise<number> {
  return 123;
}
````



# Object Types
## Optional object type properties
可选对象属性,不能为 null
````
// @flow
var obj: { foo?: boolean } = {};

obj.foo = true;    // Works!
// $ExpectError
obj.foo = 'hello'; // Error!
````

## Exact object types 
确定性对象,不允许增加属性
````
{| foo: string, bar: number |}
````

组合
````
// @flow

type FooT = {| foo: string |};
type BarT = {| bar: number |};

type FooBarFailT = FooT & BarT;
type FooBarT = {| ...FooT, ...BarT |};
````

## Objects as maps
新的js由 Map 类,不过可以还用对象模拟
indexer property 称为索引属性
````
// @flow
var o: { [string]: number } = {};
o["foo"] = 0;
o["bar"] = 1;
var foo: number = o["foo"];
````

````
// @flow
var obj: { [user_id: number]: string } = {};
obj[1] = "Julia";
obj[2] = "Camille";
obj[3] = "Justin";
obj[4] = "Mark";
````

# Array Types
````
Array<Type>
let arr1: Array<boolean> = [true, false, true];
let arr3: Array<mixed> = [1, true, "three"]
````
简写
````
Type[]
let arr: number[] = [0, 1, 2, 3];
Just note that ?Type[] is the equivalent of ?Array<T> and not Array<?T>.
````
````
// @flow
let arr1: ?number[] = null;   // Works!
let arr2: ?number[] = [1, 2]; // Works!
let arr3: ?number[] = [null]; // Error!
````
If you want to make it Array<?T> you can use parenthesis like: (?Type)[]
````
// @flow
let arr1: (?number)[] = null;   // Error!
let arr2: (?number)[] = [1, 2]; // Works!
let arr3: (?number)[] = [null]; // Works!
````


## 只读
$ReadOnlyArray<T> 
````
  // @flow
const someOperation = (arr: Array<number | string>) => {
  // Here we could do `arr.push('a string')`
}

const array: Array<number> = [1]
someOperation(array) // Error!
````


# Tuple Types
元组
````
// @flow
let tuple: [number, boolean, string] = [1, true, "three"];
let str  : string  = tuple[2]; // Works!
````

# Class Types
## Class Syntax
语法
````
class MyClass {
  prop: number;
  method(value: string): number { 
      this.prop = 42;
  }
}

let myInstance: MyClass = new MyClass();
````
## Class Generics 
泛型
````
// @flow
class MyClass<A, B, C> {
  constructor(arg1: A, arg2: B, arg3: C) {
    // ...
  }
}

var val: MyClass<number, boolean, string> = new MyClass(1, true, 'three');
````


# Type Aliases
类型别名
````
// @flow
type MyObject = {
  // ...
};

var val: MyObject = { /* ... */ };
function method(val: MyObject) { /* ... */ }
class Foo { constructor(val: MyObject) { /* ... */ } }
````
语法
````
type Alias = Type;
````

## Type Alias Generics 
泛型
````
type MyObject<A, B, C> = {
  property: A,
  method(val: B): C,
};
````
# Interface Types
接口类型
````
// @flow
interface Serializable {
  serialize(): string;
}

class Foo {
  serialize() { return '[Foo]'; }
}

class Bar {
  serialize() { return '[Bar]'; }
}

const foo: Serializable = new Foo(); // Works!
const bar: Serializable = new Bar(); // Works!
````
设置接口类型
````
// @flow
interface Serializable {
  serialize(): string;
}

class Foo implements Serializable {
  serialize() { return '[Foo]'; } // Works!
}

class Bar implements Serializable {
  // $ExpectError
  serialize() { return 42; } // Error!
}
````
多个接口
````
class Foo implements Bar, Baz {
  // ...
}
````

## 接口语法  
````
interface MyInterface {
  +covariant: number;     // read-only
  -contravariant: number; // write-only
}
````
https://flow.org/en/docs/types/interfaces/#toc-interface-syntax



# Generic Types
多态类型,泛型
````
function identity<T>(value: T): T {
  return value;
}
````



# Union Types
````
Type1 | Type2 | ... | TypeN
type Foo =
  | Type1
  | Type2
  | ...
  | TypeN
````

确定对象的类型
````
// @flow
type Success = {| success: true, value: boolean |};
type Failed  = {| error: true, message: string |};

type Response = Success | Failed;

function handleResponse(response: Response) {
  if (response.success) {
    var value: boolean = response.value;
  } else {
    var message: string = response.message;
  }
}
````

#  Intersection Types
组合类型
````
// @flow
type A = { a: number };
type B = { b: boolean };
type C = { c: string };

function method(value: A & B & C) {
  // ...
}

// $ExpectError
method({ a: 1 }); // Error!
// $ExpectError
method({ a: 1, b: true }); // Error!
method({ a: 1, b: true, c: 'three' }); // Works!
````



# Type Casting Expressions
类型检查,()是一个表达式,会检查类型并报错
````
(value: Type)
````

# Utility Types
实用类型
## $Keys<T> 
  取出主键
  ````
  // @flow
const countries = {
  US: "United States",
  IT: "Italy",
  FR: "France"
};

type Country = $Keys<typeof countries>;
//相当于 type Country = 'US' | 'IT' | 'FR'
const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
````
  
  ## $Values<T> 
  取出类型值
  ````
  // @flow
type Props = {
  name: string,
  age: number,
};

// The following two types are equivalent:
type PropValues = string | number;
type Prop$Values = $Values<Props>;
````
  
  ## $ReadOnly<T>
  只读
    
  ## $Exact<T> 
  精确
  
  ## $Diff<A, B> 
  做差
  
  ## $Rest<A, B>
  取余
  
  ## $PropertyType<T, k> 
  主键为 k 的属性
  
  ## $ElementType<T, K> 
  元素属性
  
  ## $NonMaybeType<T> 
  去除 maybe type 中的 null 和undefined
  
  ## $ObjMap<T, F> 
  ## $ObjMapi<T, F> 
  ## $Call<F, T...> 
  ## Class<T> 
  
  ## $Shape<T> 
  
  ## $Supertype<T> 
  
  ## $Subtype<T> 
  
  ## Existential Type (*) 
  
  # Module Types
  类型导出  
  对于 type aliases, interfaces, and classes 直接操作  
  其他类型,使用 typeof
  
  
  
  # Comment Types
  使用注释而避免编译js
  ````
  // @flow

/*::
type MyAlias = {
  foo: number,
  bar: boolean,
  baz: string,
};
*/

function method(value /*: MyAlias */) /*: boolean */ {
  return value.bar;
}

method({ foo: 1, bar: true, baz: ["oops"] });
````

  
  
  
