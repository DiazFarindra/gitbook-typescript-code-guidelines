# Classes are Useful

It is very common to have the following structure:

```typescript
function foo() {
    let someProperty;

    // Some other initialization code

    function someMethod() {
        // Do some stuff with `someProperty`
        // And potentially other things
    }
    // Maybe some other methods

    return {
        someMethod,
        // Maybe some other methods
    };
}
```

This is known as the _revealing module pattern_ and quite common in JavaScript (taking advantage of JavaScript closure).

If you use [_file modules_ (which you really should as global scope is bad)](classes-are-useful.md#modules) then _your file is effectively the same_. However, there are too many cases where people will write code like the following:

```typescript
let someProperty;

function foo() {
   // Some initialization code
}
foo(); // some initialization code

someProperty = 123; // some more initialization

// Some utility function not exported

// later
export function someMethod() {

}
```

Even though I am not a big fan of inheritance _I do find that letting people use classes helps them organize their code better_. The same developer would intuitively write the following:

```typescript
class Foo {
    public someProperty;

    constructor() {
        // some initialization
    }

    public someMethod() {
        // some code
    }

    private someUtility() {
        // some code
    }
}

export = new Foo();
```

And its not just developers, creating dev tools that provide great visualizations over classes are much more common, and there is one less pattern your team needs to understand and maintain.

> PS: There is nothing wrong in my opinion with _shallow_ class hierarchies if they provide significant reuse and reduction in boiler plate.

### Modules <a href="#modules" id="modules"></a>

#### Global Module <a href="#global-module" id="global-module"></a>

By default when you start typing code in a new TypeScript file your code is in a _global_ namespace. As a demo consider a file `foo.ts`:

If you now create a _new_ file `bar.ts` in the same project, you will be _allowed_ by the TypeScript type system to use the variable `foo` as if it was available globally:

```typescript
var bar = foo; // allowed
```

Needless to say having a global namespace is dangerous as it opens your code up for naming conflicts. We recommend using file modules which are presented next.

#### File Module <a href="#file-module" id="file-module"></a>

Also called _external modules_. If you have an `import` or an `export` at the root level of a TypeScript file then it creates a _local_ scope within that file. So if we were to change the previous `foo.ts` to the following (note the `export` usage):

We will no longer have `foo` in the global namespace. This can be demonstrated by creating a new file `bar.ts` as follows:

```typescript
var bar = foo; // ERROR: "cannot find name 'foo'"
```

If you want to use stuff from `foo.ts` in `bar.ts` _you need to explicitly import it_. This is shown in an updated `bar.ts` below:

```typescript
import { foo } from "./foo";
var bar = foo; // allowed
```

Using an `import` in `bar.ts` not only allows you to bring in stuff from other files, but also marks the file `bar.ts` as a _module_ and therefore, declarations in `bar.ts` don't pollute the global namespace either.

What JavaScript is generated from a given TypeScript file that uses external modules is driven by the compiler flag called `module`.
