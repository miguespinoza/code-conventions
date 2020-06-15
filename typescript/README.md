# Typescript Style Guide() {

*A mostly reasonable approach to Typescript*

## All rules from the typescript guide apply for typescript

## Table of Contents

  1. [Avoid use of any](#avoid-use-of-any)
  1. [Use nullable operator for optional parameters](#use-nullable-operator-for-optional-parameters)
  1. [Use Type Aliases to describe shape](#use-type-aliases-to-describe-shape)
  1. [Use Interfaces to describe behavior](#use-interfaces-to-describe-behavior)
  1. [Prefer Optional Chaining Operator](#prefer-optional-chaining-operator)
  1. [Prefer Nullish Coalescing](#prefer-nullish-coalescing)
  1. [Enums](#enums)

## Avoid use of any


  - [1.1](#avoid-any) Any should only be used for external libraries that do not have types definitions, 

    > Why? Typescript is a great tool, using any defeats the purpose of using it.

    ```typescript
    // bad
        type ComponentProps = {
            content: any;
        }
    // good
        type ComponentProps = {
            content: React.Element;
        }

    ```

## Use nullable operator for optional parameters
    > It is better for readability

    ```typescript
    // bad
        type ComponentProps = {
            id: string | undefined;
        }
    // good
        type ComponentProps = {
            id?: string;
        }

    ```


## Use Type Aliases to describe shape
- [2.1](#types) Types aliases are meant to describe the shape of things, use them to describe things like prop  types, return values, as long as the thing you are describing is not behavior based.

    ```typescript
    // bad
        interface IClient = {
            name: string;
            lastName: string;
            createdDate: Date;
        }
    // good
        type Client = {
            name: string;
            lastName: string;
            createdDate: Date;
        }

    ```

**[⬆ back to top](#table-of-contents)**

## Use Interfaces to describe behavior
- [2.1](#interfaces) Interfaces are used to describe the shape and behavior of a thing. They can be implemented, extended and so on.

    ```typescript
    // bad
        type Service = {
            endpoint: string;
            fetchThings: (id: string) => Thing[],
            fetchOtherThings: (id: string) => OtherThing[],
        }
    // good
        interface Service = {
            endpoint: string;
            fetchThings: (id: string) => Thing[],
            fetchOtherThings: (id: string) => OtherThing[],
        }

    ```

**[⬆ back to top](#table-of-contents)**

## Prefer Optional Chaining Operator
    ```typescript
        // bad
            const a = param?.deep?.object?.property
        // good
            const a = param && param.deep && param.deep.object && param.deep.object.property;
    ```
**[⬆ back to top](#table-of-contents)**

## Prefer Nullish Coalescing

    ```typescript
        // bad
            const a = param != null ? param : "default"
        // good
            const a = param ?? "default"
    ```
**[⬆ back to top](#table-of-contents)**

## Enums
- [3.1](#enums) Use enums to define a set of named constants.

    ```typescript
    // bad
        type DeviceStatus =  "ONLINE" | "OFFLINE",

    // good
        enum DeviceStatus = {
            ONLINE: "ONLINE",
            OFFLINE: "OFFLINE",
        }
    ```

- [3.2](#no-display-enum) Never use an enum directly for display purposes.

    > Why? Enums are implementation details, their name is subject to change during refactors

    ```typescript
    // bad
        <p> {DeviceStatus.AUTOMATIC} </p>
    // good
        <p> {enumToString(DeviceStatus.AUTOMATIC)} </p>
    ```

- [3.3](#no-enum-differences) Never use a numeric value enum

    > Why? numeric enums are very hard to debug and if the definition order changes things will break.

    ```typescript
    // bad
        type DeviceStatus = {
            ONLINE,
            OFFLINE,
        }
    // good
        type DeviceStatus = {
            ONLINE: "ONLINE",
            OFFLINE: "OFFLINE",
        }
    ```

- [3.4](#no-enum-differences) Never assign an enum value different than the enum name

    > Why? It is going to be very confusing

    ```typescript
    // bad
        type DeviceStatus = {
            ONLINE: "offline",
            OFFLINE: "online",
        }
    // good
        type DeviceStatus = {
            ONLINE: "ONLINE",
            OFFLINE: "OFFLINE",
        }
    ```

- [3.5](#no-enum-differences) Never index an enum by its numeric value

    > Why? If the enum definition order changes, the code will break

    ```typescript
    // bad
        type DeviceStatus = {
            ONLINE,
            OFFLINE,
        }
        const offlineValue = DeviceStatus[1]


    // good
        type DeviceStatus = {
            ONLINE,
            OFFLINE,
        }
        const offlineValue = DeviceStatus[DeviceStatus.OFFLINE]
    ```

- [3.5](#no-enum-differences) Enum name is PascalCase and enum values UPPERCASE

    > Why? If the enum definition order changes, the code will break

    ```typescript
    // bad
        type DeviceStatus = {
            online: "ONLINE",
            offline: "OFFLINE",
        }


    // good
        type DeviceStatus = {
            ONLINE: "ONLINE",
            OFFLINE: "OFFLINE",
        }
    ```
- [3.5](#no-typed-values) Use typed values in return functions and variables

    > Why? avoid to mutate the return value

    ```typescript
    // bad
       const someString = () => "";
       const someVariableString = "";

    // good
       const someString:string = () => "";
       const someVariableString:string = "";
    ```
**[⬆ back to top](#table-of-contents)**

# };
