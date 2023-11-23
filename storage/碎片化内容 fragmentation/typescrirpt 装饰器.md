---
title: typescrirpt 装饰器
url: https://www.yuque.com/wcywxq/euxgiy/iig77g48cbbnpl2x
---

<a name="KlIR4"></a>

## v4.x

```typescript
// Method decorator
function myMethodDecorator(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  console.log("Method decorator called");
  console.log("Target class:", target);
  console.log("Method name:", propertyKey);
  console.log("Method descriptor:", descriptor);

  // Modify method behavior example
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log("Method arguments:", args);
    const result = originalMethod.apply(this, args);
    console.log("Method result:", result);
    return result;
  };
}

class MyClass {
  @myMethodDecorator
  myMethod(param1: number, param2: string): string {
    return `Parameter 1: ${param1}, Parameter 2: ${param2}`;
  }
}

// Create class instance and call the decorated method
const instance = new MyClass();
const result = instance.myMethod(42, "Hello");
console.log(result);
```

<a name="HyWM9"></a>

## v5.x

```typescript
function logger(value: any, { kind, name }: ClassMethodDecoratorContext) {
  if (kind === "method") {
    return function (...args: any[]) {
      return value.call(this, ...args)
    }
  }
  return value
}

class MyClass {
  @logger
  print (message: string, a: string, b: string) {
    console.log('message: ', message, a, b)
  }
}

const a = new MyClass()

a.print('aaaaaaa', 'b', 'c')
```
