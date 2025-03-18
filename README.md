# 2.7 - Protection

###### ICS4U - Mr. Brash 🐿

Protecting members of a class and using Getters or Setters to access private or protected members.


# Lesson
  - [Jump to the task](TASK.md)

## Part 1/2 - Protection

With normal variables, we have no way of _hiding_ the value. Wouldn't it be great if we could make the value of a variable _private_ information?

Any member (property) or function (method) of an object can be "**+ public**" (default), "**- private**" (hidden), or "**# protected**" (read-only). Let's apply this theory to our Stack class. 

---

### The Stack

The `.contents` of the `Stack` class should be _hidden_ (private). We shouldn't be able to access it.<br>
Right now, we can easily ask for the contents and see or manipulate them:
```JS
let sample = new Stack([5, 12, 7]);
console.log(sample.contents);   // [5, 12, 7]  - we shouldn't be able to see that
sample.contents[1] = "Sucker";  // We that's bad
sample.contents = new Array();  // Uh oh...
```

> 🤔 Isn't the purpose of a Stack that we can only see the top item? 

Let's modify our `Stack` class so that it _hides_ the `.contents` _and_ the `.max_size`. To do this, we will use the `#` operator:

```JS
class Stack {
  // Hidden properties
  #contents;
  #max_size;

  constructor(content = [], max_size = 10) {
    if (max_size > 0) this.#max_size = max_size;
    if (content.length <= this.#max_size) this.#contents = content;
  }

  push(value) {
    if (!this.is_full())
      this.#contents.push(value);
  }

  pop() {
    if (!this.is_empty())
      return this.#contents.pop();
  }

  peek() { return this.#contents[this.#contents.length - 1]; }

  is_empty() { return this.#contents.length == 0; }

  is_full() { return this.#contents.length >= this.#max_size; }
}
```

(Feel free to copy that Stack definition to your [code file](main.js))
<br>Using that _new_ definition, we are _unable_ to see the `.contents` array! It's _**private**_!

```JS
// A stack with 2 items and a max of 3
let my_stack = new Stack([9, 2], 3);  

my_stack.contents;   // undefined!

my_stack.is_full();  // false
my_stack.push("Pizza");
my_stack.push("Hello");
my_stack.peek();     // "Pizza"
my_stack.is_full();  // true
```

The `Stack` object is _finally_ acting like a real Stack! The `.contents` are private - that's great! But... so is `max_size`. 🤔 Should that be _completely_ private?

**We should probably be able to _see_ the `max_size` just not _modify_ it.**

## Part 2/2 - Gets and Sets
We made the `.max_size` of our `Stack` invisible. It's private. We can't edit it and we also can't see it.

### 🧐 What if we want to see it?

Class members can be _protected_ instead of _private_. They can be read-only. To do this, we make them _private_ first and add a _getter_.

### Get
To gain read-access to a private property, we add a special _method_ called a "getter".
```JS
class Stack {
  #max_size;   // Private (for now)

  ...

  // Adding a "getter" makes the max_size "Protected" instead of "Private"
  get max_size() {
    return this.#max_size;
  }
}
```

While it looks like a _function_ inside the class definition, we access it like a _property_:
```JS
let my_stack = new Stack([], 25);
console.log(my_stack.max_size);   // Prints 25
my_stack.max_size = 100;   // This does nothing, it's only readable not writable
```

Perfect! Now we can _view_ the `.max_size` member, but we can't change it.

---

### 🤔 What if we want to be able to _modify_ the `max_size`?

The maximum size of a `Stack` has a couple rules.
  1. It cannot be zero or negative.
  2. It cannot go below the current size of `Stack.#contents`

That will require some way of checking that the new size fits the rules.

### The `SET` Statement!

By using a `setter`, we can monitor the value we are trying to use.

```JS
//... inside the Stack class
  set max_size(new_size) { 
    // Don't change it if it breaks the rules
    if ((new_size < 1) || (new_size < this.#contents.length)) 
      return -1;
    
    this.#max_size = new_size;
    return this.#max_size;
  }
```

Now we have the ability to _change_ the `.max_size` responsibly!

Let's try it:
```JS
let plates = new Stack([5, 3, 1], 5);

// Reduce the size below 3
plates.max_size = 1;   // Nothing happens (actually, it returns -1)
plates.max_size;       // 5

// Change it to 3
plates.max_size = 3;   // No problem!
plates.is_full();      // true
```

We finally did it! A fully functional `Stack` object. High five!   👋 🙌 👋
  
Now go take a look at [your task](TASK.md).


<br><br><br><br>
