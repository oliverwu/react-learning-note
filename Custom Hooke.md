# Creating Custom Hooks

* **Build useHover Hook**
```
function useHover () {
  const [hovering, setHovering] = React.useState(false)

  const mouseOver = () => setHovering(true)
  const mouseOut = () => setHovering(false)

  const attrs = {
    onMouseOver: mouseOver,
    onMouseOut: mouseOut
  }
  return [hovering, attrs]
}
```
* **Use useHover Hook**
```
function Info ({ id, size }) {
  const [hovering, attrs] = useHover()

  return (
    <React.Fragment>
      {hovering === true
        ? <Tooltip id={id} />
        : null}
      <svg
        {...attrs}
        width={size}
        viewBox="0 0 16 16" >
          <path d="M9 8a1 1 0 0 0-1-1H5.5a1 1 0 1 0 0 2H7v4a1 1 0 0 0 2 0zM4 0h8a4 4 0 0 1 4 4v8a4 4 0 0 1-4 4H4a4 4 0 0 1-4-4V4a4 4 0 0 1 4-4zm4 5.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z" />
      </svg>
    </React.Fragment>
  )
}
```

## Example
* **Build useLocalStorage Hook**
```
fucntion useLocalStorage(key) {
	let localStorageItem;
  if (key) {
    localStorageItem = localStorage(key);
  }
  const [localState, updateLocalState] = useState(
    localStorageItem    
  )
  function syncLocalStorage(event) {
    if (event.key === key) {
      updateLocalState(event.newValue)
    }
  }
  useEffect(() => {
    // This won't work on the same page that is making the changes — it is really a way for other pages on the domain using the storage to sync any changes that are made
    window.addEventListener(
      "storage", syncLocalStorage
    )

    return () => {
      window.removeEventListener(
        "storage",
        syncLocalStorage
      )
    }
  }, [])
  return localState
}
```

## Practise
* **useWait**
```
import React, {useState, useEffect}from "react";
import ReactDOM from "react-dom";

import "./styles.css";

/*
  Instructions:
    Finish implementing the `useWait` custom Hook.
    `useWait` should return a boolean that changes from
    `false` to `true` after `delay` seconds. 
*/

function useWait (delay) {
  const [state, setState] = useState(false)

  useEffect(() => {
    const id = setTimeout(() => {
      setState(true)
    }, delay)
    return () => window.clearTimeout(id)
  }, [delay])

  return state
}

function Wait({ delay = 1000, placeholder, ui }) {
  const show = useWait(delay)

  return show === true
    ? ui
    : placeholder
}

function App() {
  return (
    <div className="App">
      <Wait
        delay={3000}
        placeholder={<p>Waiting...</p>}
        ui={<p>This text should appear after 3 seconds.</p>}
      />
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```