# React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Functional Components](#Functional-Components)
  1. [Class Components](#class-Components)
  1. [Context](#context)

## Basic Rules

  - Only include one React component per file.
  - Watch closely your imports, you might be importing a component that is not meant to be reused

## Folder structure

  We must differentiate between <b>Momentum UI Components</b> and <b>React Components</b>. Both use react to render DOM however: 
  - <b>Momentum Components</b> [Docs](http://10.41.60.122/master/framework/docs/index.html#/Framework/Composition) Use a Model View View Model architecture, and are rendered by the framework using the catalog config.
  - <b>React Components</b> [Docs](https://en.reactjs.org/docs/components-and-props.html): Are the traditional react components, You can leverage all the react features (hooks, context, etc.) to build your components. But have in mind they can't be rendered directly by the framework, if your component needs to be rendered by the `shell` or the framework it must be a <b>Momentum Component.</b>


  ### General Structure
  Applications will follow the general momentum-ui folder structure documented [here](http://10.41.60.122/master/framework/docs/index.html), 
  
  - <b>Momentum Components</b>: will be placed inside `src/catalog`
  - <b>React Components</b>: that are reusable across the whole application will be placed inside `src/partials`
  - <b>React Components</b>: That are only meant to be used inside a certain part of the app will be placed in the `partials` folder of the bounding component.

  ### Services

  - Services Are global entities, They can be used by all the components in the application.
  - Services are placed in `src/services`
  - Services must not have presentation code e.g. Show a success notification. This is a responsibility of the component that is using the service.
  - Services have this structure:
  ```
      ServiceName
      │   ServiceName.spec.ts
      |   DataTypes.ts
      │   ServiceName.ts    
  ```  

  ### Component Structure

  A React Component has the following file structure:
  ```
      ComponentName
      │   ComponentName.spec.tsx
      |   ComponentName.module.scss
      │   ComponentName.tsx    
      └───partials
      │   │   ...
  ```
  - A parent folder with the same name as the component,
  - The component file, must have the same name as the component, and export default the component class or function
  - SASS styles file, All the classes used by this component must be defined in this file, i.e. no import from other style files.
  - Unit Test file

  #### Partials Folder

  A component may be divided in to sub components (thats the beauty of React).

  When the sub component is generic enough to be used in the whole application it must be placed in the global partials folder `src/partials`
  
  On the contrary when the sub components are not suited to be reused outside of this component. (Maybe it doesn't make sense or they require access to a context). They must be placed on the component partials folder.
 
  <b>A component can only use components from `src/partials` and its own `./partials`.</b>


## Naming

  - **Extensions**: Use `.jsx` extension for React components. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // bad
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

  - **Component Methods Naming**: use "onGENERIC_EVENT_NAME" to name callbacks and "handleGENERIC_EVENT_NAME" to handle them.

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx

    // bad, My Component should not be aware that clicking a move shows the move details
    <MyComponent onShowMoveDetails={this.handleMoveClicked} />

    // good
    <MyComponent onMoveClick={this.handleMoveClicked} />
    ```


**[⬆ back to top](#table-of-contents)**

## Functional Components

## Class Components
- [2.1](#types) Use Arrow functions for component methods

  >Why? Thanks to the lexical scoping, "this" will be the component, event if is called by its children

    ```typescript
    // bad
        class Component {
          handleMoveClick(){
            this.showDetails();
          }
        }
    // good
        class Component {
          handleMoveClick = () => {
            this.showDetails();
          }
        }

    ```

- [2.1](#types) Don't use constructor to sync props with state

  > Why? constructor is only invoked once, so any state you set will not be updated when the props change

    ```typescript
    class Button extends React.Component {
      constructor(){
        this.state = {
          color: this.props.color
        };
      }
      
      render() {
        const { color } = this.state; // 🔴 `color` is stale!
        return (
          <button className={'Button-' + color}>
            {this.props.children}
          </button>
        );
      }
    }
    ```

- [2.1](#types) Don't use render methods

  > Why? Render methods are a nice abstraction, but we already have a better one, React Components. Creating a react component instead of a render method is better because of all the react features we love.

    ```typescript

    // bad
    class Component {
          renderThing = () => {
            return <div> thing </div>
          }
          render(){
            return <div>
              {this.renderThing()}
            </div>
          }
        }

    // good
        class Component {
          render(){
            return <div>
              <Thing/>
            </div>
          }
        }
    ```
## Child Components
- [2.1](#types) Use Key into autogenerate child components

  >Why? React dom use key to listen if some change has been done in the child

    ```typescript
    // bad
         render(){
            return <div>
             {
               [0,1,2].map((data)=> <Thing {...data}/>)
             }
            </div>
          }
    // good
         render(){
            return <div>
             {
               [0,1,2].map((data,index)=> <Thing key={index} {...data}/>)
             }
            </div>
          }


**[⬆ back to top](#table-of-contents)**
