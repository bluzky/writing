---
title: "My First Post"
date: 2018-01-15T23:10:02+07:00
draft: false
---

# Vuejs

#book_note

[TOC]

## 1. Computed property, watcher

1. `computed` propterty
   * is a function
   * return value is cached
   * reevaluate when dependency properties changed
   * ex:

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function() {
      // `this` points to the vm instance
      return this.message
        .split('')
        .reverse()
        .join('')
    }
  }
})
```

2. `computed` vs `function`
   `function` is reevaluate each time it is called

3. `watched` property
   * is a function
   * triggered every time watched property changed
   * ex

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function() {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

## 2. Class and Style binding

### 1. Class binding

* Trigger class by property using `v-bind:class`

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

* Binding object

```html
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

* Binding array

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

### 2. Binding style

* Binding object property using `v-bind:style`

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

* Binding a single object

```html
<div v-bind:style="styleObject"></div>
```

```js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

* Binding array object

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

## 3. Conditional rendering

* syntax

```js
v-if
v-else-if / v-else
```

* `v-if` without wrapper, using `<template>` tag for invisible wrapper

* `v-if` vs `v-show`

`v-if` -> re-rendered when property changed -> high cost toggle
`v-show` -> render all at initializing not re-render when property changed -> high cost initializing

## 4. Loop rendering

## 5. Handling event

### 1. Usage

* Default binding `v-on:<event_name>`

```html
<div id="example-2">
  <!-- `greet` is the name of a method defined below -->
  <button v-on:click="greet">Greet</button>
</div>
```

```js
	methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
```

* Binding with data

```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```

```js
	methods: {
    say: function (message) {
      alert(message)
    }
  }
```

* Binding data with default event object

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

```js
methods: {
  warn: function (message, event) {
    // now we have access to the native event
    if (event) event.preventDefault()
    		alert(message)
  }
}
```

### Event modifier

> Modify event handler behaviour after valuated

To address this problem, Vue provides event modifiers for v-on. Recall that modifiers are directive postfixes denoted by a dot.

* `stop`
* `prevent`
* `capture`
* `self`
* `once`

```html
<!-- the click event's propagation will be stopped -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form v-on:submit.prevent></form>

<!-- use capture mode when adding the event listener -->
<!-- i.e. an event targeting an inner element is handled here before being handled by that element -->
<div v-on:click.capture="doThis">...</div>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div v-on:click.self="doThat">...</div>
```

### 3. Key modifier

> Only evaluate binding function if `key` property matches modifier

```html
<!-- only call `vm.submit()` when the `keyCode` is 13 -->
<input v-on:keyup.13="submit">
```

* `.enter`
* `.tab`
* `.delete` (captures both “Delete” and “Backspace” keys)
* `.esc`
* `.space`
* `.up`
* `.down`
* `.left`
* `.right`
