# .React .Style-Guide {

*A opinionated approach to Styling React Applications*

Since the dawn of CSS styling has been one of the most controversial and feared topics of web development. 
This guide proposes a biased but tested set of guidelines to style React applications using sass and modules.
This approach benefits from the component mind-set native to react but also from the robustness of CSS. 

This is also easier to bootstrap since CRA included Sass modules in v.2

## Table of Contents
### TypeScript specific
  1. [One sass component](#One-Sass-file-per-component)
  1. [Use nesting for elements](#Use-nesting-for-elements)
  1. [Avoid abstracting styles](#Avoid-abstracting-styles)
  1. [Define layouts in parent components](#Define-layouts-in-parent-components)
 

## One Sass file per component
<a name="one-sass-per-component"></a><a name="1.1"></a>
  - [1.1](#avoid-any) Create one Sass class per component and use the same name

    > Why? This encourages the Single-responsibility principle for Components

    ```html
        <MyComponent className={styles.MyComponent}/>
    ```
    ```scss
        // bad
        .container {
            ...
        }

        // good
        .MyComponent{

        }

    ```
## Use nesting for elements

<a name="one-sass-per-component"></a><a name="2.1"></a>
  - [2.1](#) Use a single Top class and nest elements under it

    > Why? This scopes all the styles and also reflects the component intentional structure

    ```scss
        // bad
        .MyComponent {
            ...
        }
        .button {
            ...
        }

        // good
        .MyComponent{
            .button{
                ...
            }
        }

    ```


<a name="one-sass-per-component"></a><a name="2.2"></a>
  - [2.2](#) Name elements using camelCase

      > Why? This makes it easy to distinguish components from elements without referencing the JSX code

    ```scss
    
        .MyComponent{
            // bad
            .Container{
                ...
            }
            // good 
            .container{
                ...
            }
        }

    ```

<a name="one-sass-per-component"></a><a name="2.3"></a>
  - [2.3](#) Take advantage of nesting to shorten classnames

    > Why? Sass modules scope the selectors so you can use meaningful short names

    ```scss
    
        .MyComponent{
            // bad
            .MyComponentContainer{
                ...
            }
            // good 
            .container{
                ...
            }
        }
    ```


## Avoid abstracting styles
<a name="one-sass-per-component"></a><a name="3.1"></a>
  - [3.1](#) Rely on React for composition, avoid sharing rules between components. For those cases where reusability is a **must** opt for sass files and imports ant not global rules. 

    > Why? Tracking the scope of reusable rules is troublesome. Isolation is best.

## Define layouts in parent components   
<a name="one-sass-per-component"></a><a name="4.1"></a>
  - [4.1](#) Make positioning, sizing and layout is a responsibility of parent Components and pass a className Prop to children.

    > Why? If elements position or size themselves they are not truly reusable

<a name="one-sass-per-component"></a><a name="4.2"></a>
  - [4.2](#) Never style the generated dom elements. 

    > Why? The generated dom can change and break your style 

    ```html
        // bad
        <MyComponent className={styles.MyComponent}>
            <Icon>
        <MyComponent>

    ```

    ```scss
        // bad
        .MyComponent{
            i{
                ...
            }
        }
    ```
    ```html
        // good
        <MyComponent className={styles.MyComponent}>
            <Icon className={styles.icon}>
        <MyComponent>

    ```

    ```scss
        // good
        .MyComponent{
            .icon{
                ...
            }
        }
    ```