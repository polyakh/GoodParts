[How To Write Better Functional Components in React](https://medium.com/better-programming/how-to-write-better-functional-components-in-react-bc974f777145)

### Memoize Complex Data(Object, Array)
```jsx
    // Bad
function SortedListView ({ title, items, comparisonFunc }) {
    const sortedItems = [...items]; // The component will re-sort the items on each re-render even if
    // the items array or the comparisionFunc does not change, but some other prop or the state changes.
    sortedItems.sort(comparisonFunc); 
    return (
        <div>
            <h1> {title} </h1>
            <ul>
                {sortedItems.map(item => <li> {item} </li>)}
            </ul>
        </div>
    );
}
// Good
function SortedListView ({ title, items, comparisonFunc }) {
    const sortedItems = useMemo(() => { // So, we can use useMemo to memoize or cache results to expensive operations by trading off some memory.
        const sortedItems = [...items];
        sortedItems.sort(comparisonFunc);
        return sortedItems;
    }, [items, comparisonFunc]);
    return (
        <div>
            <h1> {title} </h1>
            <ul>
                {sortedItems.map(item => <li> {item} </li>)}
            </ul>
        </div>
    );
} // With that being said, you do not need to use useMemo in instances where the operations are not expensive or the object is not passed into any other component, as shown in the above link.
```
> Note: Memoizing can also help prevent unnecessary re-renders when objects generated in a parent component have to be passed to a child component
> However, for components under our own control, we can always prevent unnecessary re-renders. We can do this by avoiding passing objects to the dependency array of useEffect and other similar hooks.

### Memoize Callback Functions
Just like data, we can also memoize callback functions that a component passes to other components that it renders. 
An advantage of doing this is that it will prevent useless re-renders in some cases.
[Example](https://jsfiddle.net/divinemaniac/b3cvtaoz/)
In the example above, if you go to the Result tab and type a new title in the text field, it will cause SortController to re-render. 
As a result, ascendingFn and descendingFn will be recreated. This causes comparisonFunc to change. 
Since the useMemo in SortedListView depends on comparisonFunc, it will re-sort items even if the comparisonFunc does not change logically.

We can solve this problem by wrapping ascendingFn and descendingFn in useCallback. This Hook is used to memoize functions. 
> Note that we do not pass anything in the dependency array for useCallback here because they do not rely on anything inside the component.
[Good Example](https://jsfiddle.net/divinemaniac/e9fw4L56/)

Just like useMemo, you do not need to use useCallback if the function is not passed into any component and is not a dependency for any other hook.

### Decouple Functions That Don’t Rely on the Component
Decouple Functions That Don’t Rely on the Component
Another improvement that we can make in the function above is to actually move ascendingFn and descendingFn outside of SortController. 
This is because these functions do not rely on anything inside the component. So, there is really no need to define them inside the component. 
If we do this, the component becomes more readable.  
Also, we don’t need to use useCallback anymore because these functions will not be recreated on every re-render.(It's good when passed a function into any other component)
(Example)[https://jsfiddle.net/divinemaniac/8r9nx3wp/]
We could also keep the sort utility functions in another file and import them.

### Create Subcomponents 
Creating subcomponents is a useful way to write optimized and readable React code — even with class components. 
Subcomponents divide the code base into smaller, digestible, and reusable chunks. 
This also makes it easier for React to optimize re-renders. So, dividing large components into smaller components is mostly a good idea.

### Create and Reuse Custom Hooks
Just like components, we can create custom reusable Hooks. This makes the code more readable because the code base is divided into smaller, reusable chunks. 
In our example, we can put the sorting logic inside a custom Hook called useSorted.
[Example](https://jsfiddle.net/divinemaniac/7cowkst5/)
