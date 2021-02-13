[Tao of React - Software Design, Architecture & Best Practices](https://alexkondov.com/tao-of-react/#validate-state-events)

## Components

### Favor Functional Components
They have a simple syntax.
No lifecycle methods, constructors or boilerplate.
Unless you need an error boundary they should be your go-to approach.

### Write Consistent Components
Stick to the same style for your components. 
Put helper functions in the same place, export the same way and follow the same naming patterns.
No matter if you’re exporting at the bottom of the file or directly in the definition of the component, pick one and stick to it.

### Name Components
Always name your components. It helps when you’re reading an error stack trace and using the React Dev Tools.

### Organize Helper Functions
Helper functions that don’t need to hold a closure over the components should be moved outside.
Move as many as possible outside and pass the values from state as arguments.
Composing your logic out of pure functions that rely only on input makes it easier to track bugs and extend.

### [Component Length](https://reactjs.org/docs/thinking-in-react.html#step-1-break-the-ui-into-a-component-hierarchy)
A React component is just a function that gets props and returns markup.
If a function is doing too many things, extract some of the logic and call another function. 
It’s the same with components - if you have too much functionality, split it in smaller components and call them instead.
Rely on props and callbacks for communication and data. Think about responsibilities and abstractions instead.

### Write Comments in JSX
When something needs more clarity open a code block and give additional information.
```jsx
const Component = (props) =>  <>
    {/* If the user is subscribed we don't want to show them any ads */}
    {user.subscribed ? null : <SubscriptionPlans />}
</>
```
### Use Error Boundaries
An error in one component shouldn’t bring down the entire UI. 
There are rare cases in which we want to take down the whole page or redirect if a critical error happens.

### Number of Props
A high number of props is a signal that a component is doing too much.
The number of props that a component has is correlated to how much it’s doing. 
The more props you pass to it the more responsibilities it has.
An input field, for example, may have a lot of props. In others it’s a sign that something needs to be extracted.
> Note: The more props a component takes, the more reasons to rerender.

### Pass Objects Instead of Primitives
This also reduces the changes that need to be done if the user gets an extra field for example.
```jsx
    // 👍 Use an object that holds all of them instead
    <UserProfile user={user} />
```

### [Conditional Rendering](https://kentcdodds.com/blog/use-ternaries-rather-than-and-and-in-jsx)
```jsx
    const Component = () => {
    const count = 0;
        return (
            <>
                {count && <h2>Messages: {count}</h2>}  {/* 👎 Try to avoid short-circuit operators. jsx => 0  */}
                {count ? <h2>Messages: {count}</h2> : null}  {/* 👎 Try to avoid short-circuit operators */}
            </>);
}
```
### Move Lists in a Separate Component
The parent component doesn't need to know about the details, only that it’s displaying a list.
```jsx
// 👍 Extract the list in its own component
function Component({ topic, articles }) {
  return (
    <div>
      <h1>{topic}</h1>
      <ArticlesList articles={articles} />
    </div>
  )
}
```
### Assign Default Props When Destructuring
Prefer assigning default values directly when you’re destructuring the props. It makes it easier to read the code from top to bottom without jumping and keeps the definitions and values together.
```jsx
// 👍 Place them in the arguments list
function Component({ title = '', tags = [], subscribed = false }) {
  return <div>...</div>
}
```
### Avoid Nested Render Functions
When you need to extract markup from a component or logic, don’t put it in a function living in the same component.
This means that it will have access to all the state and data of its parent.
Move it in its own component, name it and rely on props instead of a closure.

## State Management

## Use Reducers
Start with useReducer before you reach for an external library. 
This is a great mechanism to do complex state management and it doesn’t require 3rd party dependencies.
```jsx
// 👍 Unify them in a reducer instead
const TYPES = {
  SMALL: 'small',
  MEDIUM: 'medium',
  LARGE: 'large'
}

const initialState = {
  isOpen: false,
  type: TYPES.LARGE,
  error: null
}
const reducer = (state, action) => {
    return state[action.type] ?? 'defaulValue';
}
function Component() {
    const [state, dispatch] = useReducer(reducer, initialState)
    return ()
}
```
### Prefer Hooks to HOCs and Render Props(a component is a function that uses other functions.)
In some cases we need to enhance a component or give it access to external state. => HOC, render props && hooks
Hooks allow you to tap into multiple sources of external functionality without them conflicting with each other. 
No matter the number of hooks, you know where each value comes from.

With HOCs you get values as props. This makes it unclear wether they come from the parent component or the wrapping one.

Render props lead to high indentation and bad readability.

### Use Data Fetching Libraries
Often the data that we want to manage in state is retrieved from an API. We need to keep that data in memory, update it and access it in multiple places.
Modern data fetching libraries like React Query, SWR.
It’s even easier if you’re working with a GraphQL client like Apollo.

### State Management Libraries
They should be used in large applications that require managing complex state.
Recoil and Redux.


## Component Mental Models

### Container & Presentational(smart and dumb)
They are just called by the parent component with some props. The container components contain the business logic, do the data fetching and manage the state.
This mental model is what the MVC structure is for back-end applications. 
It’s generic enough to work everywhere and you can’t go wrong with it.
But, in modern UI applications that pattern falls short. Pulling all the logic in a few components leads to bloat.
They end up with too many responsibilities and become hard to manage.
The container/component structure is not wrong but it’s too generic. It doesn’t tell the reader anything about the project besides that it uses React.

### Stateless & Stateful
Data should live close to where it is used. When you’re using a GraphQL client you fetch the data in the component that displays it. 
Even if it’s not a top level one. Don’t think about containers, think about responsibilities. Consider what is the most logical component to hold a piece of state.

### Application Structure
Not all components are equal - some are used globally, others are made for a specific part of the app.
Group by route/module from the start. This is a structure that supports change and growth. 
The point is not to have your application outgrow the architecture quickly. 
If it’s based on components and containers that will happen too fast.
```markdown
// 👍 Group by module/domain
├── modules
|   ├── common
|   |   ├── components
|   |   |   ├── Button.jsx
|   |   |   ├── Input.jsx
|   ├── dashboard
|   |   ├── components
|   |   |   ├── Table.jsx
|   |   |   ├── Sidebar.jsx
|   ├── details
|   |   ├── components
|   |   |   ├── Form.jsx
|   |   |   ├── ItemCard.jsx
```

### Create a Common Module
Components like buttons, inputs and cards are used all over the place. Even if you’re not going with a module based structure it’s good to extract those.

You can see what common components you have even if you’re not using Storybook. It helps to avoid duplication.

### Use Absolute Paths
Making things easier to change is fundamental for your project structure. Absolute paths mean that you will have to change less if you need to move a component. 
Also it makes it easier to find out where everything is getting pulled from.
[scope](https://docs.npmjs.com/cli/v7/using-npm/scope)
```jsx
// 👍 Absolute ones don't change
import Input from '~modules/common/components/Input'; // with ~ as well.
```

### Wrap External Components
Try not to import too many 3rd party components directly. By creating an adapter around them we can modify the API if we have to. 
Also, we can change the library in a single place.
This goes for component libraries like Semantic UI and utility components as well. 
The simplest thing you can do is reexport them from the common module so they’re pulled from the same place.
A component doesn’t need to know what library we’re using for the date picker - only that it exists.
```jsx
// 👍 Export the component and use it referencing your internal module
import { Button, DatePicker } from '@modules/common/components'
```

### Move Components in Folders
I create a components folder for each module in my React applications.
As a general practice it’s good to have an index.js
Still, keep the component file with its name so you don’t get confused when you have multiple ones open.
```markdown
// 👍 Move them in their own folder
├── components
    ├── Header
        ├── index.js
        ├── Header.jsx
        ├── Header.scss
        ├── Header.test.jsx
```

## Performance

### Don't Optimize Prematurely
Before you make any kinds of optimizations, make sure that there is a reason for them.
Yes, it’s important to be aware of certain things but prioritize building readable and maintainable components before performance. 
Well written code is easier to improve.

When you notice a performance problem in your application - measure and identify the cause of your problem. No point in trying to reduce rerender count if your bundle size is enormous

Once you know where the performance problems are coming from, fix them in the order of their impact.

### Rerenders - Callbacks, Arrays and Objects
It’s good to try and reduce the amount of unnecessary rerenders that your app makes. 
Keep this in mind but also note that unnecessary rerenders will rarely have the greatest impact on your app.
The most common advice is to avoid passing callback functions as props.(a new function will be created each time, triggering a rerender.)
Is`s very rarely when we have problems any performance problems with callbacks and in fact that’s my go to approach.

If you are experiencing performance problems and the closures are the cause then remove them. But don’t make your code less readable ot more verbose unnecessarily.

Passing down arrays or objects directly falls into the same category of problems. They fail the reference check so they will trigger a rerender. 
If you need to pass a fixed array extract it as a constant before the component definition to make sure the same instance is passed each time.
