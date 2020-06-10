# Typescript Style Guide() {

*A mostly reasonable approach to Typescript*

## All rules from the javascript guide apply for typescript

## Table of Contents

  1. [Avoid use of any](#avoid-any)
  1. [Use Types to describe shape](#types)
  1. [Use Interfaces to describe behavior](#objects)
  1. [Prefer Optional Chaining Operator](#prefer-optional-chaining)
  1. [Prefer Nullish Coalescing](#prefer-nullish)
  1. [Enums](#enums)
  1. [@ts-ignore](#ts-ignore)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Control Statements](#control-statements)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [Contributors](#contributors)
  1. [License](#license)

## Avoid use of any

<a name="references--"></a><a name=""></a>
  - [1.1](#avoid-any) Any should only be used for external libraries that do not have types definitions, 

    > Why? Typescript is a great tool, using any defeats the purpose of using it.

    ```javascript
    // bad
        type ComponentProps = {
            content: any;
        }
    // good
        type ComponentProps = {
            content: React.Element;
        }

    ```

  <a name="references--"></a><a name=""></a>
  - [2.1](#references--prefer-const) 

    > Why? 

    ```javascript
    // bad

    // good

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

**[⬆ back to top](#table-of-contents)**

# };
