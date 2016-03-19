# react-toggles

## Introduction
React Toggles is a proof of concept of a Higher Order Component to implement feature toggles.

## How to use it
First of all, create your component as if it were going to receive by props an array with feature toggles:

```javascript
import React, { Component } from 'react';

class MyComponent extends Component {
  render() {
    let myToggle = this.props.toggles.find(t => {
      let {myToggle} = t;
      return myToggle
    });

    if (!myToggle)
      return <div>Toggle not active</div>;
    return <div>Toggle active</div>;
  }
}
```

The component will receive a prop named `toggles` with an array of toggles, similar to this:

```javascript
[{
  myToggle: true
}, {
  myDisabledToggle: false
}, {
  yetAnotherToggle: true
}]
```

In the previous example, the component will only make use of `myToggle`... But how does it receive it? This is where `react-toggles` is useful.

First, wrap your component into the higher order component `ReactToggle`:

```javascript
import { ReactToggle } from "../src";
...
export default ReactToggle(MyComponent);
```

This will export your component as a `toggle-able` component.

Secondly, the user of your component will need to provide the toggles somehow. It's up to you how to decide which toggles are activated or not, but you must provide the component a `promise` property with a `Promise` function that returns an array of feature toggles on being fullfilled:

```javascript
ReactDom.render(<MyComponent promise={
  new Promise(resolve => {
    resolve([{myToggle: true}]);
  })
}
/>, document.getElementById('main'));
```