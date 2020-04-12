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

* **useWindowDimensions**
```
import React, { useState, useEffect } from "react"
import ReactDOM from "react-dom"

import "./styles.css"

/*
  Instructions:
    You're given a `useWindowDimensions` custom Hook. Your
    job is to finish implementing it. It should return
    an object with a `width` property that represents the current
    width of the window and a `height` property which represents
    the current height. 

    To get those values, you can use teh `window.addEventListener`
    API.https://developer.mozilla.org/en-US/docs/Web/API/Window/resize_event
*/

function useWindowDimensions() {
  const [width, setWidth] = useState(window.innerWidth)
  const [height, setHeight] = useState(window.innerHeight)

  function reportWindowSize() {
    setWidth(window.innerWidth)
    setHeight(window.innerHeight)
  }

  useEffect(() => {
    window.addEventListener('resize', reportWindowSize)
    return () => {
      window.removeEventListener('resize', reportWindowSize)
    }
  }, [])

  return {width, height}
}


function App() {
  const { width, height } = useWindowDimensions()

  return (
    <div className="App">
      <h2>width: {width}</h2>
      <h2>height: {height}</h2>
      <p>Resize the window.</p>
    </div>
  )
}

const rootElement = document.getElementById("root")
ReactDOM.render(<App />, rootElement)
```

* **useFetch**
```
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";
import axios from 'axios';

import "./styles.css";

/*
  Instructions:
    Implement the `useFetch` function. 
*/

function useFetch (url) {
  const [loading, setLoading] = React.useState(true)
  const [data, setData] = useState(null)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    setLoading(true)
    const fetchData = async () => {
      try {
        const response = await axios.get(url)
        const data = response.data
        // setLoading(false)
        setData(data)
        setError(null)
        setLoading(false)
      } catch(e) {
        setError(e)
        setLoading(false)
      }
      
    }
   fetchData()
  }, [url])



  return {
    loading,
    data,
    error
  }
}

const postIds = [1,2,3,4,5,6,7,8]

function App() {
  const [index, setIndex] = React.useState(0)


  const { loading, data: post, error }= useFetch(
    `https://jsonplaceholder.typicode.com/posts/${postIds[index]}`
  )
  console.log(loading, post)

  const incrementIndex = () => {
    setIndex((i) => 
      i === postIds.length - 1
        ? i
        : i + 1
    )
  }

  if (loading === true) {
    return <p>Loading</p>
  }

  if (error) {
    return (
      <React.Fragment>
        <p>{error}</p>
        <button onClick={incrementIndex}>Next Post</button>
      </React.Fragment>
    )
  }

  return (
    <div className="App">
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      {index === postIds.length - 1 
        ? <p>No more posts</p>
        : <button onClick={incrementIndex}>
            Next Post
          </button>}
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```