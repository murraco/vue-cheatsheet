# Vue Cheatsheet

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
<li v-for="(value, key)" in object">...</li>
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

# Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Reach out to me directly at <mauriurraco@gmail.com>

# Donate

`btc: 36V7HqqENSKn6iFCBuE4iCdtB29uGoCKzN`

`eth: 0xB419E3E9fa2233383E0877d442e55C34B9C944dD`
