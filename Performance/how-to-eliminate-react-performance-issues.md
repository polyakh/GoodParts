##[How to Eliminate React Performance Issues](https://medium.com/@ohansemmanuel/how-to-eliminate-react-performance-issues-a16a250c0f27)

### 1. Extract Frequently Updated Regions into Isolated Components
Once you’ve visually noted wasted renders in your application, a good place to start looking is to attempt to break up your component tree to support regions updated frequently.

Essentially, what’s happening is that whenever the user profession is changed by clicking the button, the description prop is changed. 
This change in props then causes the App component to be re-rendered entirely.
We could create a new component called Profession which renders its own DOM elements.
Profession => Store

### 2. Use Pure Components when Appropriate

### 3. Avoid Passing New Objects as Props
A new object isn’t created at render time.

### 4. Use the Production Build

### 5. Employ Code Splitting
