[##5 things you didn’t know about React DevTools](https://blog.logrocket.com/5-things-you-didnt-know-about-react-devtools-2c6e0ef22529/)

### Interact in the console
Select a component in the React DevTools, and pop open the console (hitting the escape key lets you see both at once). 
Type in $r, and you’ll have access to the instance of that component!

You can trigger callback methods, inspect the state and even lets you augment functionality directly from the console. Pretty neat!
If you’re interested in a particular function (like a click handler or callback function), React DevTools also provide you with a nice little convenience-feature. 
If you right-click a function in the props or state overview, you get the option of making it available as a global variable! This way, you can call it whenever you want, with different arguments and contexts.

[### View source like a pro](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-react-jsx-source)
Babel provides a plugin that adds a __source prop to all JSX elements, that lets the React DevTools link directly to the source code of the component you’re inspecting. 
It slows down your application a bit, but it works great in development mode.


### Use secret React APIs
In some situations, understanding why a particular commit or re-render took longer can be a bit complicated. 
That’s why the React DevTools are introducing support for so-called interaction tracking. 
This is a programmatic API that lets you place labels on different events in your app, and trace their performance.
[scheduler](https://gist.github.com/bvaughn/8de925562903afd2e7a12554adcdda16)


