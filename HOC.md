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