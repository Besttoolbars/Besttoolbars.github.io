# Generic in typescript

## Introduction
Types, interfaces, and classes help to describe a domain model and help to structure code. It brings certainty and consistency in the data flow by language itself.

However, sometimes domain types are wrapped by some infrastructure information or some common structure, especially on integration layers(REST, libs). [Or introduce some level of abstraction]

## Wrapper example
Let’s consider an example of a common wrapper structure where one of the fields is determined by domain logic.

> Создать проблемy

The simplest solution is to create the following type (or interface or class):
```typescript
type Wrapper = {
    count: number
    value: any // or object
    // other field
}
```
where the field `value` represents domain logic.

However, in that case, type of the field `value` is downcast to any. That is not the typed approach where we want to take advantage of deterministic object structure at compile time.
So, let us try the Generic.

```typescript
type Wrapper<T> = {
    count: number
    value: T
    // other field
}
```

and the instance of that type will be
(3) `const wrapper: Wrapper<string> = { count: 1, value: "Hello generic!" }`

By that, we can achieve type variation of the field `value` without losing it.

There are four key points:
1. `Wrapper<T>` - `<...>` is the place of the generic type definition.

2. `Wrapper<T>` - `T` is the Generic within the scope of type or closure. `T`  used as the generic type name. Any uppercase character is eligible for the generic type name.

3. Instantiate object wrapper as `Wrapper<string>`. So `string` is our generic type, and that means that `value` has type string at compile time.

Let's look at places where we use generics but even don’t notice it - Promise and prototype methods (and most of an libs).

## Generics in a promise

Simple creating of promise would be:
```typescript
const promise = new Promise((resolve, reject) => { 
    resolve(123)
    //resolve("string") <- valid
});
```

There are two core questions
1. What is the type of value returned by promise?
2. What is the type of argument in `resolve` function?

The answer is any. By the way, the implicit type of constant promise would be `Promise<any>`.

> Implicit type is the type of instance\structure which is determined at compile time by the right side of expression if the type on the left side is not specified.

However, what if we need to validate argument of `resolve` function or take advantages of IDE intellisense?

> Достичь чего?

We can achieve it two approaches:
We used the first approach in our example with Wrapper type -

```typescript
const promise: Promise<number>  = new Promise((resolve, reject) => { 
    resolve(123)
    //resolve("string") <- invalid
});
```

The second is using a generic on the class constructor [and take advantage of explicit typing] (or specify type like in the first approach) - 

```typescript
const promise = new Promise<number>((resolve, reject) => {
  resolve(123);
  // resolve("abc")  <- invalid
});
```

In both cases, we achieved the concrete type of the returned value of the promise and argument type of the `resolve` function. Also, we added type validation at compile time by the language feature.

> `reject` function by definition accept any as argument type.

Another example in chain functions or composition of function(rxjs pipe)

...


