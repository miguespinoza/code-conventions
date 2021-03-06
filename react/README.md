# React/JSX Style Guide

*A mostly reasonable approach to React and JSX*


This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions (i.e async/await or static class fields) may still be included or prohibited on a case-by-case basis. Currently, anything prior to stage 3 is not included nor recommended in this guide.

## Styling Components
[This guide](styles) talks about Styling Components in React applications


## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Typescript](#typescript)
  1. [Functional Components](#functional-components)
  1. [Class Components](#class-Components)
  1. [Context](#context)

## Basic Rules

  - Only include one React component per file.
  - Watch closely your imports, you might be importing a component that is not meant to be reused. See more in the Partial Folder section

## Folder structure

  We must differentiate between **Momentum UI Components** and **React Components**. Both use react to render DOM however: 
  - **Momentum Components** [Docs](http://10.41.60.122/master/framework/docs/index.html#/Framework/Composition) Use a Model View View Model architecture, and are rendered by the framework using the catalog config.
  - **React Components** [Docs](https://en.reactjs.org/docs/components-and-props.html): Are the traditional react components, You can leverage all the react features (hooks, context, etc.) to build your components. But have in mind they can't be rendered directly by the framework, if your component needs to be rendered by the `shell` or the framework it must be a **Momentum Component.**


  ### General Structure
  Applications will follow the general momentum-ui folder structure documented [here](http://10.41.60.122/master/framework/docs/index.html), 
  
  - **Momentum Components**: will be placed inside `src/catalog`
  - **React Components**: that are reusable across the whole application will be placed inside `src/partials`
  - **React Components**: That are only meant to be used inside a certain part of the app will be placed in the `partials` folder of the bounding component.
  - **Global Types** Type definitions that are global to the application belong to `src/types`, They can be used anywhere in the application.

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
      │   ComponentName.tsx    
      |   ComponentName.module.scss
      │   ComponentName.spec.tsx
      |   ComponentNameTypes.ts
      └───partials
      │   │   ...
  ```
  - A parent folder with the same name as the component,
  - **ComponentName.tsx**, Must have the same name as the component, and export default the component class or function
  - **ComponentName.module.scss** SASS styles file, All the classes used by this component must be defined in this file, i.e. no import from other style files.
  - **ComponentName.spec.tsx** Unit Test file
  - **ComponentNameTypes.ts** Exports the type definition meant to be used by this component or their partials components

  #### Partials Folder
  <a name="#partials-folder"></a>
  A component may be divided in to sub components (that's the beauty of React).

  When the sub component is generic enough to be used in the whole application it must be placed in the global partials folder `src/partials`
  
  On the contrary when the sub components are not suited to be reused outside of this component. (Maybe it doesn't make sense or they require access to a context). They must be placed on the component partials folder.
 
  **A component can only use components from `src/partials` and its own `./partials`.**


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




## Typescript
  This section describes the specific rules and types for react and typescript
- Useful React Prop Type Examples
```typescript
  type UsefulProps = {
    /** Accepts any react node */
    children: React.ReactNode, 
    /** function that doesn't take or return anything (VERY COMMON) */
    onClick: () => void;
    /** function with named prop (VERY COMMON) */
    onChange: (id: number) => void;
    /** alternative function type syntax that takes an event (VERY COMMON) */
    onClick(event: React.MouseEvent<HTMLButtonElement>): void;
    /** an optional prop (VERY COMMON!) */
    optional?: OptionalType;
  }
```
- Check full guide [here](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet/blob/master/README.md#basic-cheatsheet-table-of-contents) 

## Functional Components
- Declare functional components with `React.FC` or `React.FunctionComponent`
  > Why? React.FC is explicit about the return type, while using a normal function declaration is implicit.

  ```typescript
    type ComponentProps = {
      message: string;
    }

    // Bad
    const App = ({ message }: AppProps) => <div>{message}</div>;

    //Good
    const Component: React.FunctionComponent<ComponentProps> = ({ message }) => (
      <div>{message}</div>
    );

    //Best
    const Component: React.FC<ComponentProps> = ({ message }) => (
      <div>{message}</div>
    );
  ```

## Class Components
⚠ We encourage the use of [Functional Components](#functional-components) over Class components.

- Define Props and State with the generic  `React.Component<props, state>`. Generic names are composed by the component name and the Props or State word like this:  "COMPONENT_NAMEProps" and "COMPONENT_NAMEState"

  ```typescript
    type AppProps = {
      // using `interface` is also ok
      message: string;
    };
    type AppState = {
      count: number; // like this
    };
    class App extends React.Component<AppProps, AppState> {
      state: AppState = {
        // optional second annotation for better type inference
        count: 0,
      };
      render() {
        return (
          <div>
            {this.props.message} {this.state.count}
          </div>
        );
      }
    }
  ```

- Use Arrow functions for component methods

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

- Don't use constructor to sync props with state

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

- Don't use render methods

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
- Use Key into autogenerate child components

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
    ```

## Context
- Use the new Context API over the legacy deprecated one

  >Why? The Legacy API will break in future versions of react see [here:](https://es.reactjs.org/docs/legacy-context.html)

    ```typescript
    // bad
    class App extends React.Component<> {
      contextType: Context;
      constructor(props, context){

      }
      render() {
        return (
          <div>
            {this.props.message} {this.state.count}
          </div>
        );
      }
    }
    // good
    class App extends React.Component<> {
      render() {
        return (
          <Context.Consumer>
          {(value) => {
            <div>
              {this.props.message} {this.state.count}
            </div>
          }}
          </Context.Consumer>
        );
      }
    }
    ```

- **mobx**, if you need to use Mobx observables and Context, **Wrap context render result in an `<Observer>` component**

  >Why? Mobx observables only work within an observer, and the context render prop is not one.

    ```typescript
    // bad
    class App extends React.Component<> {
      @observable
      name =  "name"
      render() {
        return (
          <Context.Consumer>
          {(value) => {
            <div>
              {this.name}
            </div>
          }}
          </Context.Consumer>
        );
      }
    }
    // good
    class App extends React.Component<> {
      render() {
        return (
          <Context.Consumer>
          {(value) => {
            <Observer>
              {() => (
                <div>
                  {this.name}
                </div>
              )}
            </Observer>
          }}
          </Context.Consumer>
        );
      }
    }
    ```


**[⬆ back to top](#table-of-contents)**
