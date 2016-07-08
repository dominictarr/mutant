mutant
===

Create observables and map them to DOM elements. Massively inspired by `hyperscript` and `observ-*`.

No virtual dom, just direct observable bindings. Unnecessary garbage collection is avoided by using mutable objects instead of blasting immutable junk all over the place.

## Current Status: Experimental / Unpublished

## Install

```bash
npm install @mmckegg/mutant --save
```

## Use

```js
var Struct = require('@mmckegg/mutant/struct')
var send = require('@mmckegg/mutant/send')
var h = require('@mmckegg/mutant/html-element')

var state = Struct({
  text: 'Test',
  color: 'red',
  value: 0
})

var element = h('div.cool', {
  classList: ['cool', state.text],
  style: {
    'background-color': state.color
  }
}, [
  h('div', [
    state.text, ' ', state.value, ' ', h('strong', 'test')
  ]),
  h('div', [
    h('button', {
      'ev-click': send(state.color.set, 'blue')
    }, 'Change color')
  ])
])

setTimeout(function () {
  state.text.set('Another value')
}, 5000)

setInterval(function () {
  state.value.set(state.value() + 1)
}, 1000)

setInterval(function () {
  // bulk update state
  state.set({
    text: 'Retrieved from server (not really)',
    color: '#FFEECC',
    value: 1337
  })
}, 10000)

document.body.appendChild(element)
```
