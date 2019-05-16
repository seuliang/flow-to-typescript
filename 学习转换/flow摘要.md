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










