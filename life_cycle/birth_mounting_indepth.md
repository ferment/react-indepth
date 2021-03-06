# Birth/Mounting In-depth
 A React Component kicks off the life cycle during the initial application ex: `ReactDOM.render()`. With the initialization of the component instance, we start moving through the Birth phase of the life cycle. Before we dig deeper into the mechanics of the Birth phase, let's step back a bit and talk about what this phase focuses on.
 
 The most obvious focus of the birth phase is the initial configuration for our Component instance. This is where we pass in the `props` that will define the instance. But during this phase there are a lot more moving pieces that we can take advantage of. 
 
 In Birth we configure the default `state` and get access to the initial UI display. It also starts the mounting process for children of the Component. Once the children mount, we get first access to the Native UI layer[^1] (DOM, UIView, etc.). With Native UI access, we can start to query and modify how our content is actually displayed. This is also when we can begin the process of integrating 3rd Party UI libraries and components.
 
## Components vs. Elements
 When learning React, many developers have a common misconception. At first glance, one would assume that a mounted instance is the same as a component class. For example, if I create a new React component and then `render()` it to the DOM:
 
 ```javascript
 import React from 'react';
 import ReactDOM from 'react-dom';
 
 class MyComponent extends React.Component {
   render() {
     return <div>Hello World!</div>;
   }
 };
 
 ReactDOM.render(<MyComponent />, document.getElementById('mount-point')); 
 ```
 
 The initial assumption is that during `render()` an instance of the `MyComponent` class is created, using something like `new MyComponent()`. This instance is then passed to render. Although this sounds reasonable, the reality of the process is a little more involved.
 
 What is actually occurring is the JSX processor converts the `<MyComponent />` line to use `React.createElement` to generate the instance. This generated Element is what is passed to the `render()` method:
 
 ```javascript
 // generated code post-JSX processing
 ReactDOM.render(
   React.createElement(MyComponent, null), document.getElementById('mount-point')
 );
 ```
 
 A React Element is really just a description[^2] of what will eventually be used to generate  the Native UI. This is a core, pardon the pun, *element* of virtual DOM technology in React.
 
 
> The primary type in React is the ReactElement. It has four properties: type, props, key and ref. It has no methods and nothing on the prototype.
>
> -- https://facebook.github.io/react/docs/glossary.html#react-elements


The Element is a lightweight object representation of what will become the component instance. If we try to access the Element thinking it is the Class instance we will have some issues, such as availability of expected methods. 

So, how does this tie into the life cycle? These descriptor Elements are essential to the creation of the Native UI and are the catalyst to the life cycle.
 
## The First `render()`
 To most React developers, the `render()` method is the most familiar. We write our JSX and layout here. It's where we spend a lot of time and drives the layout of the application. When we talk about the first `render()` this is a special version of the `render()` method that mounts our entire application on the Native UI.

 In the browser, this is the `ReactDOM.render()` method. Here we pass in the root Element and tell React where to mount our content. With this call, React begins processing the passed Element(s) and generate instances of our React components. The Element is used to generate the type instance and then the `props` are passed to the Component instance.

 This is the point where we enter the Component life cycle. React uses the `instance` property on the Element and begins construction.

 
 ***Next Up:*** [Initialization & Construction](birth/initialization_and_construction.md)

---

[^1] The Native UI layer is the system that handles UI content rendering to screen. In a browser, this is the DOM. On device, this would be the UIView (or comparable). React handles the translation of Component content to the native layer format.

[^2] Dan Abramov chimed in with this terminology on a StackOverflow question. http://stackoverflow.com/a/31069757
