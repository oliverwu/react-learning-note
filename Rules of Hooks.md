# Rules of Hooks
```
function Counter () {
  // 👍 from the top level function component
  const [count, setCount] = React.useState(0)

  if (count % 2 === 0) {
    // 👎 not from the top level
    React.useEffect(() => {})
  }

  const handleIncrement = () => {
    setCount((c) => c + 1)

    // 👎 not from the top level
    React.useEffect(() => {})
  }

  ...
}
```

```
function useAuthed () {
  // 👍 from the top level of a custom Hook
  const [authed, setAuthed] = React.useState(false)
}
```

```
class Counter extends React.Component {
  render () {
    // 👎 from inside a Class component
    const [count, setCount] = React.useState(0)
  }
}
```

```
function getUser () {
  // 👎 from inside a normal function
  const [user, setUser] = React.useState(null)
}
```
