# Component-Library

OpenLibrary does not have one of these yet, but we strive to do so, evolving the foundations laid in the [Design-Pattern-Library](Design-Pattern-Library)

## What is a component?

A component is a language agnostic block of HTML styled by CSS, which may or may not use JavaScript to enhance it. Some components will be rendered in Python on the server side, some will be rendered on JavaScript and some will be rendered in both.

A component should be reusable. It should not make sure of id's in any way and if it does, these should be provided as parameters to the user of the component.

## Specification for a component

* A component will use some kind of templating library to render a block of HTML. A base CSS
* It may use JavaScript to append additional HTML elements and wire up behaviours.
* A component is a JavaScript class or a Python function that returns an HTML string OR an HTML element.

## Rules for a component

### Server side rendered components

#### A component must be usable when JavaScript is disabled.

JavaScript progressively enhances such components to add functionality \(for example changing a link to open a dialog instead\).

#### When a component is progressively enhanced it  must not cause [reflow or repaint](https://javascript.tutorialhorizon.com/2015/06/06/what-are-reflows-and-repaints-and-how-to-avoid-them/)

This is important as it keeps the experience smooth.

#### No duplication.

A component should have a single source of truth.

```text
class UI:
  def goodform( btnLabel ):
     return '<div class="form">%s</div>'%self::btn(btnLabel)
  def badform( btnLabel ):
     return '<div class="form"><button class="btn">%s</button></div>'%btnLabel
  def btn( label ):
     return '<button class="btn">%s</button>'%label
```

### JS components

#### Components must be scoped and cannot have side effects on things outside their scope.

When considering the DOM tree, a component should not be able to access any parent elements. Likewise another component cannot make modifications to it. This means a component cannot bind events to document.body for example. This is important as it avoids unexpected conflicting behaviours, for example consider the following example where a list widget registers an event to close itself, but a SearchBar stops propagation preventing that event from ever occurring:

```text
function SearchBar() { $( 'body' ).on( 'click', (ev) => ev.stopPropagation() ); }
function ListWidget() { $( 'html' ).on( 'click', (ev) => { this.closeListWidget(); } ); }
new SearchBar();
new ListWidget();
```

#### Components manage their own state

We will probably want to use something like Redux to manage application state, but while we refactor and in the absence of such as library we should at least ensure that components keep their own state. This means that other components should not be changing.

```text
function SearchBar() { this.state = { foo: 1 }; }
const sb = new SearchBar();
// bad!
function Bar() { sb.state.foo = 2 }
```

#### Components are event-driven

I would like us to take an event driven approach to building out components in OpenLibrary.

The components should not make assumptions such as "clicking X saves something to localStorage". This will be left to the consumer.

This ensures that our components are as reusable as possible and that we can document them with minimum dependencies in storybook ui.

It also means the widget can be used in other contexts. For example we might want to add a search bar in a lists widget feature as well as the main header.

We may want to consider the [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern) as part of this.

```text
function SearchBar( node ) {
  node.querySelectorAll( '.buttons' ).addEventListener( 'click', onButtonClick );
}

new SearchBar( { onButtonClick: function () { alert('I clicked a button!' ); } )
```

#### Composition not inheritance

React.js and similar libraries have shown that the composition pattern is much better for UIs than the inheritance model.

[https://reactjs.org/docs/composition-vs-inheritance.html](https://reactjs.org/docs/composition-vs-inheritance.html)

#### No inheritance

We should not make use of class `extends` with the exception of a framework base class.

For instance if we are using React, it is acceptable for `class Element extends React.Component` but we should not be extending anything else e.g. `Poodle extends Dog`.

[https://codeburst.io/inheritance-is-evil-stop-using-it-6c4f1caf5117](https://codeburst.io/inheritance-is-evil-stop-using-it-6c4f1caf5117)

## Refactoring existing components

### Write unit tests

We should look to write unit tests first and for all with the existing code before doing this. To support testing we should do the minimum possible e.g. exposing functions where necessary and adding return values when we need to check the return value of something.

The existence of tests should be a precursor to any large refactor as it defines a specification of how a feature behaves and makes it easier for others in the team to verify that the new component is an adequate replacement.

### Refactor.

