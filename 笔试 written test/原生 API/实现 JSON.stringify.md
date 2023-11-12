---
title: 实现 JSON.stringify
url: https://www.yuque.com/wcywxq/sd8kbc/mtoe2n
---

JSON.stringify()  方法将一个 JavaScript 对象或值转换为 JSON 字符串，如果指定了一个 replacer 函数，则可以选择性地替换值，或者指定的 replacer 是数组，则可选择性地仅包含数组指定的属性。[MDN](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FJSON%2Fstringify)

```javascript
const jsonstringify = data => {
    // 确认一个对象是否存在循环引用
    const isCyclic = obj => {
        // 使用Set数据类型来存储已经检测过的对象
        let stackSet = new Set()
        let detected = false

        const detect = obj => {
            // 不是对象类型的话，可以直接跳过
            if (obj && typeof obj != "object") {
                return
            }
            // 当要检查的对象已经存在于stackSet中时，表示存在循环引用
            if (stackSet.has(obj)) {
                return (detected = true)
            }
            // 将当前obj存如stackSet
            stackSet.add(obj)

            for (let key in obj) {
                // 对obj下的属性进行挨个检测
                if (obj.hasOwnProperty(key)) {
                    detect(obj[key])
                }
            }
            // 平级检测完成之后，将当前对象删除，防止误判
            /*
              例如：对象的属性指向同一引用，如果不删除的话，会被认为是循环引用
              let tempObj = {
                name: '前端胖头鱼'
              }
              let obj4 = {
                obj1: tempObj,
                obj2: tempObj
              }
            */
            stackSet.delete(obj)
        }

        detect(obj)

        return detected
    }

    // 特性七:
    // 对包含循环引用的对象（对象之间相互引用，形成无限循环）执行此方法，会抛出错误。
    if (isCyclic(data)) {
        throw new TypeError("Converting circular structure to JSON")
    }

    // 特性九:
    // 当尝试去转换 BigInt 类型的值会抛出错误
    if (typeof data === "bigint") {
        throw new TypeError("Do not know how to serialize a BigInt")
    }

    const type = typeof data
    const commonKeys1 = ["undefined", "function", "symbol"]
    const getType = s => {
        return Object.prototype.toString
            .call(s)
            .replace(/\[object (.*?)\]/, "$1")
            .toLowerCase()
    }

    // 非对象
    if (type !== "object" || data === null) {
        let result = data
        // 特性四：
        // NaN 和 Infinity 格式的数值及 null 都会被当做 null。
        if ([NaN, Infinity, null].includes(data)) {
            result = "null"
            // 特性一：
            // `undefined`、`任意的函数`以及`symbol值`被`单独转换`时，会返回 undefined
        } else if (commonKeys1.includes(type)) {
            // 直接得到undefined，并不是一个字符串'undefined'
            return undefined
        } else if (type === "string") {
            result = '"' + data + '"'
        }

        return String(result)
    } else if (type === "object") {
        // 特性五:
        // 转换值如果有 toJSON() 方法，该方法定义什么值将被序列化
        // 特性六:
        // Date 日期调用了 toJSON() 将其转换为了 string 字符串（同Date.toISOString()），因此会被当做字符串处理。
        if (typeof data.toJSON === "function") {
            return jsonstringify(data.toJSON())
        } else if (Array.isArray(data)) {
            let result = data.map(it => {
                // 特性一:
                // `undefined`、`任意的函数`以及`symbol值`出现在`数组`中时会被转换成 `null`
                return commonKeys1.includes(typeof it) ? "null" : jsonstringify(it)
            })

            return `[${result}]`.replace(/'/g, '"')
        } else {
            // 特性二：
            // 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值。
            if (["boolean", "number"].includes(getType(data))) {
                return String(data)
            } else if (getType(data) === "string") {
                return '"' + data + '"'
            } else {
                let result = []
                // 特性八
                // 其他类型的对象，包括 Map/Set/WeakMap/WeakSet，仅会序列化可枚举的属性
                Object.keys(data).forEach(key => {
                    // 特性三:
                    // 所有以symbol为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们。
                    if (typeof key !== "symbol") {
                        const value = data[key]
                        // 特性一
                        // `undefined`、`任意的函数`以及`symbol值`，出现在`非数组对象`的属性值中时在序列化过程中会被忽略
                        if (!commonKeys1.includes(typeof value)) {
                            result.push(`"${key}":${jsonstringify(value)}`)
                        }
                    }
                })

                return `{${result}}`.replace(/'/, '"')
            }
        }
    }
}

// 各种测试

// 1. 测试一下基本输出
console.log(jsonstringify(undefined)) // undefined
console.log(jsonstringify(() => {})) // undefined
console.log(jsonstringify(Symbol("前端胖头鱼"))) // undefined
console.log(jsonstringify(NaN)) // null
console.log(jsonstringify(Infinity)) // null
console.log(jsonstringify(null)) // null
console.log(
    jsonstringify({
        name: "前端胖头鱼",
        toJSON() {
            return {
                name: "前端胖头鱼2",
                sex: "boy"
            }
        }
    })
)
// {"name":"前端胖头鱼2","sex":"boy"}

// 2. 和原生的JSON.stringify转换进行比较
console.log(jsonstringify(null) === JSON.stringify(null))
// true
console.log(jsonstringify(undefined) === JSON.stringify(undefined))
// true
console.log(jsonstringify(false) === JSON.stringify(false))
// true
console.log(jsonstringify(NaN) === JSON.stringify(NaN))
// true
console.log(jsonstringify(Infinity) === JSON.stringify(Infinity))
// true
let str = "前端胖头鱼"
console.log(jsonstringify(str) === JSON.stringify(str))
// true
let reg = new RegExp("w")
console.log(jsonstringify(reg) === JSON.stringify(reg))
// true
let date = new Date()
console.log(jsonstringify(date) === JSON.stringify(date))
// true
let sym = Symbol("前端胖头鱼")
console.log(jsonstringify(sym) === JSON.stringify(sym))
// true
let array = [1, 2, 3]
console.log(jsonstringify(array) === JSON.stringify(array))
// true
let obj = {
    name: "前端胖头鱼",
    age: 18,
    attr: ["coding", 123],
    date: new Date(),
    uni: Symbol(2),
    sayHi: function () {
        console.log("hello world")
    },
    info: {
        age: 16,
        intro: {
            money: undefined,
            job: null
        }
    },
    pakingObj: {
        boolean: new Boolean(false),
        string: new String("前端胖头鱼"),
        number: new Number(1)
    }
}
console.log(jsonstringify(obj) === JSON.stringify(obj))
// true
console.log(jsonstringify(obj))
// {"name":"前端胖头鱼","age":18,"attr":["coding",123],"date":"2021-10-06T14:59:58.306Z","info":{"age":16,"intro":{"job":null}},"pakingObj":{"boolean":false,"string":"前端胖头鱼","number":1}}
console.log(JSON.stringify(obj))
// {"name":"前端胖头鱼","age":18,"attr":["coding",123],"date":"2021-10-06T14:59:58.306Z","info":{"age":16,"intro":{"job":null}},"pakingObj":{"boolean":false,"string":"前端胖头鱼","number":1}}

// 3. 测试可遍历对象
let enumerableObj = {}

Object.defineProperties(enumerableObj, {
    name: {
        value: "前端胖头鱼",
        enumerable: true
    },
    sex: {
        value: "boy",
        enumerable: false
    }
})

console.log(jsonstringify(enumerableObj))
// {"name":"前端胖头鱼"}

// 4. 测试循环引用和Bigint

let obj1 = { a: "aa" }
let obj2 = { name: "前端胖头鱼", a: obj1, b: obj1 }
obj2.obj = obj2

console.log(jsonstringify(obj2))
// TypeError: Converting circular structure to JSON
console.log(jsonStringify(BigInt(1)))
// TypeError: Do not know how to serialize a BigInt
```
