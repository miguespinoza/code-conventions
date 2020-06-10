# Airbnb React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions (i.e async/await or static class fields) may still be included or prohibited on a case-by-case basis. Currently, anything prior to stage 3 is not included nor recommended in this guide.

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Functional Components](#Functional-Components)
  1. [Class Components](#class-Components)
  1. [Context](#context)

## Basic Rules

  - Only include one React component per file.
## Folder structure
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
