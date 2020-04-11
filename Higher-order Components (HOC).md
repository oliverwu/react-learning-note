# Higher-order Component
## Higher-Order Function 
* Is a function 
* Takes in a callback function as an argument 
* Returns a new function 
* The function it returns can invoke the original callback function that was passed in

```
function higherOrderFunction (callback) {
  return function () {
    return callback()
  }
}
```
### Example

```
function add (x, y) {
  return x + y
}

function makeAdder (x, addReference) {
  return function (y) {
    return addReference(x, y)
  }
}

const addFive = makeAdder(5, add)
const addTen = makeAdder(10, add)
const addTwenty = makeAdder(20, add)

addFive(10) // 15
addTen(10) // 20
addTwenty(10) // 30
```

## Higher-Order Component
* Is a component
* Takes in a component as an argument
* Returns a new component
* The component it returns can render the original component that was passed in
  
```
function higherOrderComponent (Component) {
  return class extends React.Component {
    render() {
      return <Component />
    }
  }
}
```

> Take in a “Component” argument.
```
function withHover (Component) {

}
```

> Return a new component
```
function withHover (Component) {
  return class WithHover extends React.Component {

  }
}
```

> Render the “Component” argument passing it a “hovering” prop.
```
function withHover(Component) {
  return class WithHover extends React.Component {
    constructor(super) {
      super(props)

      this.state = {
        hovering: false
      }

      this.mouseOver = this.mouseOver.bind(this)
      this.mouseOut = this.mouseOut.bind(this)
    }
    mouseOver() {
      this.setState({hovering: true})
    }
    mouseOut() {
      this.setState({hovering: false})
    }
    render() {
      return (
        <div onMouseOver={this.mouseOver} onMouseOut={this.mouseOut}>
          <Component hovering={this.state.hovering} />
        </div>
      );
    }
  }
}
```

> Pass the original component to our withHover higher-order component.
```
const InfoWithHover = withHover(Info)
const TrendChartWithHover = withHover(TrendChart)
const DailyChartWithHover = withHover(DailyChart)
```

## what if the component already had a prop named `hovering`? We’d have a naming collision.
> Now we’ve set the default prop name to hovering (via ES6’s default parameters), but if the consumer of withHover wants to change that, they can by passing in the new prop name as the second argument.
```
function withHover(Component, propName = 'hovering') {
  return class WithHover extends React.Component {
    constructor(super) {
      super(props)

      this.state = {
        hovering: false
      }

      this.mouseOver = this.mouseOver.bind(this)
      this.mouseOut = this.mouseOut.bind(this)
    }
    mouseOver() {
      this.setState({hovering: true})
    }
    mouseOut() {
      this.setState({hovering: false})
    }
    render() {
      const props = {
        [propName]: this.state.hovering
      }
      return (
        <div onMouseOver={this.mouseOver} onMouseOut={this.mouseOut}>
          <Component {...props} />
        </div>
      );
    }
  }
}

function Info ({ showTooltip, height }) {
  return (
    <>
      {showTooltip === true
        ? <Tooltip id={this.props.id} />
        : null}
      <svg
        className="Icon-svg Icon--hoverable-svg"
        height={height}
        viewBox="0 0 16 16" width="16">
          <path d="M9 8a1 1 0 0 0-1-1H5.5a1 1 0 1 0 0 2H7v4a1 1 0 0 0 2 0zM4 0h8a4 4 0 0 1 4 4v8a4 4 0 0 1-4 4H4a4 4 0 0 1-4-4V4a4 4 0 0 1 4-4zm4 5.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z" />
      </svg>
    </>
  )
}

const InfoWithHover = withHover(Info, 'showTooltip')
```

## Pass value to the Component created by HOC
```
render() {
  const props = {
	  [propName]: this.state.hovering,
    ...this.props,
  }

  return (
    <div onMouseOver={this.mouseOver} onMouseOut={this.mouseOut}>
      <Component {...props} />
    </div>
  );
}
```