# React Render Props
* **Use without React's children prop**
## Hover component
```
class Hover extends React.Component {
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
        {this.props.render(this.state.hovering)}
      </div>
    )
  }
}
```
## UI component
```
function Info (props) {
  return (
    <>
      {props.hovering === true
        ? <Tooltip id={this.props.id} />
        : null}
      <svg
        onMouseOver={this.mouseOver}
        onMouseOut={this.mouseOut}
        className="Icon-svg Icon--hoverable-svg"
        height={this.props.height}
        viewBox="0 0 16 16" width="16">
          <path d="M9 8a1 1 0 0 0-1-1H5.5a1 1 0 1 0 0 2H7v4a1 1 0 0 0 2 0zM4 0h8a4 4 0 0 1 4 4v8a4 4 0 0 1-4 4H4a4 4 0 0 1-4-4V4a4 4 0 0 1 4-4zm4 5.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z" />
      </svg>
    </>
  )
}

function TrendChart (props) {
  return (
    <>
      {props.hovering === true
        ? <Tooltip id={this.props.id}/>
        : null}
      <Chart
        type='trend'
        onMouseOver={this.mouseOver}
        onMouseOut={this.mouseOut}
      />
    </>
  )
}


function DailyChart (props) {
  return (
    <>
      {props.hovering === true
        ? <Tooltip id={this.props.id}/>
        : null}
      <Chart
        type='daily'
        onMouseOver={this.mouseOver}
        onMouseOut={this.mouseOut}
      />
    </>
  )
}

function App () {
  return (
    <>
      <Hover render={(hovering) =>
        <Info hovering={hovering}>
      } />

      <Hover render={(hovering) =>
        <TrendChart hovering={hovering}>
      } />

      <Hover render={(hovering) =>
        <DailyChart hovering={hovering}>
      } />
    </>
  )
}
```

* **Use with React's children prop**
* ## Hover component
```
class Hover extends React.Component {
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
        {this.props.children(this.state.hovering)}
      </div>
    )
  }
}
```
## UI component
```
function App () {
  return (
    <>
      <Hover>
        {(hovering) => <Info hovering={hovering}>}
      </Hover>

      <Hover>
        {(hovering) => <TrendChart hovering={hovering}>}
      </Hover>

      <Hover>
        {(hovering) => <DailyChart hovering={hovering}>}
      </Hover>
    </>
  )
}
```