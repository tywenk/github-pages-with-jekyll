---
title: "Optinal Chaining in Javascript"
date: 2022-03-29
---

What is Optional Chaining in Javascript?

Optional Chaining is a new feature of the Javascript language introduced in 2020 that allows for in-line checking of nested object properties. It's a very concise and easy-to-write method that also elegantly allows a program to fail. It avoids the dreaded `cannot read property of undefined` error message when its tries to access a property of an object that does not exist.

Optional chaining is also known as safe navigation.

---

Given an object, `animals`, how would you assign and access a nested value of `name` in `dog`?

```js
const animals = {
	cat: {
		name: "marge",
		age: 2,
	},
	dog: {
		name: "bob",
		age: 6,
	},
}
```

Without optional chaining you would first check to see if `dog` is a property of `animals` with a ternary expression:

`const name = animals.dog ? animals.dog.name : undefined`

This use of a ternary is a valid but a little too lengthy of a way to check for properties as you drill into a object.

With optional chaining, this can be done very efficiently:

`const name = animals.dog?.name`

Optional chaining syntax is using the `?` to check if `dog` exists as a property of `animals`. If it isn't, it will return `undefined` and the object chaining _short circuits_ so that `name` is never called. If `dog` doesexist as a property of `animals`, then the program will proceed and attempt to get `dog`'s property of `name`.

At this point optional chaining does not care if `name` exists as a property -- and in fact there is not way to optional chain the start and end of object drilling. Both these syntaxes are **incorrect**:

`const name = animals?.dog?.name`

`const name = animals.dog?.name?.`

In the following example you can see how elegant it is to sneak in a `?` into dot notation chaining, while also ensuring that the program will fail gracefully by returning `undefined` instead of throwing an error.

```js
const dogName = animals.dog.name

const catName = animals.cat?.name

const duckName = animals.duck?.name

const dogWeight = animals.dog?.weight

console.table({
	dogName: dogName,
	catName: catName,
	duckName: duckName,
	dogWeight: dogWeight,
})
```

This is what will be logged:

| index     | Value     |
| --------- | --------- |
| dogName   | 'bob'     |
| catName   | 'marge'   |
| duckName  | undefined |
| dogWeight | undefined |

Beyond searching properties of an object, optional chaining can also be used to check the truthy-ness of object's expressions, array indexes, and function arguemnts.

```
obj.val?.prop
obj.val?.[expr]
obj.arr?.[index]
obj.func?.(args)
```

The quirky thing to note is that optional chaining requires both symbols `?` and `.` even in situations when dot notation is not used, such as in accessing an array with an index.

Optional chaining (a.k.a. safe navigation) may have slightly different syntax. In Ruby, instead of `?.` the language uses `&.` for [interesting reasons](https://stackoverflow.com/questions/33735228/why-does-ruby-use-its-own-syntax-for-safe-navigation-operator). In many languages, though, something similar to `?` is used.

---

References:

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

[FreeCodeCamp](https://www.freecodecamp.org/news/how-the-question-mark-works-in-javascript/)
