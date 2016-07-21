# remeasure

Get position and size of the DOM element for any React Component

#### Installation

```
$ npm i remeasure --save
```

#### Usage

```javascript
// ES2015
import measure from 'remeasure';

// CommonJS
const measure = require('remeasure');

// script
var measure = window.Remeasure;

// apply it as a decorator
@measure
class MyComponent extends React.Component {
  render() {
    const {
      position,
      size
    } = this.props;
  
    return (
      <div>
        I have access to my size and position through props!
      </div>
    );
  }
}

// or as a function wrapper
const StatelessComponent = measure(({position, size}) => {
  return (
    <div>
      In here too!
    </div>
  );
});
```

ANy component that has `measure` applied to it will have the props `position` and `size` injected, which contain a variety of measurements related to (you guessed it) the component's position and size. A complete list of properties:
* Position
  * bottom (relative to window)
  * offsetLeft
  * offsetTop
  * left (relative to window)
  * right (relative to window)
  * top (relative to window)
* Size
  * clientHeight
  * clientWidth
  * height
  * offsetHeight
  * offsetWidth
  * scrollHeight
  * scrollWidth
  * width
  
The `bottom`, `left`, `right`, and `top` properties in `position` are what you would expect from the result of `element.getBoundingClientRect()`.

These properties are retrieved on mount, but will also automatically update if the element is resized thanks to [element-resize-event](https://github.com/KyleAMathews/element-resize-event).

#### Advanced usage

If you want to limit the items that are injected into the component, you can pass either a string or array of strings to the decorator before wrapping the component.

**measure(`string|array<string>`)** *returns `function`*

Examples:

```javascript
import measure from 'remeasure';

// pass a string value for a single property
const measureOnlyOffsetWidth = measure('offsetWidth');

const MyStatelessComponent = measureOnlyOffsetWidth(({size}) => {
  return (
    <div>
      Only size is injected (because no position values were requested), 
      with offsetWidth as the only property
    </div>
  );
});

// or an array of string values for multiple properties
@measure(['top', 'height'])
class MyComponent extends Component {
  render() {
    const {
      position,
      size
    } = this.props;
  
    return (
      <div>
        Both the position and size props are injected (because values
        from both position and size were requested), and each will have
        a single property on them (top on position, height on size).
      </div>
    );
  }
}

// or quickly select the complete list of either size or position
@measure('size')
class MySizedComponent extends Component {
  render() {
    const size = this.props.size;
  
    return (
      <div>
        I have the size prop injected with all properties, but not
        position.
      </div>
    );
  }
}
```

#### Browser support
`remeasure` has been tested and confirmed to work on the following browsers:
* Chrome
* Firefox
* Opera
* Edge
* IE9+

#### Development

Standard stuff, clone the repo and `npm i` to get the dependencies. npm scripts available:
* `build` => builds the distributed JS with `NODE_ENV=development` and with sourcemaps
* `build-minified` => builds the distributed JS with `NODE_ENV=production` and minified
* `compile-for-publish` => runs the `lint`, `test`, `transpile`, `dist` scripts
* `dev` => runs the webpack dev server for the playground
* `dist` => runs the `build` and `build-minified`
* `lint` => runs ESLint against files in the `src` folder
* `prepublish` => if in publish, runs `compile-for-publish`
* `test` => run ava with NODE_ENV=test
* `test:watch` => runs `test` but with persistent watcher
* `transpile` => runs Babel against files in `src` to files in `lib`