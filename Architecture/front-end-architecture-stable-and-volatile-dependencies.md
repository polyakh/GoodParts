## [Front-end Architecture: Stable and Volatile Dependencies](https://dmitripavlutin.com/frontend-architecture-stable-and-volatile-dependencies/)

Designing correctly the dependencies is an important skill to architect Front-end applications. 


### 1. Stable dependencies
Examples of `stable` dependencies are the utility libraries like lodash, ramda.
Moreover, the JavaScript language itself provides:
 * Utility functions, like Object.keys(), Array.from()
 * Methods on primitives and objects, like string.slice(), array.includes(), object.toString()

All the built-in functions that the language provides are also considered stable dependencies.
   
However, aside from stable ones, some dependencies may change under certain circumstances. Such volatile dependencies have to be segregated from stable ones and designed differently.

### 2. Volatile dependencies
To determine whether loggedIn cookie is set-up, you have to consider the `environment where the application runs`.
On the client-side, you can access the cookie from document.cookie property, while on the server-side you’d need to read the HTTP request header cookie.

Generally, the dependency is volatile if any of the following criteria are met:
* The dependency requires runtime environment setup for the application (network access, web services, file system)
* The dependency is in development
* The dependency has non-deterministic behavior (random number generator, access of current date, etc).

Another example of volatile dependency is the library to access a database or a fetching library that accesses the network.

Even a dependency that is still in development or one you can probably change for an alternative solution in the future can also be volatile.

A good rule of thumb to distinguish a volatile dependency is to analyze how easy you can unit test the component that depends on it. 
If the dependency requires a lot of setup ceremony and mocks to be tested (e.g. a fetching library requires mocking network requests), then most likely it’s a volatile.

#### 2.1 A bad design
Page component depends directly on both cookieClient and cookieServer libraries.

Why implementing the cookie management volatile dependency such way is a problem? Let’s see:

* Tight coupling to all dependency implementations. The component Page depends directly on cookieClient and cookieServer implementations
* Dependency on the environment. Every time you need the cookie management library, you have to invoke the expression typeof 
  window === 'undefined' to determine whether the app runs on the client or server-side, and choose according to cookie management implementation.
* Unnecessary code. The client-side bundle is going to include the cookieServer library which isn’t used on the client-side. And vice-versa for server-side.
* Difficult testing. The unit tests of Page component would require lots of mockups like setting window variable and mockup document.cookie

#### 2.2 A better design
The idea consists in applying the Dependency Inversion Principle and decouple Page component from cookieClient and cookieServer. 
Instead, let’s make the Page component depend on an abstract interface Cookie.
First, let’s define an interface Cookie that describes what methods a cookie library should implement,
define the React context that’s going to hold a specific implementation of the cookie management library
CookieContext injects the dependency into the Page component.

The only thing that Page component knows about is the Cookie interface, and nothing more. The component is decoupled from the implementation details of how cookies are accessed.

Page component doesn’t care about what concrete implementation it gets. The only requirement is that the injected dependency to conform to the Cookie interface.
The concrete implementation of a volatile dependency is composed close to the bootstrap (or main) scripts of the application. This place is named Composition Root.

The benefits of good design of volatile dependencies:
* Loose coupling. The component Page doesn’t depend on all possible implementations of the dependency
* Free of implementation details and environment. The component doesn’t care whether it runs on the client or server-side
* Dependency upon stable abstraction. The component depends only on an abstract interface Cookie
* Easy testing. The component knows only about the interface, you can easily test such a component by injecting dummy implementations using context.


Links:
* [Composition Root.](https://stackoverflow.com/a/6277806)
* [Prop drilling](https://kentcdodds.com/blog/prop-drilling)

#### 2.3 Be aware of added complexity
The improved design requires more moving parts: a new interface that describes the dependency and a way to inject the dependency.

All these moving parts add complexity. So you should carefully consider whether the benefits of this design outweigh the added complexity.

#### 2.4 Injection mechanisms
While in the the previous example React context was injecting the concerete implementation — React context is only one of the possible options.

The same way you can inject implementations using props

Good design makes the components not depend directly upon volatile dependency, 
but rather depend on a stable interface (by using the Dependency Inversion Principle) that describes the dependency, and then allows a dependency injection mechanism (like React context) to supply the concrete dependency implementation.
