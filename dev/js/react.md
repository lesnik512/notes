## Controled and uncontrolled components

```js
// Controlled:
<input type="text" value={value} onChange={handleChange} />

// Uncontrolled:
<input type="text" defaultValue="foo" ref={inputRef} />
// Use `inputRef.current.value` to read the current value of <input>
```

## Suspense component

```js
const OneComponent = React.lazy(() => import('./OneComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OneComponent />
      </Suspense>
    </div>
  );
}
```

## Methods order when component re-rendered
- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentDidUpdate()

## Other
- `React.PureComponent` is exactly the same as `React.Component` except that it handles the `shouldComponentUpdate()` method for you. When props or state changes, `PureComponent` will do a shallow comparison on both props and state.
- `Synthetic Event` is a cross-browser wrapper around the browser's native event. It's API is same as the browser's native event, including `stopPropagation()` and `preventDefault()`, except the events work identically across all browsers.
- `Portal` is a recommended way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.
