# Vue Cheatsheet

This is a simplified cheatsheet along with some tips for people who often works with Vue.js.

***

<h3 align="center">Please help this repo with a :star: if you find it useful! :blush:</h3>

***

## Table of Contents

* [Expressions](#expressions)
* [Directives](#directives)
* [List Rendering](#list-rendering)
* [Binding](#binding)
* [Actions / Events](#actionsevents)
* [Component Anatomy](#component-anatomy)
* [Custom Events](#custom-events)
* [Life Cycle Hooks](#life-cycle-hooks)
* [Using a Single Slot](#using-a-single-slot)
* [Multiple slots](#multiple-slots)
* [Libraries You Should Know](#libraries-you-should-know)
* [Tips](#tips)
  * [Nested objects are NOT reactive (by default)](#1-nested-objects-are-not-reactive-by-default)
  * [Learn and use Vuex from the start](#2-learn-and-use-vuex-from-the-start)
  * [When in doubt, re-render](#3-when-in-doubt-re-render)
  * [Learn the difference between props and data](#4-learn-the-difference-between-props-and-data)
  * [Have a plan for loading elements](#5-have-a-plan-for-loading-elements)
  * [Make common filters global](#6-make-common-filters-global)
* [Contribution](#contribution)
* [Donate](#donate)

## Expressions

```vue
<div id="app">
  <p>I have a {{ product }}</p>
  <p>{{ product + 's' }}</p>
  <p>{{ isWorking ? 'YES' : 'NO' }}</p>
  <p>{{ product.getExpiryDate() }}</p>
</div>
```

## Directives

Element inserted/removed based on truthiness:

```html
<p v-if="inStock">{{ product }}</p>
<p v-else-if="onSale">...</p>
<p v-else>...</p>
```

Toggles the display: none CSS property:

```html
<p v-show="showProductDetails">
  ...
</p>
```

Two-way data binding:

```html
<input v-model="firstName">
v.model.lazy="..." // Syncs input after change event
v.model.number="..." // Always returns a number
v.model.trim="..." // Strips whitespace
```

## List Rendering

```html
<li v-for="item in items" :key="item.id">
  {{ item }}
</li>
```

To access the position in the array:

```html
<li v-for="(item, index) in items">...</li>
```

To iterate through objects:

```html
<li v-for="value in object">...</li>
<li v-for="(value, index) in object">...</li>
<li v-for="(value, name, index) in object">...</li>
```

Using v-for with a component:

```html
<cart-product v-for="item in products" :product="item" :key="item.id">
```

## Binding

```html
<a v-bind:href="url">...</a>
<a :href="url">...</a> // Shorthand
```

True or false will add or remove attribute:

```html
<button :disabled="isButtonDisabled">...</button>
```

If isActive is truthy, the class 'active' will appear:

```html
<div :class="{ active: isActive }">...</div>
```

Style color set to value of activeColor:

```html
<div :style="{ color: activeColor }">...</div>
```

Passing arguments to a computed binding:

```vue
<template>
  <ProductComponent :cost="product_type('product_2')"></ProductComponent>
</template>

<script>
  import ProductComponent from @/components/ProductComponent
  
  export default {
    components: { ProductComponent },
    data() {
      return {
        products: {
          product_1: '100',
          product_2: '200',
          product_3: '300'
        }
      }
    },
    computed: {
      product_type() {
        // Argument passed to arrow function, NOT computed function declaration.
        return (product_id) => { // Arrow function to allow 'this' instance to be accessible.
          return this.products[product_id]  // Square bracket notation for 'any' type variable
        }
      }
    }
  }
</script>
```

## Actions/Events

Calls addToCart method on component:

```html
<button v-on:click="addToCart">...</button>
<button @click="addToCart">...</button> // Shorthand
```

Arguments can be passed:

```html
<button @click="addToCart(product)">...</button>
```

To prevent default behaviour (e.g. page reload):

```html
<form @submit.prevent="addProduct">...</form>
```

Only trigger once:

```html
<img @mouseover.once="showImage">
.stop // Stop all event propagation
.self // Only trigger if event.target is element itself
```

Keyboard entry example:

```html
<input @keyup.enter="submit">
```

Call onCopy when control-c is pressed:

```html
<input @keyup.ctrl.c="onCopy">
```

Key modifiers:

```markdown
.tab
.delete
.esc
.space
.up
.down
.left
.right
.ctrl
.alt
.shift
.meta
```

Mouse modifiers:

```markdown
.left
.right
.middle
```

## Component Anatomy

```vue
<template>
  <span>{{ message }}</span>
</template>

<script>
  import ProductComponent from '@/components/ProductComponent'
  import ReviewComponent from '@/components/ReviewComponent'
  
  export default {
    components: { // Components that can be used in the template
      ProductComponent,
      ReviewComponent
    },
    props: { // The parameters the component accepts
      message: String,
      product: Object,
      email: {
        type: String,
        required: true,
        default: 'none',
        validator: function (value) {
          // Should return true if value is valid
        }
      }
    },
    data: function () { // Must be a function
      return {
        firstName: 'Vue',
        lastName: 'Mastery'
      }
    },
    computed: { // Return cached values until dependencies change
      fullName: function () {
        return `${this.firstName} ${this.lastName}`
      }
    },
    watch: { // Called when firstName changes value
      firstName: function (value, oldValue) { /* ... */ }
    },
    methods: { /* ... */ }
  }
</script>
```

## Custom Events

Use props (above) to pass data into child components, custom events to pass data to parent elements.

Set listener on component, within its parent:

```html
<button-counter v-on:incrementBy="incWithVal">...</button-counter>
```

Inside parent component:

```js
methods: {
  incWithVal: function (toAdd) { ... }
}
```

Inside button-counter template:

```js
this.$emit('incrementyBy', 5)
```

## Life Cycle Hooks

```
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
beforeDestroy
destroyed
```

## Using a single slot

Component template `MyComponent`:

```html
<div>
  <h2>I'm a title</h2>
  <slot>
    Only displayed if no slot content
  </slot>
</div>
```

Use of `MyComponent` with custom data for the slot:

```html
<my-component>
  <p>This will go in the slot</p>
</my-component>
```

## Multiple slots

Component `AppLayout` template:

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot>Default content</slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

Use of `AppLayout` with custom data for the slots:

```html
<app-layout>
  <h1 slot="header">Page title</h1>
  <p>The main content</p>
  <p slot="footer">Contact info</p>
</app-layout>
```

## Libraries you should know

**[Vue CLI](https://router.vuejs.org)**

Command line interface for rapid Vue development.

**[Vue Router](https://router.vuejs.org)**

Navigation for a Single-Page Application.

**[Vue DevTools](https://github.com/vuejs/vue-devtools)**

Browser extension for debugging Vue applications.

**[Nuxt.js](https://nuxtjs.org)**

Library for server side rendering, code-splitting, hot-reloading, static generation and more.

# Tips

### 1. Nested objects are NOT reactive (by default)

```vue
<script>
  export default {
    data () {
      return {
        someVar: ''
      }
    },
    mounted () {
      this.someVar: {
        level1: {
          level2: {
            level3: 'something old'
          }
        }
      }
    },
    methods : {
      changeSomeVar () {
        this.someVar.level1.level2.level3 = 'something new'
      }
    }
  }
</script>
```

That method looks like it should work, say you have an input that matches `someVar.level1.level2.level3`, if you ran this method it would not update the model. Instead you need to use `Vue.set` or in a SPC (Single Page Component) you'd just use `this.$set`:

```vue
<script>
  export default {
    // ...
    methods : {
      changeSomeVar () {
        this.$set(this.someVar.level1.level2, 'level3', 'new value here')
      }
    }
  }
</script>
```

### 2. Learn and use Vuex from the start

This could start a flame war, some Vue fan will tell you to start with an event bus and work your way up, but Vuex is modular enough that you can use it on small and large apps alike. If you're building a SPA there's no chance you'll have fun without Vuex, you're going to implement a lot of the same functionality in your event bus, and making any other developer who works on the project's life a living hell.

A good primer on vuex can be found here: [WTF is Vuex? A Beginner’s Guide To Vue’s Application Data Store](https://vuejsdevelopers.com/2017/05/15/vue-js-what-is-vuex/)

### 3. When in doubt, re-render

Here's a simple use case, say you have an order form that pops up. If for some reason the user closes the order form and reopens it you might find some of the fields won't allow edits, or they have stale data, or if you're triggering the popup via a select box it might not work right. Honestly it's a major headache.

One trick is to re-render your components. The easiest method I've found to do that is whenever a modal or some other component is registered on the DOM pass it a key, or on mount make it generate a random one. A good key could just be to use `Date.now()` or `moment.js` to generate a UTC timestamps and use that.

The key tells Vue that this is a NEW instance, forget about the old one, and let's start over.

### 4. Learn the difference between props and data

Essentially a prop is data that you pass INTO the component from a parent component or on initializing the root component for the first time.

Data is the reactive properties defined on the instance. I find it to be a good practice if you ever think you'll need to update the value or use it re-actively to create a new value on mount that is a duplicate of the prop. So say you have a prop called `colorProp`, you might have a value in data called just `color`, then in your `mounted()` method have `this.color` set to `colorProp`.

### 5. Have a plan for loading elements

You may start out just letting users wait without knowing what's going on but this is going to get dirty fast. Especially when you bring Vuex and multiple data points into the mix. It's best to have a single global **loader** setup that triggers whenever the global **loading** property from Vuex is updated. This way you can always toggle it properly and make sure to un-toggle it.

One caveat is though - be sure you catch all errors - especially when using axios and promises and be sure to end the loading message on errors so users can go back, fix things and resubmit the form.

### 6. Make common filters global

The example below is a bad example on how you should be using filters:

```vue
<template>
  <div>
    <!-- Bad idea -->
    <input type="text" ..> ${{ moneyVar | money }}
  </div>
</template>

<script>
  export default {
    data() { 
      moneyVar: 3.50
    },
    filters: {
      money: function (value) {
        if (!value) {
          return '0.00'
        }
        return '$' + parseFloat(value).toFixed(2)
      }
    }
  }
</script>
```

We should make it a global filter:

```vue
<script>
  Vue.filter('money', function (value) {
    if (!value) {
      return '0.00'
    }
    return '$' + parseFloat(value).toFixed(2)
  })
</script>
```

# Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Reach out to me directly at <mauriurraco@gmail.com>
