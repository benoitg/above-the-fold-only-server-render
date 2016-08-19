# skip-server-render

A React component wrapper for optionally skipping SSR.

## Installation

```
npm install skip-server-render
```

## Usage

By default, the `SkipServerRender` component does nothing and simply returns the child component.
You can tell the component to skip server rendering either by passing a prop `skip={true}` or
setting up `skipServerRender` in your app context and passing the component a `contextKey` prop.

You can skip server side rendering by passing a skip prop:

```js

const SomeComponent = () => {
  return (
    <SkipServerRender skip={true}>
      <div>This will not be server side rendered.</div>
    </SkipServerRender>
  );
};
```

You can also skip server side rendering by setting context and passing a contextKey prop:

```js

const SomeComponent = () => {
    return (
      <SkipServerRender contextKey="skipServerRender.SomeComponent">
        <div>This will not be server side rendered based on the context.</div>
      </SkipServerRender>
    );
};

class SomeApp extends React.Component {
  getChildContext() {
    return {
      skipServerRender: {
        SomeComponent: true
      }
    };
  }

  render() {
    return (
      <SomeComponent />
    );
  }
}

SomeApp.childContextTypes = {
  skipServerRender: React.PropTypes.shape({
    AnotherComponent: React.PropTypes.bool
  })
};

```

## Performance

`SkipServerRender` helps performance both by decreasing the load on `renderToString` and sending the end user a smaller amount of markup.
The following table outlines a clear performance increase in the `example` app by skipping server rendering on
the `Footer` component and several other below the fold zones.

|          | HTML Size      | renderToString Time |
| -------- | -------------- | ------------------- |
| before   | 452 kB         | 249 ms              |
| after    | 315 kB         | 177 ms              |
| diff     | -137 kB (-30%) | -72 ms (-29%)       |


## Development Guide

We have an ever-green guide to our development practices with this archetype.
[Click here](https://github.com/electrode-io/electrode-archetype-react-component/blob/master/DEVELOPMENT.md)
before starting development on a component library.

## Scripts

To run the demo:

```
gulp demo
```

To view the demo, navigate to `http://localhost:4000`

To view the demo with hot reload enabled, navigate to `http://localhost:4000/webpack-dev-server/`

To run tests:

```
gulp test
```

To build /lib:

```
gulp build
```

## npm link

When using npm link, you must delete react from `zeus-components-layout/node_modules/`. This is because npm link is just a symlink, not a proper `npm install`.

You must also run `gulp build`
