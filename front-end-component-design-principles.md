[Front end component design principles](https://engineering.carsguide.com.au/front-end-component-design-principles-55c5963998c9)
Components should have plenty of thought put into their design so that they can be reusable, composable, [discrete](https://medium.com/zestgeek/a-discrete-way-to-write-reactjs-components-58e1330a88b7) and loosely coupled, but functional enough 
to be sufficient in and of themselves when leveraged within and outside their intended use cases.
Such a design is easier said than done and we don’t always have time to plan things the way we want.
### Hierarchy and class diagrams
Your components collectively form a tree structure and it’s a good idea to create a visual representation of this tree during design. 
It helps you get a good overall idea of your application layout. A good way to represent these entities is component diagrams.
UML for front end components, the diagram can show:
* State
* Props
* Methods
* Relationship to other components 
  
So, let’s take a look at a basic hierarchical component diagram plan for a simple table component that will render an array of data objects. 
The component will consist of a total row count display, a header row, some data rows, and the ability to sort on a column when clicking its header cell. 
In its props it will be passed the list of columns (which have a property name and a human readable version of that property), and then an array of data. 
We can add in an optional ‘on row click’ function for funsies.
![UML](https://miro.medium.com/max/1050/0*BpUkUiLupqoBO3ux)
While something like this may seem a bit much, and can definitely be quite involved for large applications, it has many advantages. 
A big one is that it forces you to think about specifics before you start writing any code, like what type of data each component needs, what methods it will have to implement, what the required state properties are, and so on.

Once you have the general idea of how to build a component (or group of components), it’s easy to think that when you actually start coding it’ll flesh 
itself all out neatly and as you expected, but there’ll almost always be things you didn’t accommodate for. 
You don’t want to have to redo parts of your project or use messy work arounds as a result. Other advantages to these diagrams include:
* A easy to understand view of component composition and association
* An easy to understand overview of your application UI hierarchy
* A view of your hierarchy’s data and how it flows
* A snapshot of a component’s functional responsibilities
* Relatively easy to create using diagram software

### Flat, data-oriented state/props
If you have nested data your performance can suffer, through things such as unnecessary re-rendering from shallow equality checks.
In libraries which involve immutability, like React, you have to create copies of state rather than changing them directly like in Vue, and doing so with nested data can create awkward, ugly code.

Flat props also make it nice and clear what data values a component is working with.
If you pass in an object then you have no idea what its properties are, and so finding out what the props of the component actually are is extra work
If you flatten the object out, it’s much quicker to see what you’re working with.
![Example](https://miro.medium.com/max/3600/0*oWFoKOVdtDEiOt5t)
The state/props should also contain just the data needed to render your markup.
You shouldn’t store entire components in the state/props and render straight from there.
(In addition, for data heavy applications, data [normalisation](https://github.com/paularmstrong/normalizr) can have huge benefits and you may want to consider doing that in addition to flattening).

### State change purity
Changes to state should usually be in response to some kind of event, like the user clicking a button, or an API response returning. 
They shouldn’t be in response to other state changes, as this chaining can create component behaviour that is hard to understand and maintain. 
**State changes should be free of side effects.**

### Loose coupling
A core idea of components is that they are reusable, and for that they have to be functional and complete in and of themselves
‘Coupling’ is a term which refers to the dependence of entities on one another.
Loosely coupled entities should be able to function by themselves, without relying on other modules
In terms of front end components, the main part of coupling is how much a component’s functionality relies on its parent and the props it’s passed, 
and what children it renders (as well as imports, like 3rd party modules or custom scripts).

Tightly coupled components tend to be more work to reuse, don’t function properly when not children of their original parent component, 
have a child or series of children that only make sense in their original context, and lead to code duplication as they are overfitted to their original use case.

When designing a component you should try and think of a general use case, rather than the specific use case it was originally made to satisfy. 
While some components are obviously going to be for a specific purpose only, and that’s ok, plenty will have wider applicability if you approach them with a broad view when designing them.

et’s have a look at a simple React use case where you want to make a list of links for navigating around your site, with a logo displayed. 
In this case we’ll be looking at unbinding a component from the context it was originally designed. Here’s the initial version:
![Example](https://miro.medium.com/max/561/0*YdWfhl9HILY99Di0)
While this may fulfil the initial intended use case, it’s not reusable in any situation except the initial context it was made for.

Let’s make a more reusable component:
![Example](https://miro.medium.com/max/960/0*SRwtEhiEfcFEneFa)
Here we can see that while it does have the original links and logo as the default values, we can pass in props to override them. 
So let’s say we want to use create special use case for admins:
![Example](https://miro.medium.com/max/689/0*bgK5B8oRXhMQJQWG)
No need for a new component! And we can dynamically build that link array too if we wanted, which solves our use case of users having custom link lists.
We can still note that this component isn’t bound to any specific parent nor children.
Now this component is much more reusable beyond its original context.
Assuming it doesn’t serve a highly specific, one-off use case, the ultimate goal of designing a component is that it be loosely coupled from its parent, renders generic and logical child elements, and not be bound to the context from which it originated.

### Auxiliary code separation
This one may be less academic, but I still feel it’s important enough to mention. 
Physically interacting with your code base is part of software engineering, and sometimes some basic organisational principles can really make things smoother. 
When working on a code base for 8 hours a day for weeks, small changes can make a big difference. 
One such organisational principle is the idea of separating out auxiliary code into its own file so that you don’t have to deal with it when working on your component. 
Such code includes, but is not limited to, things like:
* Configuration code
* Dummy data
* Large amounts of non-technical documentation

Not only is it messy and annoying to have to scroll over this stuff when trying to work on the core code of your component, it can exacerbate bias. 
When working on components you want them to be as generic and reusable as possible. Seeing the specific information pertaining to a component’s current context can make it difficult to think of the component’s usage beyond its original use case.

### [View distillation](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)
While it may be challenging, a good way to develop components is to make them contain the bare minimum Javascript needed to render them. 
Everything extraneous, like data fetching, data munging or event handling logic should ideally be made generic and moved into external scripts or lifted up into a common ancestor.

This boils the component down into the ‘view’ part of it, i.e. what you see (the markup and styling). 
The Javascript within it is only there to help render the view, with maybe a little extra logic specific to that component
Anything beyond that, like API calls, non-specific formatting of values (e.g. currency or time) or munging of data that’s 
reused across components, can be moved to external scripts of lifted up.

Good general design techniques employed as moving the generic munging function to an external script and not embedding the hard coded data.
We can lifting up the data and making the event handling passed in as props, so that the components simply render the data and don’t encapsulate any other logic.
![Example](https://miro.medium.com/max/1050/0*_OM8O-ONfOtk8mqP)

### Timely modularisation
While trying to be more proactive about breaking up your code into loosely coupled, reusable chunks is a good thing, it’s certainly possible to go overboard. 
Not every little bit of markup needs to be its own component, not every little bit of logic needs to be pulled out to an external script.

1. Is there enough markup/logic to warrant it? 
   If it’s just a few lines of code, you could end up creating more code separating it than just leaving it in.
2. Is the code repeated (or likely to be repeated)? 
   If something is used once and only once, and serves a specific use case that is unlikely to be used elsewhere, it can potentially be better just to leave it embedded. 
   If need be you can always separate it out later (but do NOT use that as an excuse to never do it).
3. Is your performance suffering? Changing state/props causes re-rendering, and when this happens you only want the relevant elements to be diffed and re-rendered. 
   In larger, undifferentiated components, you may find that state changes cause re-rendering in a whole lot of places where it isn’t needed, and your performance can start to suffer.
4. *Are you having trouble testing all parts of your code? You want to be able to test a variety of things, like that components work regardless of 
   their context, and that all your Javascript logic works as intended. This can be hard when your elements have a single, 
   assumed context or if you embed a whole bunch of logic into a single function, respectively. Rendering tests can also become unwieldy if testing a single, giant component with lots of markup and styling.   
5. Do you have a clear rationale? 
   When splitting up your code you should be thinking about what exactly it achieves. 
   Does this allow for looser coupling? Is the chunk I’m breaking off a discrete entity that logically makes sense being on its own? 
   Is this code ever actually likely to be reused elsewhere? If you can’t answer this question clearly, then you’re just splitting up code into (potentially tightly coupled) chunks for the sake of it, which can cause problems.
6. Do the benefits outweigh the cost? Separating out code inevitably takes time and effort, the amount of which vary depending on the specific scenario, and there’s many factors which will come into play
   when ultimately making this decision (such as the points in this list, and many more). 
   Doing some research on the cost and benefits of abstraction in general can help give you an idea of some factors to keep in mind when making this decision for your code. 
   Ultimately I included this point because it’s easy to forget the effort required if we focus too hard on the advantages. Weigh everything up and make an informed decision.
   
### Centralised/shared state accommodation
A lot of larger applications use centralised store tools like Redux or Vuex (or have state sharing setups like React’s Context API). 
This means they’re getting props from the store and not passed in via the parent. 
*When thinking about reusability of components you always need to consider not only the direct, parental props, but the store ones too. 
If you used that component in another project, you’d need those values in the store. Or maybe the other project doesn’t use a centralised store tool at all, 
and you’d have to transpile it into a form where those are now passed as parental props.

Since hooking a component up to the store (or context provider) is easy and can be done regardless of the component’s hierarchical location, it’s easy to quickly create a lot of tight coupling between the store and a web of components all over your hierarchy. 
Usually hooking up the component to the store is just a few lines, then it’s a single extra line for each property/function you attach. 
Just remember that while this kind of coupling may be easy, it’s implications are no different, and you should think through ways to mitigate the risk just as you would with parental props.
