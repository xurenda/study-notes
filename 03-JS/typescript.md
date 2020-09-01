# Typescript

> `javascript` 超集：`typescript`



## 类型声明

> `typescript` 支持类型声明，便于项目维护和可阅读，减少出错率。

~~~
let/const 变量名: 数据类型 = 值
~~~

~~~typescript
// 布尔：boolean
let bool: boolean = true

// 数值：number
let num: number = 18

// 字符串：string
let str: string = 'hello world'
str = `the num is ${num}`

// 数组：array
// 写法1（推荐）
let arr1: number[] = [ 1, 2, 3 ]
let arr2: (number | string)[] = [ 1, 'a' ]
// 写法2
let arr3: array<number> = [ 1, 2, 3 ]
let arr4: array<number | string> = [ 1, 'a' ]

// 元组：tuple（更严格的数组：规定值的数量和各自的类型）
let tuple: [ string, number, boolean ]
tuple = [ 'ok', 66, false ]

// 枚举：enum
// 不指定索引默认从 0 开始
enum Roles {
    SUPER_ADMIN, // 0
    ADMIN, // 1
    USER // 2
}
console.log(Roles.SUPER_ADMIN) // 0
console.log(Roles[1]) // ADMIN
// 指定索引，指定元素后索引递增，指定元素前元素索引为默认值
enum Letter {
    A, // 0
    B = 5, // 5
    C // 6
}

// 任意：any（不限类型，尽量少用）
let value: any
value = 123
value = 'abc'
value = new Date()

// void：函数无 return
const foo = (text: string): void => {
    console.log(text)
}

// null \ undefined
const n: null = null
const u: undefined
// null 和 undefined 是其他所有类型的子类型，也就是说，其他类型可以赋值为 null 或 undefined

// never：永远不存在的值
const errorFunc = (message: string): never => {
	throw new Error(message)
}
const infiniteFunc = (): never => {
    while (true) {}
}
let neverVariable = (() => {
    while (true) {}
})()
// neverVariable：never 类型
// never 类型可以赋值给其他类型，其他类型不可以赋值给 never 类型

// 对象：object
function bar(obj: object): void {
    console.log(obj)
}
bar({ name: 'sam', age: 18 })

// 类型断言（强制类型转换）
const getLength = (target: string | number): number => {
    if ((<string>target).length || (<string>target).length === 0) // 写法1，使用tsx语法不支持
        return (target as string).length // 写法2（推荐）
    else
        return target.toString().length
}
~~~



## Symbol

> `ES6` 新增基本类型，而非引用类型

~~~typescript
// 每一个 symbol 值都是独一无二的
const s1 = Symbol() // 注意：不需要 new
const s2 = Symbol()
console.log(s1 === s2) // false

// 括号中的字符串只用于标识，以便于阅读和区别
const s3 = Symbol('lison')

console.log(s3) // "Symbol('lison')"
console.log(s3.toString()) // "Symbol('lison')"
console.log(Boolean(s3)) // true
console.log(!s3) // false

// symbol 作为属性名，通过 [] 进行 set/get
// 因为 symbol 不可修改、不可覆盖，所以常用作私有属性、私有方法
const s4 = Symbol('name')
const info = { [s4]: 'lison' }
console.log(info) // {Symbol(name): "lison"}
console.log(info[s4]) // "lison"
console.log(info.s4) // undefined
info[s4] = 'sam'
console.log(info) // {Symbol(name): "sam"}
info.s4 = 'lili'
console.log(info) // {s4: "lili", Symbol(name): "sam"}

// 属性名的遍历
// 1. for in 无法访问 symbol
for (const key in info)
    console.log(key) // s4
// 2. Object.keys() 无法访问 symbol
console.log(Object.keys(info)) // ["s4"]
// 3. Object.getOwnPropertyNames() 无法访问 symbol
console.log(Object.getOwnPropertyNames(info)) // ["s4"]
// 4. JSON.stringify() 无法访问 symbol
console.log(JSON.stringify(info)) // '{"s4":"lili"}'
// 5. Object.getOwnPropertySymbols() 专门访问 symbol，且只能访问 symbol
console.log(Object.getOwnPropertySymbols(info)) // [Symbol(name)]
// 6. Reflect.ownKeys() 可以访问 symbol 和普通属性
console.log(Reflect.ownKeys(info)) // (2) ["s4", Symbol(name)]

// Symbol.for()：通过 key 创建 symbol 值，key 一样则 symbol 值一样
// Symbol.keyFor()：获得 symbol 的 key
const s5 = Symbol.for('lison')
const s6 = Symbol.for('lison')
console.log(s5 === s6) // true
console.log(Symbol.keyFor(s5)) // "lison"
const s7 = Symbol()
console.log(Symbol.keyFor(s7)) // undefined

// 11 个内置 Symbol 值
~~~



## 学习参考

1. 官网：https://www.typescriptlang.org/
2. 中文手册：https://typescript.bootcss.com/
3. Typescript 全面解读 - bilibili：https://www.bilibili.com/video/av67175606