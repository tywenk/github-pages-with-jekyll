---
title: "React: How to use useState()"
date: 2022-03-03
---

_In this blog I will be strictly discussing React in the context of using it with functional components and hooks._

# What is useState?

`useState` is a built-in method of the React library that allows developers to store variables that persist throughout the rendering lifecycle of a componenet. Much like a variable, state can be called and mutated throughout a component. State can also be passed down as a prop to children components. State has a unique property which is that it requires a _setter_ function to change the state, rather than in Vanilla Javascript where you can reassign a variable declared with `let` elsewhere within appropriate scope. This gochya does add a little syntactical weight to `useState` but it can be easily managed.

To use `useState` you have to import it at the top of your component, like so:

```jsx
import { useState } from "react"
```

> Note that if you are using a React version older than version 17, then your syntax for import will look like: `import React, { useState } from "react"`

To implement `useState` you will insert the useState assignment call _inside_ of your functional component. For example say we wanted to store the `health` of a player of our game made in React:

```jsx
import { useState } from "react"

function Player() {
	const [health, setHealth] = useState(100)
	return (
		<div>
			<h1>Hello, Player</h1>
		</div>
	)
}

export default Inventory
```

Let's focus in on the line of code that is setting state.

```jsx
const [health, setHealth] = useState(100)
```

`useState` is a method that returns an array, or more precisely, a _tuple_. A tuple is an array of set length. While Javascript does not officially have tuples, colloquially we can say that useState returns a tuple because it always returns an array with length two.

The array `useState` returns a specific ordering. The first index is reserved for the current state of the variable, and the seconds index is reserved for a setter function that can mutate the value of the In this case, `health` references the current value of the player's health. `setHealth` references a function which takes as either a) and argument or b) a callback that sets the value of `health`. By convention the setter function is prepended with `set` although it is not required. Also by convention when one calls `useState` they reference it using array destructuring, as we did in our example.

Finally, the number `100` in `useState` is used to initialize `health` to the value of `100`.

In short, the syntax for declaring a state variable can be broken down into this more readable psuedo code:

```
const [declare the variable name, declare setter function to change variable] = useState(initial value)
```

So, how does one set a new state value?

In most cases it is as easy as `setHealth(80)`. This will overwrite the initial value of `100` and set it to `80`. However is cases where you want to add onto the current value, you must use a callback in the setter. So, to add `30` to now current value of health (`80`), we use `setHealth(health => health += 30)`.

Fundamentally this is because the setter function is _asynchronous_. By implementing a callback, we refer the the current value of the state. If we don't use a callback then we are blindly setting the value and not waiting to ensure the value was set. This callback structure becomes useful when (or if) you want to chain series of setters together all of which rely on the previous one completing. For example

```jsx
setHealth((health) => (health += 30))
setHealth((health) => (health += 30))
setHealth((health) => (health += 30))
```

Assuming `health` starts at `100` then health will be `190` at the end of this operation. If we did not use callbacks, then the value comes out differently, and not as expected.

```jsx
setHealth(health += 30))
setHealth(health += 30))
setHealth(health += 30))
```

In _some_ cases this may return `130` as the final value. This unexpected behavior is why you should always use callbacks in your setter function to ensure you are getting the most current value of the state you are trying to manipulate.

If you are coming from Javascript this might seem like a lot of work to declare a variable. Why can't we just `const health = 100` and later in the code, to update it, set `health = 80`?

React components have a render lifecycle. Each time a component re-renders it will redeclare all of the variables inside of it, essentially cleaning its cache. If we look at this diagram's "Updating" column we can see that `setState()` causes a render to occur.

![react render lifecycle](https://miro.medium.com/max/1400/1*cEWErpe-oY-_S1dOaT1NtA.jpeg)
**[Source](https://code.likeagirl.io/understanding-react-component-life-cycle-49bf4b8674de)**

When we set a new value to state, the entire component **automatically** re-renders to update its UI to account for new values. **This is where React gets its name. Setting state tells React to "react" to the changes you give it.** With React you don't need to impertively (see: manually) tell Javascript: "ok, if this variable changes, then update this part of the UI." React as a library handles that all by itself.

So, to reiterate, calling a setting function on state variable causes a re-render of the component. There a few caveats here, like if the state doesn't change React is smart enough not to re-render.

# What is state below the hood?

I'll leave you with this note, which I found interesting, about how state is actually handled behind the scenes by React:

"State is a plain JavaScript object used by React to represent an information about the component’s current situation. It’s managed in the component (just like any variable declared in a function). The difference is while a “normal” variable “disappears” when their function exits, the state variables are preserved by React." [Citation](https://medium.com/edonec/state-in-react-an-overview-a182675cee2c#:~:text=State%20is%20a%20plain%20JavaScript%20object%20used%20by%20React%20to,variable%20declared%20in%20a%20function).
