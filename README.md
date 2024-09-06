---
cover: >-
  https://images.unsplash.com/photo-1577083165633-14ebcdb0f658?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw1fHxhcnR8ZW58MHx8fHwxNzI1NjA0NzU5fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Coding Styleguides

## Introduction

This document contains procedures for creating and writing program code in INDI Technology, which aims to make the code base that is created highly scalable and easy to maintain. so that other programmers can easily understand the code that we create.

{% hint style="info" %}
it is necessary to follow the standards developed. if there is a documented way to achieve something, go for it. **Whenever we want to do something different from the standard, make sure you have a reason why you are not following the standard.**
{% endhint %}

Good code is code that can document the code itself, is easy to read, and is easily understood by others, this will minimize bugs and can find bugs without making testing first.

make your code look fun!

<figure><img src=".gitbook/assets/giphy.gif" alt=""><figcaption></figcaption></figure>

## About Typescript

TypeScript stands in an unusual relationship to JavaScript. TypeScript offers all of JavaScript’s features, and an additional layer on top of these: TypeScript’s type system. The main benefit of TypeScript is that it can highlight unexpected behavior in your code, lowering the chance of bugs.

## General Typescript Rules

This is the style guide for the TypeScript language that was based on the one that is provided by Google. It contains both rules and best practices. you can read more [here](https://google.github.io/styleguide/tsguide.html).

This Style Guide uses [RFC 2119](http://tools.ietf.org/html/rfc2119) terminology when using the phrases _must_, _must not_, _should_, _should not_, and _may_. All examples given are non-normative and serve only to illustrate the normative language of the style guide.

{% hint style="info" %}
Since "consistency is the key", TypeScript Style Guide strives to enforce the majority of the rules by using automated tooling such as ESLint, TypeScript, Prettier, etc. Still, certain design and architectural decisions must be followed which are described with conventions below.
{% endhint %}

### Identifiers

Identifiers must use only ASCII letters, digits, underscores (for constants and structured test method names), and the '$' sign. Thus each valid identifier name is matched by the regular expression `[$\w]+`.

| Style           | Category                                                           |
| --------------- | ------------------------------------------------------------------ |
| `CamelCase`     | class / interface / type / enum / decorator / type / parameters    |
| `CamelCase`     | variable / parameter / function / method / property / module alias |
| `CONSTANT_CASE` | global constant values, including enum values                      |
| `#ident`        | private identifiers are never used.                                |

* **Abbreviations**: Treat abbreviations like acronyms in names as whole words, i.e. use `loadHttpUrl`, not `loadHTTPURL`, unless required by a platform name (e.g. `XMLHttpRequest`).
* **Dollar sign**: Identifiers _should not_ generally use `$`, except when aligning with naming conventions for third party frameworks. [See below](https://ts.dev/style/#naming-style) for more on using `$` with `Observable` values.
* **Type parameters**: Type parameters, like in `Array<T>`, may use a single upper case character (`T`) or `UpperCamelCase`.
* **Test names**: Test method names in Closure `testSuite`s and similar xUnit-style test frameworks may be structured with `_` separators, e.g. `testX_whenY_doesZ()`.
*   **`_` prefix/suffix**: Identifiers must not use `_` as a prefix or suffix.

    This also means that `_` must not be used as an identifier by itself (e.g. to indicate a parameter is unused).

### Files

* All TypeScript files must have a ".ts" or ".tsx" extension.
* They should be all lower case and only include letters, numbers, and periods.
* It is OK (even recommended) to separate words with periods (e.g. `my.view.html`).
* **All files should end in a new line.** This is necessary for some Unix systems.

### Source Code Formatting <a href="#source-code-formatting" id="source-code-formatting"></a>

Source code formatting can be completely automated. Humans should not waste time arguing about comma placement in code reviews.

In very rare situations (e.g. long multiline container literals or formatting bugs that cause semantic issues), it can be necessary to disable formatter for a section. Use this only where absolutely needed.

### Indentation

* The unit of indentation is four spaces.
* **Never use tabs**, as this can lead to trouble when opening files in different IDEs/Text editors. Most text editors have a configuration option to change tabs to spaces.

### Line Length

* Lines must not be longer than 140 characters.
* When a statement runs over 140 characters on a line, it should be broken up, ideally after a comma or operator.

### Do not use `null`

Use `undefined`. Do not use `null` except where external libraries require it.

### **Aliases**

When creating a local-scope alias of an existing symbol, use the format of the existing identifier. The local alias must match the existing naming and format of the source. For variables use `const` for your local aliases, and for class fields use the `readonly` attribute.

```ts
const { Foo } = SomeType;
const CAPACITY = 5;

class Teapot {
  readonly BrewStateEnum = BrewStateEnum;
  readonly CAPACITY = CAPACITY;
}
```

### Quotes

* Use single-quotes `''` for all strings, and use double-quotes `""` for strings within strings.
* When you need to use an apostrophe inside a string it is recommended to use double-quotes.
* Use template literals only when using expression interplation `${}`

```typescript
// bad
let greeting = "Hello World!";

// good
let greeting = 'Hello World!';

// bad
let phrase = 'It\'s Friday!';

// good
let phrase = "It's Friday!";

// bad
let html = "<div class='bold'>Hello World</div>";

// bad
let html = '<div class=\'bold\'>Hello World</div>';

// good
let html = '<div class="bold">Hello World</div>';

// bad
let template = `string text string text`;

// good
let template = `string text ${expression} string text`;
```

### Commas

* Use trailing commas always. DO NOT USE leading commas.
* Always use an additional trailing comma

```typescript
// bad
const person = {
    firstName: 'John'
  , lastname: 'Smith'
  , email: 'john.smith@outlook.com'
};

// bad
const person = {
    firstName: 'John',
    lastname: 'Smith',
    email: 'john.smith@outlook.com'
};

// good
const person = {
    firstName: 'John',
    lastname: 'Smith',
    email: 'john.smith@outlook.com',
};
```

### Iterators

Don't use iterators whenever it's possible to use higher-order functions.

```typescript
// bad
const numbers = [1, 2, 3];
let sum = 0;

for (let num of numbers) {
    sum += num;
}

// good
const numbers = [1, 2, 3];
const sum = numbers.reduce((prev, current) => {
    return prev + current;
}, 0);

// bad
const number = [1, 2, 3];
const add1 = [];
for (let i = 0; i < numbers.length; i += 1) {
  add1.push(numbers[i] + 1);
}

// good
const number = [1, 2, 3];
const add1 = numbers.map((num) => {
    return num + 1;
});
```

### Object and Array Literals

* Use curly braces `{}` instead of `new Object()`.
* Use brackets `[]` instead of `new Array()`.

### Destructuring

* Use Object destructuring always.
* Use Array destructuring except when returning

```
// bad
function toName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;
    const email = user.email;

    if (isEmpty(lastName)) {
        if (isEmpty(firstName)) {
            return email;
        }

        return firstName;
    }

    return `${firstName} ${lastName}`;
}

// good
function toName({ firstName, lastName, email }) {
    if (isEmpty(lastName)) {
        if (isEmpty(firstName)) {
            return email;
        }

        return firstName;
    }

    return `${firstName} ${lastName}`;
}

const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

### Assignment Expressions

* Assignment expressions inside of the condition block of `if`, `while`, and `do while` statements should be avoided.

```typescript
// bad
while (node = node.next) {
    // ...
}

// good
while (typeof node === 'object') {
    node = node.next;

    // ...
}
```

### === and !== Operators

* It is better to use `===` and `!==` operators whenever possible.
* `==` and `!=` operators do type coercion, which can lead to headaches when debugging code.

### Promises and Async/Await

* Always use `async/await` wherever possible. Avoid using `Promise.then` and `Promise.catch`.

```typescript
// bad
function reddits(): Promise<reddit.IListing> {
    return fetch('https://www.reddit.com/r/all.json')
        .then((response) => {
            return response.json();
        })
        .then((result) => {
            return result.data;
        })
        .catch((error) => {
            console.log(error);

            return {
                kind: 'Listing',
                data: {
                    children: [],
                },
            };
        });
}

// good
async function reddits(): Promise<reddit.IListing> {
    try {
        const response = await fetch('https://www.reddit.com/r/all.json');

        return response.json();
    } catch(error) {
        console.log(error);

        return {
            kind: 'Listing',
            data: {
                children: [],
            },
        };
    }
}
```

### Eval

* **Never use eval**
* **Never use the Function constructor**
* **Never pass strings to `setTimeout` or `setInterval`**

### TSLint

{% hint style="warning" %}
Always use a Linter
{% endhint %}

Linting your code is very helpful for preventing minor issues that can escape the eye during development. We use TSLint (written by Palantir) for our linter.

TSLint: [https://github.com/palantir/tslint](https://github.com/palantir/tslint)

### Prettier

{% hint style="warning" %}
Always use a formatter
{% endhint %}

Formatting your code is very helpful for readability. When working in teams, it is nice to be able to look at code that is in the same format across the entire application.

Prettier: [https://github.com/prettier/prettier](https://github.com/prettier/prettier)

### Services[​](https://mkosir.github.io/typescript-style-guide/#services) <a href="#services" id="services"></a>

Documentation becomes outdated the moment it's written, and worse than no documentation is wrong documentation. The same can be said about types when describing modules your app interacts with (APIs, messaging systems, databases).

For external API services e.g. REST, GraphQL etc. it's crucial to generate types from their contracts, whether it's Swagger, schemas, or other sources (e.g. [openapi-ts](https://github.com/drwpow/openapi-typescript), [graphql-config](https://github.com/kamilkisiela/graphql-config)...). Avoid manually declaring and maintaining them, as it's easy to get them out of sync.

As exception manually declare types only when their is truly no documentation provided by external service.

## Types

When creating types we try to think of how they would best **describe our code**.\
Being expressive and keeping types as **narrow as possible** brings benefits to the codebase:

* Increased Type Safety - Catch errors at compile-time, since narrowed types provide more specific information about the shape and behavior of your data.
* Improved Code Clarity - Cognitive load is reduced by providing clearer boundaries and constraints on your data which makes your code easier to understand by other developers.
* Easier Refactoring - Refactor with confidence, since types are narrow, making changes to your code becomes less risky.
* Optimized Performance - In some cases, narrow types can help the TypeScript compiler generate more optimized JavaScript code.

{% hint style="info" %}
Just because you don't need to add types, doesn't mean you shouldn't. In some cases explicit type declaration can increase code readability and intent.
{% endhint %}

```typescript
// bad
let numbers = [];

// bad
let numbers: Array<number> = [];

// good
let numbers: number[] = [];
```

### Type Inference[​](https://mkosir.github.io/typescript-style-guide/#type-inference) <a href="#type-inference" id="type-inference"></a>

As rule of thumb, explicitly declare a type when it help narrows it.

```typescript
// ✖️ Avoid - Don't explicitly declare a type, it can be inferred.
const userRole: string = 'admin'; // Type 'string'
const employees = new Map<string, number>([['Gabriel', 32]]);
const [isActive, setIsActive] = useState<boolean>(false);

// ✅ Use type inference.
const USER_ROLE = 'admin'; // Type 'admin'
const employees = new Map([['Gabriel', 32]]); // Type 'Map<string, number>'
const [isActive, setIsActive] = useState(false); // Type 'boolean'


// ✖️ Avoid - Don't infer a (wide) type, it can be narrowed.
const employees = new Map(); // Type 'Map<any, any>'
employees.set('Lea', 'foo-anything');
type UserRole = 'admin' | 'guest';
const [userRole, setUserRole] = useState('admin'); // Type 'string'

// ✅ Use explicit type declaration to narrow the type.
const employees = new Map<string, number>(); // Type 'Map<string, number>'
employees.set('Gabriel', 32);
type UserRole = 'admin' | 'guest';
const [userRole, setUserRole] = useState<UserRole>('admin'); // Type 'UserRole'
```

### Data Immutability[​](https://mkosir.github.io/typescript-style-guide/#data-immutability) <a href="#data-immutability" id="data-immutability"></a>

Majority of the data should be immutable with use of `Readonly`, `ReadonlyArray`.

Using readonly type prevents accidental data mutations, which reduces the risk of introducing bugs related to unintended side effects.

When performing data processing always return new array, object etc. To keep cognitive load for future developers low, try to keep data objects small.\
As an exception mutations should be used sparingly in cases where truly necessary: complex objects, performance reasoning etc.

```
// ✖️ Avoid data mutations
const removeFirstUser = (users: Array<User>) => {
  if (users.length === 0) {
    return users;
  }
  return users.splice(1);
};

// ✅ Use readonly type to prevent accidental mutations
const removeFirstUser = (users: ReadonlyArray<User>) => {
  if (users.length === 0) {
    return users;
  }
  return users.slice(1);
  // Using arr.splice(1) errors - Function 'splice' does not exist on 'users'
};
```

### Return Types[​](https://mkosir.github.io/typescript-style-guide/#return-types) <a href="#return-types" id="return-types"></a>

Including return type annotations is highly encouraged, although not required ([eslint rule](https://typescript-eslint.io/rules/explicit-function-return-type/)).

Consider benefits when explicitly typing the return value of a function:

* Return values makes it clear and easy to understand to any calling code what type is returned.
* In the case where there is no return value, the calling code doesn't try to use the undefined value when it shouldn't.
* Surface potential type errors faster in the future if there are code changes that change the return type of the function.
* Easier to refactor, since it ensures that the return value is assigned to a variable of the correct type.
* Similar to writing tests before implementation (TDD), defining function arguments and return type, gives you the opportunity to discuss the feature functionality and its interface ahead of implementation.
* Although type inference is very convenient, adding return types can save TypeScript compiler a lot of work.

### Discriminated union[​](https://mkosir.github.io/typescript-style-guide/#discriminated-union) <a href="#discriminated-union" id="discriminated-union"></a>

If there is only one TypeScript feature to choose, embrace discriminated unions.

Discriminated unions are a powerful concept to model complex data structures and improve type safety, leading to clearer and less error-prone code.\
You may encounter discriminated unions under different names such as tagged unions or sum types in various programming languages as C, Haskell, Rust (in conjunction with pattern-matching).

Discriminated unions advantages:

* As mentioned in [Args as Discriminated Union](https://app.gitbook.com/o/XsiIH2Ag99nJgvWwXuiv/s/P38EefRXJ31akd9VFgob/), discriminated union eliminates optional args which decreases complexity on function API.
*   Exhaustiveness check - TypeScript can ensure that all possible variants of a type are implemented, eliminating the risk of undefined or unexpected behavior at runtime. ([eslint rule](https://typescript-eslint.io/rules/switch-exhaustiveness-check/))

    ```typescript
    type Circle = { kind: 'circle'; radius: number };
    type Square = { kind: 'square'; size: number };
    type Triangle = { kind: 'triangle'; base: number; height: number };

    // Create discriminated union 'Shape', with 'kind' property to discriminate the type of object.
    type Shape = Circle | Square | Triangle;

    // TypeScript warns us with errors in calculateArea function
    const calculateArea = (shape: Shape) => {
      // Error - Switch is not exhaustive. Cases not matched: "triangle"
      switch (shape.kind) {
        case 'circle':
          return Math.PI * shape.radius ** 2;
        case 'square':
          return shape.size * shape.width; // Error - Property 'width' does not exist on type 'square'
      }
    };
    ```
* Avoid code complexity introduced by [flag variables](https://mkosir.github.io/typescript-style-guide/#type-union--boolean-flags).
* Clear code intent, as it becomes easier to read and understand by explicitly indicating the possible cases for a given type.
* TypeScript can narrow down union types, ensuring code correctness at compile time.
* Discriminated unions make refactoring and maintenance easier by providing a centralized definition of related types. When adding or modifying types within the union, the compiler reports any inconsistencies throughout the codebase.
* IDEs can leverage discriminated unions to provide better autocompletion and type inference.

### Template Literal Types[​](https://mkosir.github.io/typescript-style-guide/#template-literal-types) <a href="#template-literal-types" id="template-literal-types"></a>

Embrace using template literal types, instead of just (wide) `string` type.\
Template literal types have many applicable use cases e.g. API endpoints, routing, internationalization, database queries, CSS typings ...

```typescript
// ✖️ Avoid
const userEndpoint = '/api/usersss'; // Type 'string' - Since typo 'usersss', route doesn't exist and results in runtime error

// ✅ Use
type ApiRoute = 'users' | 'posts' | 'comments';
type ApiEndpoint = `/api/${ApiRoute}`; // Type ApiEndpoint = "/api/users" | "/api/posts" | "/api/comments"
const userEndpoint: ApiEndpoint = '/api/users';

// ✖️ Avoid
const homeTitle = 'translation.homesss.title'; // Type 'string' - Since typo 'homesss', translation doesn't exist and results in runtime error

// ✅ Use
type LocaleKeyPages = 'home' | 'about' | 'contact';
type TranslationKey = `translation.${LocaleKeyPages}.${string}`; // Type TranslationKey = `translation.home.${string}` | `translation.about.${string}` | `translation.contact.${string}`
const homeTitle: TranslationKey = 'translation.home.title';

// ✖️ Avoid
const color = 'blue-450'; // Type 'string' - Since color 'blue-450' doesn't exist and results in runtime error

// ✅ Use
type BaseColor = 'blue' | 'red' | 'yellow' | 'gray';
type Variant = 50 | 100 | 200 | 300 | 400;
type Color = `${BaseColor}-${Variant}` | `#${string}`; // Type Color = "blue-50" | "blue-100" | "blue-200" ... | "red-50" | "red-100" ... | #${string}
const iconColor: Color = 'blue-400';
const customColor: Color = '#AD3128';
```

### Type any & unknown[​](https://mkosir.github.io/typescript-style-guide/#type-any--unknown) <a href="#type-any--unknown" id="type-any--unknown"></a>

`any` data type must not be used as it represents literally “any” value that TypeScript defaults to and skips type checking since it cannot infer the type. As such, `any` is dangerous, it can mask severe programming errors.

When dealing with ambiguous data type use `unknown`, which is the type-safe counterpart of `any`.\
`unknown` doesn't allow dereferencing all properties (anything can be assigned to `unknown`, but `unknown` isn’t assignable to anything).

```typescript
// ✖️ Avoid any
const foo: any = 'five';
const bar: number = foo; // no type error

// ✅ Use unknown
const foo: unknown = 5;
const bar: number = foo; // type error - Type 'unknown' is not assignable to type 'number'

// Narrow the type before dereferencing it using:
// Type guard
const isNumber = (num: unknown): num is number => {
  return typeof num === 'number';
};
if (!isNumber(foo)) {
  throw Error(`API provided a fault value for field 'foo':${foo}. Should be a number!`);
}
const bar: number = foo;

// Type assertion
const bar: number = foo as number;
```

### Type & Non-nullability Assertions[​](https://mkosir.github.io/typescript-style-guide/#type--non-nullability-assertions) <a href="#type--non-nullability-assertions" id="type--non-nullability-assertions"></a>

Type assertions `user as User` and non-nullability assertions `user!.name` are unsafe. Both only silence TypeScript compiler and increase the risk of crashing application at runtime.\
They can only be used as an exception (e.g. third party library types mismatch, dereferencing `unknown` etc.) with strong rational why being introduced into codebase.

```typescript
type User = { id: string; username: string; avatar: string | null };
// ✖️ Avoid type assertions
const user = { name: 'Nika' } as User;
// ✖️ Avoid non-nullability assertions
renderUserAvatar(user!.avatar); // Runtime error

const renderUserAvatar = (avatar: string) => {...}
```

### Type Error[​](https://mkosir.github.io/typescript-style-guide/#type-error) <a href="#type-error" id="type-error"></a>

If TypeScript error can't be mitigated, as last resort use `@ts-expect-error` to suppress it ([eslint rule](https://typescript-eslint.io/rules/prefer-ts-expect-error/)). If at any future point suppressed line becomes error-free, TypeScript compiler will indicate it.\
`@ts-ignore` is not allowed, where `@ts-expect-error` must be used with provided description ([eslint rule](https://typescript-eslint.io/rules/ban-ts-comment/#allow-with-description)).

```typescript
// ✖️ Avoid @ts-ignore
// @ts-ignore
const newUser = createUser('Gabriel');

// ✅ Use @ts-expect-error with description
// @ts-expect-error: The library type definition is wrong, createUser accepts string as an argument.
const newUser = createUser('Gabriel');
```

### Type Definition[​](https://mkosir.github.io/typescript-style-guide/#type-definition) <a href="#type-definition" id="type-definition"></a>

TypeScript offers two options for type definitions - `type` and `interface`. As they come with some functional differences in most cases they can be used interchangeably. We try to limit syntax difference and pick one for consistency.

All types must be defined with `type` alias [(eslint rule)](https://typescript-eslint.io/rules/consistent-type-definitions/#type).

{% hint style="info" %}
Consider using interfaces if developing package that can be further extended, team is more comfortable working with interfaces etc. In such case disable lint rule where needed e.g. using type unions (type Status = 'loading' | 'error') etc.
{% endhint %}

```typescript
// ✖️ Avoid interface definitions
interface UserRole = 'admin' | 'guest'; // invalid - interface can't define (commonly used) type unions

interface UserInfo {
  name: string;
  role: 'admin' | 'guest';
}

// ✅ Use type definition
type UserRole = 'admin' | 'guest';

type UserInfo = {
  name: string;
  role: UserRole;
};
```

In case of declaration merging (e.g. extending third-party library types) use `interface` and disable lint rule.

```typescript
// types.ts
declare namespace NodeJS {
  // eslint-disable-next-line @typescript-eslint/consistent-type-definitions
  export interface ProcessEnv {
    NODE_ENV: 'development' | 'production';
    PORT: string;
    CUSTOM_ENV_VAR: string;
  }
}

// server.ts
app.listen(process.env.PORT, () => {...}
```

#### Array Types[​](https://mkosir.github.io/typescript-style-guide/#array-types) <a href="#array-types" id="array-types"></a>

Array types must be defined with generic syntax ([eslint rule](https://typescript-eslint.io/rules/array-type/#generic)).

{% hint style="info" %}
As there is no functional difference between 'generic' and 'array' definition, feel free to set the one your team finds most readable to work with.
{% endhint %}

```typescript
// ✖️ Avoid
const x: string[] = ['foo', 'bar'];
const y: readonly string[] = ['foo', 'bar'];

// ✅ Use
const x: Array<string> = ['foo', 'bar'];
const y: ReadonlyArray<string> = ['foo', 'bar'];
```

## Naming things

The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

TypeScript expresses information in types, so names _should not_ be decorated with information that is included in the type. (See also [Testing Blog](https://testing.googleblog.com/2017/10/code-health-identifiernamingpostforworl.html) for more about what not to include.)

Strive to keep naming conventions consistent and readable, with important context provided, because another person will maintain the code you have written.

Some concrete examples of this rule:

* Do not use trailing or leading underscores for private properties or methods.
* All variable and function names should be formed with alphanumeric `A-Z, a-z, 0-9` and underscore `_` charcters.
* Variable, module, and function names should use lowerCamelCase.
* Do not use the `opt_` prefix for optional parameters.
  * For accessors, see [accessor rules](https://ts.dev/style/#getters-and-setters-accessors) below.
* Do not mark interfaces specially (`IMyInterface` or `MyFooInterface`) unless it's idiomatic in its environment. When introducing an interface for a class, give it a name that expresses why the interface exists in the first place (e.g. `class TodoItem` and `interface TodoItemStorage` if the interface expresses the format used for storage/serialization in JSON).
* Suffixing `Observable`s with `$` is a common external convention and can help resolve confusion regarding observable values vs concrete values. Judgement on whether this is a useful convention is left up to individual teams, but _should_ be consistent within projects.

### Use of var, let, and const

* Use `const` where appropriate, for values that never change
* Use `let` everywhere else
* Avoid using `var`

### Named Export[​](https://mkosir.github.io/typescript-style-guide/#named-export) <a href="#named-export" id="named-export"></a>

Named exports must be used to ensure that all imports follow a uniform pattern ([eslint rule](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-default-export.md)).\
This keeps variables, functions... names consistent across the entire codebase.\
Named exports have the benefit of erroring when import statements try to import something that hasn't been declared.

In case of exceptions e.g. Next.js pages, disable rule:

```typescript
// .eslintrc.js
overrides: [
  {
    files: ["src/pages/**/*"],
    rules: { "import/no-default-export": "off" },
  },
],
```

### Naming Conventions[​](https://mkosir.github.io/typescript-style-guide/#naming-conventions) <a href="#naming-conventions" id="naming-conventions"></a>

While it's often hard to find the best name, try optimize code for consistency and future reader by following conventions:

#### **Variables**[**​**](https://mkosir.github.io/typescript-style-guide/#variables-1)

**Locals**\
Camel case\
`products`, `productsFiltered`

**Booleans**\
Prefixed with `is`, `has` etc.\
`isDisabled`, `hasProduct`

[Eslint rule](https://typescript-eslint.io/rules/naming-convention) implements:

```typescript
// .eslintrc.js
'@typescript-eslint/naming-convention': [
  'error',
  {
    selector: 'variable',
    types: ['boolean'],
    format: ['PascalCase'],
    prefix: ['is', 'should', 'has', 'can', 'did', 'will'],
  },
],
```

**Constants**\
Capitalized\
`PRODUCT_ID`

**Object constants**\
Singular, capitalized with const assertion.

```typescript
const ORDER_STATUS = {
  pending: 'pending',
  fulfilled: true,
  error: 'Shipping Error',
} as const;
```

If object type exist use `satisfies` operator in conjunction with const assertion, to conform object matches its type.

```typescript
// OrderStatus type is already defined - e.g. generated from database schema model, API etc.
type OrderStatus = {
  pending: 'pending' | 'idle';
  fulfilled: boolean;
  error: string;
};

const ORDER_STATUS = {
  pending: 'pending',
  fulfilled: true,
  error: 'Shipping Error',
} as const satisfies OrderStatus;
```

**Functions**[**​**](https://mkosir.github.io/typescript-style-guide/#functions-1)\
Camel case\
`filterProductsByType`, `formatCurrency`

**Types**[**​**](https://mkosir.github.io/typescript-style-guide/#types-1)\
Pascal case\
`OrderStatus`, `ProductItem`

[Eslint rule](https://typescript-eslint.io/rules/naming-convention) implements:

```typescript
// .eslintrc.js
'@typescript-eslint/naming-convention': [
  'error',
  {
    selector: 'typeAlias',
    format: ['PascalCase'],
  },
],
```

**Generics**[**​**](https://mkosir.github.io/typescript-style-guide/#generics)\
A generic variable must start with the capital letter T followed by a descriptive name `TRequest`, `TFooBar`.

Creating more complex types often includes generics, which can make them hard to read and understand, that's why we try to put best effort when naming them.\
Naming generics using popular convention with one letter `T`, `K` etc. is not allowed, the more variables we introduce, the easier it is to mistake them.

```typescript
// ✖️ Avoid naming generics with one letter
const createPair = <T, K extends string>(first: T, second: K): [T, K] => {
  return [first, second];
};
const pair = createPair(1, 'a');

// ✅ Name starts with the capital letter T followed by a descriptive name
const createPair = <TFirst, TSecond extends string>(first: TFirst, second: TSecond): [TFirst, TSecond] => {
  return [first, second];
};
const pair = createPair(1, 'a');
```

[Eslint rule](https://typescript-eslint.io/rules/naming-convention) implements:

```typescript
// .eslintrc.js
'@typescript-eslint/naming-convention': [
  'error',
  {
    // Generic type parameter must start with letter T, followed by any uppercase letter.
    selector: 'typeParameter',
    format: ['PascalCase'],
    custom: { regex: '^T[A-Z]', match: true },
  },
],
```

**Abbreviations & Acronyms**[**​**](https://mkosir.github.io/typescript-style-guide/#abbreviations--acronyms)\
Treat acronyms as whole words, with capitalized first letter only.

```typescript
// ✖️ Avoid
const FAQList = ['qa-1', 'qa-2'];
const generateUserURL(params) => {...}

// ✅ Use
const FaqList = ['qa-1', 'qa-2'];
const generateUserUrl(params) => {...}
```

In favor of readability, strive to avoid abbreviations, unless they are widely accepted and necessary.

```typescript
// ✖️ Avoid
const GetWin(params) => {...}

// ✅ Use
const GetWindow(params) => {...}
```

**React Components**[**​**](https://mkosir.github.io/typescript-style-guide/#react-components)\
Pascal case\
`ProductItem`, `ProductsPage`

**Prop Types**[**​**](https://mkosir.github.io/typescript-style-guide/#prop-types)\
React component name following "Props" postfix\
`[ComponentName]Props` - `ProductItemProps`, `ProductsPageProps`

**Callback Props**[**​**](https://mkosir.github.io/typescript-style-guide/#callback-props)\
Event handler (callback) props are prefixed as `on*` - e.g. `onClick`.\
Event handler implementation functions are prefixed as `handle*` - e.g. `handleClick` ([eslint rule](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-handler-names.md)).

```typescript
// ✖️ Avoid inconsistent callback prop naming
<Button click={actionClick} />
<MyComponent userSelectedOccurred={triggerUser} />

// ✅ Use prop prefix 'on*' and handler prefix 'handle*'
<Button onClick={handleClick} />
<MyComponent onUserSelected={handleUserSelected} />
```

**React Hooks**[**​**](https://mkosir.github.io/typescript-style-guide/#react-hooks)\
Camel case, prefixed as 'use' ([eslint rule](https://github.com/facebook/react/tree/main/packages/eslint-plugin-react-hooks)), symmetrically convention as `[value, setValue] = useState()` ([eslint rule](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/hook-use-state.md#rule-details))

```typescript
// ✖️ Avoid inconsistent useState hook naming
const [userName, setUser] = useState();
const [color, updateColor] = useState();
const [isActive, setActive] = useState();

// ✅ Use
const [name, setName] = useState();
const [color, setColor] = useState();
const [isActive, setIsActive] = useState();
```

Custom hook must always return an object:

```typescript
// ✖️ Avoid
const [products, errors] = useGetProducts();
const [fontSizes] = useTheme();

// ✅ Use
const { products, errors } = useGetProducts();
const { fontSizes } = useTheme();
```

### **Use meaningful variable names.**

Distinguish names in such a way that the reader knows what the differences offer.

❌

```typescript
function isBetween(a1: number, a2: number, a3: number): boolean {
  return a2 <= a1 && a1 <= a3;
}
```

✅

```typescript
 function isBetween(value: number, left: number, right: number): boolean {
   return left <= value && value <= right;
 }
```

### **Use pronounceable variable names**

If you can't pronounce it, you can't discuss it without sounding weird.

❌

```typescript
class Subs {
  public ccId: number;
  public billingAddrId: number;
  public shippingAddrId: number;
}
```

✅

```typescript
class Subscription {
  public creditCardId: number;
  public billingAddressId: number;
  public shippingAddressId: number;
}
```

### **Avoid mental mapping**

Explicit is better than implicit.\
_Clarity is king._

❌

```typescript
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

✅

```typescript
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

### **Don't add unneeded context**

If your class/type/object name tells you something, don't repeat that in your variable name.

❌

```typescript
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

✅

```typescript
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

### Interfaces

* Interfaces should use UpperCamelCase.
* Interfaces should be prefaced with the capital letter I.
* Only `public` members should be in an interface, leave out `protected` and `private` members.

```typescript
interface IPerson {
    firstName: string;
    lastName: string;
    toString(): string;
}
```

### Constants

All constants you be defined with the `const` keyword and `UPPERCASE`.

```typescript
const CONTANTS_EXAMPLE = 'value';
```

### Classes

* Classes/Constructors should use UpperCamelCase (PascalCase).
* `Private` and `private static` members in classes should be denoted with the `private` keyword.
* `Protected` members in classes do not use the `private` keyword.
* Default to using `protected` for all instance members
* Use `private` instance members sparingly
* Use `public` instance members only when they are used by other parts of the application.

```typescript
class Person {
    protected fullName: string;

    constructor(public firstName: string, public lastName: string) {
        this.fullName = firstName + ' ' + lastName;
    }

    public toString() {
        return this.fullName;
    }

    protected walkFor(millis: number) {
        console.log(this.fullName + ' is now walking.');

        // Wait for millis milliseconds to stop walking
        setTimeout(() => {
            console.log(this.fullName + ' has stopped walking.');
        }, millis);
    }
}
```

## Comments

In general <mark style="color:yellow;">**try to avoid comments**</mark>, by writing expressive code and name things what they are.

Use comments when you need to add context or explain choices that cannot be expressed through code (e.g. config files).

Comments should always be complete sentences. As rule of thumb try to explain `why` in comments, not `how` and `what`.

```typescript
// ✖️ Avoid
// convert to minutes
const m = s * 60;
// avg users per minute
const myAvg = u / m;

// ✅ Use - Expressive code and name things what they are
const SECONDS_IN_MINUTE = 60;
const minutes = seconds * SECONDS_IN_MINUTE;
const averageUsersPerMinute = noOfUsers / minutes;

// ✅ Use - Add context to explain why in comments
// TODO: Filtering should be moved to the backend once API changes are released.
// Issue/PR - https://github.com/foo/repo/pulls/55124
const filteredUsers = frontendFiltering(selectedUsers);
// Use Fourier transformation to minimize information loss - https://github.com/dntj/jsfft#usage
const frequencies = signal.FFT();
```

JSDocs can be interpreted by IDEs for better intellisense. Below is an example of a JSDoc comment block for a function.

```typescript
/**
 * Takes in a name and returns a greeting string.
 *
 * @param name The name of the greeted person.
 */
function getGreeting(name: string): string {
    return 'Hello ' + name + '!';
}
```

### Class

* All classes must have block comments `/**...*/` for all public variables and functions.
* All public functions should use [JSDoc](http://usejsdoc.org/) style comments.
* Functions need to have a comment explaining what the function does, and all of the input parameters need to be annotated with `@param`.
* The class should include a block comment containing the description of the class
* The constructor should contain a JSDoc comment annotating any input parameters.

```typescript
/**
 * Contains properties of a Person.
 */
class Person {
    /**
     * Returns a new Person with the specified name.
     *
     * @param name The name of the new Person.
     */
    public static GetPerson(name: string): Person {
        return new Person(name);
    }

    /**
     * @param name The name of the new Person.
     */
    constructor(public name: string) { }

    /**
     * Instructs this Person to walk for a certain amount
     * of time.
     *
     * @param millis The number of milliseconds the Person
     * should walk.
     */
    public walkFor(millis: number): void {
        console.log(this.name + ' is now walking.');

        setTimeout(() => {
            console.log(this.name + ' has stopped walking.');
        }, millis);
    }
}
```

### Inline

* Inline comments are comments inside of complex statements (loops, functions, etc).
* Use `//` for all inline comments.
* Keep comments clear and concise.
* Place inline comments on a newline above the line they are annotating
* Put an empty line before the comment.

```typescript
// bad
let lines: Array<string>; // Holds all the lines in the file.

// good
// Holds all the lines in the file.
let lines: Array<string>;

// bad
function walkFor(name: string, millis: number): void {
    console.log(name + ' is now walking.');
    // Wait for millis milliseconds to stop walking
    setTimeout(() => {
        console.log(name + ' has stopped walking.');
    }, millis);
}

// good
function walkFor(name: string, millis: number): void {
    console.log(name + ' is now walking.');

    // Wait for millis milliseconds to stop walking
    setTimeout(() => {
        console.log(name + ' has stopped walking.');
    }, millis);
}
```

## Functions

Function conventions should be followed as much as possible (some of the conventions derives from functional programming basic concepts):

* should have a single responsibility.
* should be stateless where the same input arguments return the same value every single time.
* should accept at least one argument and return data.
* should not have side effects, but be pure. It's implementation should not modify or access variable value outside its local environment (global state, fetching, etc.).
* All functions must be declared before they are used.
* Any closure functions should be defined right after the let declarations.

```typescript
// bad
function createGreeting(name: string): string {
    let message = 'Hello ';

    return greet;

    function greet() {
        return message + name + '!';
    }
}

// good
function createGreeting(name: string): string {
    let message = 'Hello ';

    function greet() {
        return message + name + '!';
    }

    return greet;
}
```

* There should be no space between the name of the function and the left parenthesis `(` of its parameter list.
* There should be one space between the right parenthesis `)` and the left curly `{` brace that begins the statement body.

```typescript
// bad
function foo (){
    // ...
}

// good
function foo() {
    // ...
}
```

* The body of the function should be indented 4 spaces.
* The right curly brace `}` should be on a new line.
* The right curly brace `}` should be aligned with the line containing the left curly brace `{` that begins the function statement.

```typescript
// bad
function foo(): string {
  return 'foo';}

// good
function foo(): string {
    return 'foo';
}
```

* For each function parameter
  * There should be no space between the parameter and the colon `:` indicating the type declaration.
  * There should be a space between the colon `:` and the type declaration.

```typescript
// bad
function greet(name:string) {
  // ...
}

// good
function greet(name: string) {
  // ...
}
```

### Single Object Arg <a href="#single-object-arg" id="single-object-arg"></a>

To keep function readable and easily extensible for the future (adding/removing args), strive to have single object as the function arg, instead of multiple args.\
As exception this does not apply when having only one primitive single arg (e.g. simple functions isNumber(value), implementing currying etc.).

```typescript
// ✖️ Avoid having multiple arguments
transformUserInput('client', false, 60, 120, null, true, 2000);

// ✅ Use options object as argument
transformUserInput({
  method: 'client',
  isValidated: false,
  minLines: 60,
  maxLines: 120,
  defaultInput: null,
  shouldLog: true,
  timeout: 2000,
});
```

### Required & Optional Args[​](https://mkosir.github.io/typescript-style-guide/#required--optional-args) <a href="#required--optional-args" id="required--optional-args"></a>

**Strive to have majority of args required and use optional sparingly.**\
If function becomes to complex it probably should be broken into smaller pieces.\
An exaggerated example where implementing 10 functions with 5 required args each, is better then implementing one "can do it all" function that accepts 50 optional args.

### Args as Discriminated Union[​](https://mkosir.github.io/typescript-style-guide/#args-as-discriminated-union) <a href="#args-as-discriminated-union" id="args-as-discriminated-union"></a>

When applicable use **discriminated union type** to eliminate optional args, which will decrease complexity on function API and only necessary/required args will be passed depending on its use case.

```
// ❌ Avoid optional args as they increase complexity of function API
type StatusParams = {
  data?: Products;
  title?: string;
  time?: number;
  error?: string;
};

// ✅ Strive to have majority of args required, if that's not possible,
// use discriminated union for clear intent on function usage
type StatusParamsSuccess = {
  status: 'success';
  data: Products;
  title: string;
};

type StatusParamsLoading = {
  status: 'loading';
  time: number;
};

type StatusParamsError = {
  status: 'error';
  error: string;
};

type StatusParams = StatusSuccess | StatusLoading | StatusError;

export const parseStatus = (params: StatusParams) => {...
```

### Anonymous Functions

* All anonymous functions should be defined as fat-arrow/lambda `() => { }` functions unless it is absolutely necessary to preserve the context in the function body.
* All fat-arrow/lambda functions should have parenthesis `()` around the function parameters.

```typescript
// bad
clickAlert() {
    let element = document.querySelector('div');

    this.foo = 'foo';

    element.addEventListener('click', function(ev: Event) {
        // this.foo does not exist!
        alert(this.foo);
    });
}

// good
clickAlert() {
    let element = document.querySelector('div');

    this.foo = 'foo';

    element.addEventListener('click', (ev: Event) => {
        // TypeScript allows this.foo to exist!
        alert(this.foo);
    });
}
```

* Always surround the function block with braces `{}`

```typescript
// bad
element.addEventListener('submit', ev => ev.preventDefault());

// bad
element.addEventListener('submit', (ev: Event) => ev.preventDefault());

// good
element.addEventListener('submit', (ev: Event) => {
    ev.preventDefault();
});
```

* There should be a space between the right parenthesis `)` and the `=>`
* There should be a space between the `=>` and the left curly brace `{` that begins the statement body.

```typescript
// bad
element.addEventListener('click', (ev: Event)=>{alert('foo');});

// good
element.addEventListener('click', (ev: Event) => {
    alert('foo');
});
```

* The statement body should be indented 4 spaces.
* The right curly brace `}` should be on a new line.
* The right curly brace `}` should be aligned with the line containing the left curly brace `{` that begins the function statement.

## Variables

#### Const Assertion[​](https://mkosir.github.io/typescript-style-guide/#const-assertion) <a href="#const-assertion" id="const-assertion"></a>

Strive to use const assertion `as const`:

* type is narrowed
* object gets `readonly` properties
*   array becomes `readonly` tuple

    ```
    // ❌ Avoid declaring constants without const assertion
    const FOO_LOCATION = { x: 50, y: 130 }; // Type { x: number; y: number; }
    FOO_LOCATION.x = 10;

    const BAR_LOCATION = [50, 130]; // Type number[]
    BAR_LOCATION.push(10);

    const RATE_LIMIT = 25;
    const RATE_LIMIT_MESSAGE = `Max number of requests/min is ${RATE_LIMIT}.`; // Type string



    // ✅ Use const assertion
    const FOO_LOCATION = { x: 50, y: 130 } as const; // Type '{ readonly x: 50; readonly y: 130; }'
    FOO_LOCATION.x = 10; // Error

    const BAR_LOCATION = [50, 130] as const; // Type 'readonly [10, 20]'
    BAR_LOCATION.push(10); // Error

    const RATE_LIMIT = 25;
    const RATE_LIMIT_MESSAGE = `Max number of requests/min is ${RATE_LIMIT}.` as const; // Type 'Rate limit exceeded! Max number of requests/min is 25.'
    ```

#### Enums & Const Assertion[​](https://mkosir.github.io/typescript-style-guide/#enums--const-assertion) <a href="#enums--const-assertion" id="enums--const-assertion"></a>

Const assertion must be used over enum.

While enums can still cover use cases as const assertion would, we tend to avoid it. Some of the reasonings as mentioned in TypeScript documentation - [Const enum pitfalls](https://www.typescriptlang.org/docs/handbook/enums.html#const-enum-pitfalls), [Objects vs Enums](https://www.typescriptlang.org/docs/handbook/enums.html#objects-vs-enums), [Reverse mappings](https://www.typescriptlang.org/docs/handbook/enums.html#reverse-mappings)...

```
// .eslintrc.js
'no-restricted-syntax': [
  'error',
  {
    selector: 'TSEnumDeclaration',
    message: 'Use const assertion or a string union type instead.',
  },
],
```

```
// ❌ Avoid using enums
enum UserRole {
  GUEST,
  MODERATOR,
  ADMINISTRATOR,
}

enum Color {
  PRIMARY = '#B33930',
  SECONDARY = '#113A5C',
  BRAND = '#9C0E7D',
}

// ✅ Use const assertion
const USER_ROLES = ['guest', 'moderator', 'administrator'] as const;
type UserRole = (typeof USER_ROLES)[number]; // Type "guest" | "moderator" | "administrator"

// Use satisfies if UserRole type is already defined - e.g. database schema model
type UserRoleDB = ReadonlyArray<'guest' | 'moderator' | 'administrator'>;
const AVAILABLE_ROLES = ['guest', 'moderator'] as const satisfies UserRoleDB;

const COLOR = {
  primary: '#B33930',
  secondary: '#113A5C',
  brand: '#9C0E7D',
} as const;
type Color = typeof COLOR;
type ColorKey = keyof Color; // Type "PRIMARY" | "SECONDARY" | "BRAND"
type ColorValue = Color[ColorKey]; // Type "#B33930" | "#113A5C" | "#9C0E7D"
```

#### Type Union & Boolean Flags[​](https://mkosir.github.io/typescript-style-guide/#type-union--boolean-flags) <a href="#type-union--boolean-flags" id="type-union--boolean-flags"></a>

Strive to use type union variable instead multiple boolean flag variables.

Flags tend to compound over time and become hard to maintain, since they hide the actual app state.

```typescript
// ✖️ Avoid introducing multiple boolean flag variables
const isPending, isProcessing, isConfirmed, isExpired;

// ✅ Use type union variable
type UserStatus = 'pending' | 'processing' | 'confirmed' | 'expired';
const userStatus: UserStatus;
```

When maintaining code that makes the number of possible states grows quickly because use of flags, there are almost always unhandled states, utilize [discriminated union](https://app.gitbook.com/o/XsiIH2Ag99nJgvWwXuiv/s/P38EefRXJ31akd9VFgob/).

### Null & Undefined[​](https://mkosir.github.io/typescript-style-guide/#null--undefined) <a href="#null--undefined" id="null--undefined"></a>

In TypeScript types `null` and `undefined` many times can be used interchangeably.\
Strive to:

* Use `null` to explicitly state it has no value - assignment, return function type etc.
* Use `undefined` assignment when the value doesn't exist. E.g. exclude fields in form, request payload, database query ([Prisma differentiation](https://www.prisma.io/docs/concepts/components/prisma-client/null-and-undefined))

## Statements

* Each line should contain at most one statement.
* A semicolon should be placed at the end of every simple statement.

```typescript
// bad
let greeting = 'Hello World'

alert(greeting)

// good
let greeting = 'Hello World';

alert(greeting);
```

### Compound

Compound statements are statements containing lists of statements enclosed in curly braces `{}`.

* The enclosed statements should start on a newline.
* The enclosed statements should be indented 4 spaces.

```typescript
// bad
if (condition === true) { alert('Passed!'); }

// good
if (condition === true) {
    alert('Passed!');
}
```

* The left curly brace `{` should be at the end of the line that begins the compound statement.
* The right curly brace `}` should begin a line and be indented to align with the line containing the left curly brace `{`.

```typescript
// bad
if (condition === true)
{
    alert('Passed!');
}

// good
if (condition === true) {
    alert('Passed!');
}
```

* **Braces `{}` must be used around all compound statements** even if they are only single-line statements.

```typescript
// bad
if (condition === true) alert('Passed!');

// bad
if (condition === true)
    alert('Passed!');

// good
if (condition === true) {
    alert('Passed!');
}
```

If you do not add braces `{}` around compound statements, it makes it very easy to accidentally introduce bugs.

```typescript
if (condition === true)
    alert('Passed!');
    return condition;
```

It appears the intention of the above code is to return if `condition === true`, but without braces `{}` the return statement will be executed regardless of the condition.

* Compount statements do not need to end in a semicolon `;` with the exception of a `do { } while();` statement.

### Return

* If a `return` statement has a value you should not use parenthesis `()` around the value.
* The return value expression must start on the same line as the `return` keyword.

```typescript
// bad
return
    'Hello World!';

// bad
return ('Hello World!');

// good
return 'Hello World!';
```

* It is recommended to take a return-first approach whenever possible.

```typescript
// bad
function getHighestNumber(a: number, b: number): number {
    let out = b;

    if(a >= b) {
        out = a;
    }

    return out;
}

// good
function getHighestNumber(a: number, b: number): number {
    if(a >= b) {
        return a;
    }

    return b;
}
```

* Always **explicitly define a return type**. This can help TypeScript validate that you are always returning something that matches the correct type.

```typescript
// bad
function getPerson(name: string) {
    return new Person(name);
}

// good
function getPerson(name: string): Person {
    return new Person(name);
}
```

### If

* Alway be explicit in your `if` statement conditions.

```typescript
// bad
function isString(str: any) {
    return !!str;
}

// good
function isString(str: any): str is string {
    return typeof str === 'string';
}
```

Sometimes simply checking falsy/truthy values is fine, but the general approach is to be explicit with what you are looking for. This can prevent a lot of unncessary bugs.

If statements should take the following form:

```typescript
if (/* condition */) {
    // ...
}

if (/* condition */) {
    // ...
} else {
    // ...
}

if (/* condition */) {
    // ...
} else if (/* condition */) {
    // ...
} else {
    // ...
}
```

### For

For statements should have the following form:

```typescript
for(/* initialization */; /* condition */; /* update */) {
    // ...
}

let keys = Object.keys(/* object */);
let length = keys.length;

for(let i = 0; i < length; i += 1) {
    // ...
}
```

Object.prototype.keys is supported in `ie >= 9`.

* Use Object.prototype.keys in lieu of a `for...in` statement.

With TypeScript you can use `for...of` statements:

```typescript
let arr = [2, 3, 4];

for (const value of arr) {
    // ...
}
```

### While

While statements should have the following form:

```typescript
while (/* condition */) {
    // ...
}
```

### Do While

* Do while statements should be avoided unless absolutely necessary to maintain consistency.
* Do while statements must end with a semicolon `;`

Do while statements should have to following form:

```typescript
do {
    // ...
} while (/* condition */);
```

### Switch

Switch statements should have the following form:

```typescript
switch (/* expression */) {
    case /* expression */:
        // ...
        /* termination */
    default:
        // ...
}
```

* Each switch group except default should end with `break`, `return`, or `throw`.

### Try

* Try statements should be avoided whenever possible. They are not a good way of providing flow control.
* Try statements should be used when using `async/await` syntax.

Try statements should have the following form:

```typescript
try {
    // ...
} catch (error: Error) {
    // ...
}

try {
    // ...
} catch (error: Error) {
    // ...
} finally {
    // ...
}
```

### Continue

* It is recommended to take a continue-first approach in all loops.

### Throw

* Avoid the use of the throw statement unless absolutely necessary.
* Flow control through try/catch exception handling is not recommended.

## Typings

### External

* Typings are sometimes packaged with node modules, in this case you don't need to do anything
* Use `@types` for all external library declarations not included in `node_modules`
* Actively add/update/contribute typings when they are missing

### Internal

* Create declaration files `.d.ts` for your interfaces instead of putting them in your `.ts` files
* Let the TypeScript compiler infer as much as possible
* Avoid defining types when it is unnecessary

```typescript
// bad
let a: number = 2;

// good
let a = 2;
```

* Always define the return type of functions, this helps to make sure that functions always return the correct type

```typescript
// bad
function sum(a: number, b: number) {
    return a + b;
}

// good
function sum(a: number, b: number): number {
    return a + b;
}
```

## Source Organization[​](https://mkosir.github.io/typescript-style-guide/#source-organization) <a href="#source-organization" id="source-organization"></a>

### Code Collocation[​](https://mkosir.github.io/typescript-style-guide/#code-collocation) <a href="#code-collocation" id="code-collocation"></a>

* Every application or package in monorepo has project files/folders organized and grouped **by feature**.
* **Collocate code as close as possible to where it's relevant.**
* Deep folder nesting should not represent an issue.

### Imports[​](https://mkosir.github.io/typescript-style-guide/#imports) <a href="#imports" id="imports"></a>

Import paths can be relative, starting with `./` or `../`, or they can be absolute `@common/utils`.

To make import statements more readable and easier to understand:

* **Relative** imports `./sortItems` must be used when importing files within the same feature, that are 'close' to each other, which also allows moving feature around the codebase without introducing changes in these imports.
* **Absolute** imports `@common/utils` must be used in all other cases.
* **All** imports must be auto sorted by tooling e.g. [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports), [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)...

```typescript
// ✖️ Avoid
import { bar, foo } from '../../../../../../distant-folder';

// ✅ Use
import { locationApi } from '@api/locationApi';

import { foo } from '../../foo';
import { bar } from '../bar';
import { baz } from './baz';
```

## Appendix - React[​](https://mkosir.github.io/typescript-style-guide/#appendix---react) <a href="#appendix---react" id="appendix---react"></a>

Since React components and hooks are also functions, respective [function conventions](https://mkosir.github.io/typescript-style-guide/#functions) applies.

### Required & Optional Props[​](https://mkosir.github.io/typescript-style-guide/#required--optional-props) <a href="#required--optional-props" id="required--optional-props"></a>

**Strive to have majority of props required and use optional sparingly.**

Especially when creating new component for first/single use case majority of props should be required. When component starts covering more use cases, introduce optional props.\
There are potential exceptions, where component API needs to implement optional props from the start (e.g. shared components covering multiple use cases, UI design system components - button `isDisabled` etc.)

If component/hook becomes to complex it probably should be broken into smaller pieces.\
An exaggerated example where implementing 10 React components with 5 required props each, is better then implementing one "can do it all" component that accepts 50 optional props.

#### Props as Discriminated Type[​](https://mkosir.github.io/typescript-style-guide/#props-as-discriminated-type) <a href="#props-as-discriminated-type" id="props-as-discriminated-type"></a>

When applicable use **discriminated type** to eliminate optional props, which will decrease complexity on component API and only necessary/required props will be passed depending on its use case.

```typescript
// ✖️ Avoid optional props as they increase complexity of component API
type StatusProps = {
  data?: Products;
  title?: string;
  time?: number;
  error?: string;
};

// ✅ Strive to have majority of props required, if that's not possible,
// use discriminated union for clear intent on component usage
type StatusSuccess = {
  status: 'success';
  data: Products;
  title: string;
};

type StatusLoading = {
  status: 'loading';
  time: number;
};

type StatusError = {
  status: 'error';
  error: string;
};

type StatusProps = StatusSuccess | StatusLoading | StatusError;

export const Status = (status: StatusProps) => {...
```

### Props To State[​](https://mkosir.github.io/typescript-style-guide/#props-to-state) <a href="#props-to-state" id="props-to-state"></a>

In general avoid using props to state, since component will not update on prop changes. It can lead to bugs that are hard to track, with unintended side effects and difficulty testing.\
When there is truly a use case for using prop in initial state, prop must be prefixed with `initial` (e.g. `initialProduct`, `initialSort` etc.)

```typescript
// ✖️ Avoid using props to state
type FooProps = {
  productName: string;
  userId: string;
};

export const Foo = ({ productName, userId }: FooProps) => {
  const [productName, setProductName] = useState(productName);
  ...

// ✅ Use prop prefix `initial`, when there is a rational use case for it
type FooProps = {
  initialProductName: string;
  userId: string;
};

export const Foo = ({ initialProductName, userId }: FooProps) => {
  const [productName, setProductName] = useState(initialProductName);
  ...
```

### Props Type[​](https://mkosir.github.io/typescript-style-guide/#props-type) <a href="#props-type" id="props-type"></a>

```typescript
// ✖️ Avoid using React.FC type
type FooProps = {
  name: string;
  score: number;
};

export const Foo: React.FC<FooProps> = ({ name, score }) => {

// ✅ Use props argument with type
type FooProps = {
  name: string;
  score: number;
};

export const Foo = ({ name, score }: FooProps) => {...
```

### Store & Pass Data[​](https://mkosir.github.io/typescript-style-guide/#store--pass-data) <a href="#store--pass-data" id="store--pass-data"></a>

* Pass only the necessary props to child components rather than passing the entire object.
* Utilize storing state in the URL, especially for filtering, sorting etc.
* Don't sync URL state with local state.
* Consider passing data simply through props, using the URL, or composing children. Use global state (Zustand, Context) as a last resort.
*   Use React compound components when components should belong and work together: `menu`, `accordion`,`navigation`, `tabs`, `list`,...\
    Always export compound components as:

    ```typescript
    // PriceList.tsx
    const PriceListRoot = ({ children }) => <ul>{children}</ul>;
    const PriceListItem = ({ title, amount }) => <li>Name: {name} - Amount: {amount}<li/>;

    // ✖️
    export const PriceList = {
      Container: PriceListRoot,
      Item: PriceListItem,
    };

    // ✖️
    PriceList.Item = Item;
    export default PriceList;

    // ✅
    export const PriceList = PriceListRoot as typeof PriceListRoot & {
      Item: typeof PriceListItem;
    };
    PriceList.Item = PriceListItem;

    // App.tsx
    import { PriceList } from "./PriceList";

    <PriceList>
      <PriceList.Item title="Item 1" amount={8} />
      <PriceList.Item title="Item 2" amount={12} />
    </PriceList>;
    ```
* UI components should show derived state and send events, nothing more (no business logic).
* As in many programming languages functions args can be passed to the next function and on to the next etc.\
  React components are no different, where prop drilling should not become an issue.\
  If with app scaling prop drilling truly becomes an issue, try to refactor render method, local states in parent components, using composition etc.
* Data fetching is only allowed in container components.
* Use of server-state library is encouraged ([react-query](https://github.com/tanstack/query), [apollo client](https://github.com/apollographql/apollo-client)...).
* Use of client-state library for global state is discouraged.\
  Reconsider if something should be truly global across application, e.g. `themeMode`, `Permissions` or even that can be put in server-state (e.g. user settings - `/me` endpoint). If still global state is truly needed use [Zustand](https://github.com/pmndrs/zustand) or Context.

## Appendix - Tests[​](https://mkosir.github.io/typescript-style-guide/#appendix---tests) <a href="#appendix---tests" id="appendix---tests"></a>

### What & How To Test[​](https://mkosir.github.io/typescript-style-guide/#what--how-to-test) <a href="#what--how-to-test" id="what--how-to-test"></a>

Automated test comes with benefits that helps us write better code and makes it easy to refactor, while bugs are caught earlier in the process.\
Consider trade-offs of what and how to test to achieve confidence application is working as intended, while writing and maintaining tests doesn't slow the team down.

✅ Do:

* Implement test to be short, explicit, and pleasant to work with. Intent of a test should be immediately visible.
* Strive for AAA pattern, to maintain clean, organized, and understandable unit tests.
  * Arrange - Setup preconditions or the initial state necessary for the test case. Create necessary objects and define input values.
  * Act - Perform the action you want to unit test (invoke a method, triggering an event etc.). **Strive for minimal number of actions**.
  * Assert - Validate the outcome against expectations. **Strive for minimal number of asserts**.\
    A rule "unit tests should fail for exactly one reason" doesn't need to apply always, but it can indicate a code smell if there are tests with many asserts in codebase.
* As mentioned in [function conventions](https://mkosir.github.io/typescript-style-guide/#functions) try to keep them pure, and impure one small and focused.\
  It makes them easy to test, by passing args and observing return values, since we will **rarely need to mock dependencies**.
* Strive to write tests in a way your app is used by a user, meaning test business logic.\
  E.g. For a specific user role or permission, given some input, we receive the expected output from the process.
* Make tests as isolated as possible, where they don't depend on order of execution and should run independently with its own local storage, session storage, data, cookies etc. Test isolation improves reproducibility, makes debugging easier and prevents cascading test failures.
* Tests should be resilient to changes.
  * Black box testing - Always test only implementation that is publicly exposed, don't write fragile tests on how implementation works internally.
  * Query HTML elements based on attributes that are unlikely to change. Order of priority must be followed as specified in [Testing Library](https://testing-library.com/docs/queries/about/#priority) - [role](https://testing-library.com/docs/queries/byrole), [label](https://testing-library.com/docs/queries/bylabeltext), [placeholder](https://testing-library.com/docs/queries/byplaceholdertext), [text contents](https://testing-library.com/docs/queries/bytext), [display value](https://testing-library.com/docs/queries/bydisplayvalue), [alt text](https://testing-library.com/docs/queries/byalttext), [title](https://testing-library.com/docs/queries/bytitle), [test ID](https://testing-library.com/docs/queries/bytestid).
  * If testing with a database then make sure you control the data. If test are run against a staging environment make sure it doesn't change.

❌ Don't:

* Don't test implementation details. When refactoring code, tests shouldn't change.
* Don't re-test the library/framework.
* Don't mandate 100% code coverage for applications.
* Don't test third-party dependencies. Only test what your team controls (package, API, microservice etc.). Don't test external sites links, third party servers, packages etc.
*   Don't test just to test.

    ```typescript
    // ✖️ Avoid
    it('should render the user list', () => {
      render(<UserList />);
      expect(screen.getByText('Users List')).toBeInTheDocument();
    });
    ```

### Test Description[​](https://mkosir.github.io/typescript-style-guide/#test-description) <a href="#test-description" id="test-description"></a>

All test descriptions must follow naming convention as `it('should ... when ...')`.\
[Eslint rule](https://github.com/jest-community/eslint-plugin-jest/blob/main/docs/rules/valid-title.md#mustmatch--mustnotmatch) implements regex:

```typescript
// .eslintrc.js
'jest/valid-title': [
  'error',
  {
    mustMatch: { it: [/should.*when/u.source, "Test title must include 'should' and 'when'"] },
  },
],
```

```typescript
// ✖️ Avoid
it('accepts ISO date format where date is parsed and formatted as YYYY-MM');
it('after title is confirmed user description is rendered');

// ✅ Name test description as it('should ... when ...')
it('should return parsed date as YYYY-MM when input is in ISO date format');
it('should render user description when title is confirmed');
```

### Test Tooling[​](https://mkosir.github.io/typescript-style-guide/#test-tooling) <a href="#test-tooling" id="test-tooling"></a>

Tests can be run through scripts, but it's highly encouraged to use [Jest Runner](https://marketplace.visualstudio.com/items?itemName=firsttris.vscode-jest-runner) and [Playwright Test](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright) VS code extension alongside.\
With extension any single [unit/integration](https://github.com/mkosir/typescript-style-guide/raw/main/misc/vscode-jest-runner.gif) or [E2E](https://github.com/mkosir/typescript-style-guide/raw/main/misc/vscode-playwright-test.gif) test can be run instantly, especially if testing app or package in larger monorepo codebase.

```shell
code --install-extension firsttris.vscode-jest-runner
code --install-extension ms-playwright.playwright
```
