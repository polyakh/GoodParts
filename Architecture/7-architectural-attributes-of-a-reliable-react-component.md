[7 Architectural Attributes of a Reliable React Component](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/#1-single-responsibility)

Component-based development is productive: a complex system is built from specialized and easy to manage pieces. 
Yet only well designed components ensure composition and reusability benefits.
Make your components decoupled, focused on a single task, well tested.

Unfortunately, it’s tempting to follow the wrong path: write big components with many responsibilities, tightly couple components, forget about unit tests.
[Technical debt](https://www.nczonline.net/blog/2012/02/22/understanding-technical-debt/)

When writing a React application, I regularly ask myself:
* How to correctly structure the component?
* At what point a big component should split into smaller components?
* How to design a communication between components that prevents tight coupling?

[“Single responsibility”](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/#1-single-responsibility)
SRP a bit obscured, check out [this article](https://8thlight.com/blog/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html).
A component has a [single responsibility](https://en.wikipedia.org/wiki/Single-responsibility_principle) when it has one reason to change.
A responsibility is either **to render a list of items, or to show a date picker, or to make an HTTP request, or to draw a chart, or to lazy load an image**, etc. 
Your component should pick only one responsibility and implement it. When you modify the way component implements its responsibility (e.g. a change to limit the number of items for render a list of items responsibility) - it has one reason to change.
мсимт
Let’s follow a few examples.

__Example 1__ 
A component fetches remote data, correspondingly it has one reason to change when fetch logic changes.
A reason to change happens when:
* The server URL is modified
* The response format is modifiedчф
* You want to use a different HTTP requests library
* Or any modification related to fetch logic only.
  
__Example__ 2 
A table component maps an array of data to a list of row components, as result having one reason to change when mapping logic changes.
A reason to change occurs when:
* You have a task to limit the number of rendered row components (e.g. display up to 25 rows)
* You’re asked to show a message “The list is empty” when there are no items to display
* Or any modification related to mapping of array to row components only.
Does your component have many responsibilities? If the answer is yes, split the component into chunks by each individual responsibility. 
An alternative reasoning about the single responsibility principle says to create the component around a clearly distinguishable axis of change. 
An [axis of change attracts](https://stackoverflow.com/questions/2952662/srp-axis-of-change) modifications of the same meaning. 
An axis of change attracts modifications of the same meaning. In the previous 2 examples, the axis of change were fetch logic and mapping logic.

1.1 The pitfall of multiple responsibilities
A common oversight happens when a component has multiple responsibilities. 
At first glance, this practice seems harmless and requires less work:
* You start coding right away: no need to recognize the responsibilities and plan the structure accordingly
* One big component does it all: no need to create components for each responsibility
* No split - no overhead: no need to create props and callbacks for communication between split components.

Such naive structuring is easy to code at the beginning. 
Difficulties will appear on later modifications, as the application grows and becomes more complex.
A component that implements simultaneously multiple responsibilities has many reasons to change.
Now emerges the main problem: changing the component for one reason unintentionally influences how other responsibilities are implemented by the same component.
Unintentional side effects of are hard to predict and control.
For example, <ChartAndForm> implements simultaneously 2 responsibilities to draw a chart, and handle a form that provides data for that chart. 
<ChartAndForm> has 2 reasons to change: draw the chart and handle the form.

When you change a form field (e.g. transform an <input> into a <select>), you can unintentionally break how chart is rendered. 
Moreover the chart implementation is non reusable, because it’s coupled with the form details.

Solving multiple responsibilities issue requires to split <ChartAndForm> in 2 components: <Chart> and <Form>. 
Each chunk has one responsibility: to draw the chart or correspondingly handle the form. The communication between the chunks is done through props.
The worst case of multiple responsibilities problem is so called God component anti-pattern (an analogy of [God object](https://en.wikipedia.org/wiki/God_object)).
!! always renders same elements for same prop values


Test

That’s fine: more verbose is better than less clear.
names are <FetchAuthors>, <AuthorsContainer> or <AuthorsPage>.
https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/#72-case-study-write-self-explanatory-code
