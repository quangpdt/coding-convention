# TypeScript & Angular
## Principles, Style Guide and Coding Convention

> **Disclaimer**: This note is not originally written by me. 
I collected information from repositories on Github as well as from my co-workers and try to combine them as much as possible. 
If you found any issue or want to contribute to this note, feel free to make a PR. Peace!

## Principles

### Tools
Configuration is rules. Therefore, configuration can be implemented by Tools. 

> Suggested tools:

- Suggested IDE: [IntelliJ](https://www.jetbrains.com/idea/), [WebStorm](https://www.jetbrains.com/webstorm/)
- Suggested rules definition tools: [TSLint](https://palantir.github.io/tslint/)
- Suggest code formatting tools: [Prettier](https://prettier.io/)


### Write code for humans

1. Readability
2. Use full words instead of abbreviations 
3. Be nice to your teammates

### Make it work, then make it better
Don't optimise for performance prior it works

### Ask for code review
1. Discussing code is fun and leads to better quality. 
2. You are responsible for all your code, you write. 
3. Four eyes see more than two.

## StyleGuide & Coding Convention
Key Sections:

* [Variable](#variable-and-function)
* [Class](#class)
* [Interface](#interface)
* [Type](#type)
* [Namespace](#namespace)
* [Enum](#enum)
* [`null` vs. `undefined`](#null-vs-undefined)
* [Formatting](#formatting)
* [Single vs. Double Quotes](#quotes)
* [Tabs vs. Spaces](#spaces)
* [Use semicolons](#semicolons)
* [Annotate Arrays as `Type[]`](#array)
* [File Names](#filename)
* [`type` vs `interface`](#type-vs-interface)
* [TSLint Rules](#tslint-rules)
* [Editor Config](#editor-config)
* [Prettier Rules](#prettier-rules)

### Variable and Function
* Generally

    - Use PascalCase for type names.
    - Do not use "I" as a prefix for interface names.
    - Use PascalCase for enum values.
    - Use camelCase for function names.
    - Use camelCase for property names and local variables.
    - Do not use "_" as a prefix for private properties.
    - Use whole words in names when possible.
    
* Use a single declaration per variable statement 
    > E.g: Use `var x = 1; var y = 2;` over `var x = 1, y = 2;`.

* Use `camelCase` for variable and function names

    > Reason: Conventional JavaScript
    
    **Bad**
    ```ts
    var FooVar;
    function BarFunc() { }
    ```
    **Good**
    ```ts
    let fooVar;
    function barFunc() { }
    ```

* Try to use positive expectation, be optimistic

    > Reason: Make the others feel positive when reading your code
    
    **Bad**
    ```ts
    isFailed = true;
    ```
    
    **Good**
    ```ts
    isSuccessful = false;
    ```

* Do not use "_" as a prefix for private properties.
    > Refer to [TSLint Variable Name](https://palantir.github.io/tslint/rules/variable-name/)

### Class
* Use `PascalCase` for class names.

    > Reason: This is actually fairly conventional in standard JavaScript.
    
    **Bad**
    ```ts
    class foo { }
    ```
    **Good**
    ```ts
    class Foo { }
    ```
    
* Use `camelCase` of class members and methods

    > Reason: Naturally follows from variable and function naming convention.
    
    **Bad**
    ```ts
    class Foo {
        Bar: number;
        Baz() { }
    }
    ```
    **Good**
    ```ts
    class Foo {
        bar: number;
        baz() { }
    }
    ```
    
### Interface

* Use `PascalCase` for name.

    > Reason: Similar to class
    
    * Use `camelCase` for members.
    
    > Reason: Similar to class

* **Don't** prefix with `I`

    > Reason: Unconventional. `lib.d.ts` defines important interfaces without an `I` (e.g. Window, Document etc).
    
    **Bad**
    ```ts
    interface IFoo {
    }
    ```
    **Good**
    ```ts
    interface Foo {
    }
    ```

### Type

* Use `PascalCase` for name.

    > Reason: Similar to class

* Use `camelCase` for members.

    > Reason: Similar to class


### Namespace

* Use `PascalCase` for names

    > Reason: Convention followed by the TypeScript team. Namespaces are effectively just a class with static members. Class names are `PascalCase` => Namespace names are `PascalCase`
    
    **Bad**
    ```ts
    namespace foo {
    }
    ```
    **Good**
    ```ts
    namespace Foo {
    }
    ```

### Enum

* Use `PascalCase` for enum names

    > Reason: Similar to Class. Is a Type.
    
    **Bad**
    ```ts
    enum color {
    }
    ```
    **Good**
    ```ts
    enum Color {
    }
    ```

* Use `PascalCase` for enum member

    > Reason: Convention followed by TypeScript team i.e. the language creators e.g `SyntaxKind.StringLiteral`. Also helps with translation (code generation) of other languages into TypeScript.
    
    **Bad**
    ```ts
    enum Color {
        red
    }
    ```
    **Good**
    ```ts
    enum Color {
        Red
    }
    ```

### Null vs. Undefined

* Prefer not to use either for explicit unavailability

    > Reason: these values are commonly used to keep a consistent structure between values. In TypeScript you use *types* to denote the structure
    
    **Bad**
    ```ts
    let foo = {x:123,y:undefined};
    ```
    **Good**
    ```ts
    let foo:{x:number,y?:number} = {x:123};
    ```

* Use `undefined` in general (do consider returning an object like `{valid:boolean,value?:Foo}` instead)

    ***Bad***
    ```ts
    return null;
    ```
    ***Good***
    ```ts
    return undefined;
    ```

* Use `null` where its a part of the API or conventional

    > Reason: It is conventional in Node.js e.g. `error` is `null` for NodeBack style callbacks.
    
    **Bad**
    ```ts
    callback(undefined)
    ```
    **Good**
    ```ts
    callback(null)
    ```

* Use *truthy* check for **objects** being `null` or `undefined`

    **Bad**
    ```ts
    if (error === null)
    ```
    **Good**
    ```ts
    if (error)
    ```

* Use `== undefined` / `!= undefined` (not `===` / `!==`) to check for `null` / `undefined` on primitives as it works for both `null`/`undefined` but not other falsy values (like `''`,`0`,`false`) e.g.

    **Bad**
    ```ts
    if (error !== null)
    ```
    **Good**
    ```ts
    if (error != undefined)
    ```

### Quotes

* Prefer single quotes (`'`) unless escaping.

    > Reason: More JavaScript teams do this (e.g. [airbnb](https://github.com/airbnb/javascript), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)). Its easier to type (no shift needed on most keyboards). [Prettier team recommends single quotes as well](https://github.com/prettier/prettier/issues/1105)
    
    > Double quotes are not without merit: Allows easier copy paste of objects into JSON. Allows people to use other languages to work without changing their quote character. Allows you to use apostrophes e.g. `He's not going.`. But I'd rather not deviate from where the JS Community is fairly decided.

* When you can't use double quotes, try using back ticks (\`).

    > Reason: These generally represent the intent of complex enough strings.

### Spaces

* Use `4` spaces. Not tabs.

    > Reason: More JavaScript teams do this (e.g. [airbnb](https://github.com/airbnb/javascript), [idiomatic](https://github.com/rwaldron/idiomatic.js), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)). The TypeScript/VSCode teams use 4 spaces but are definitely the exception in the ecosystem.

### Semicolons

* Use semicolons.

    > Reasons: Explicit semicolons helps language formatting tools give consistent results. Missing ASI (automatic semicolon insertion) can trip new devs e.g. `foo() \n (function(){})` will be a single statement (not two). Recommended by TC39 as well.

### Array

* Annotate arrays as `foos:Foo[]` instead of `foos:Array<Foo>`.

    > Reasons: Its easier to read. Its used by the TypeScript team. Makes easier to know something is an array as the mind is trained to detect `[]`.

### Filename
- Name files with `kebab-case`. E.g. `accordian.tsx`, `my-control.tsx`, `navigation-controller.ts`, etc.

    > Reason: Conventional across many JS teams.

- 1 file per logical component (e.g. parser, scanner, emitter, checker).


### type vs. interface

Prefer Interface over Type

> Reason: Interfaces are generally preferred over type literals because interfaces can be implemented, extended and merged.
  
### TSLint Rules

Follow generated Angular CLI project. Sample `tslint.json` file can be found [from here](https://github.com/angular/angular-cli/blob/0bef5aa4be/tests/angular_devkit/build_ng_packagr/ng-packaged/tslint.json).

```json
{
  "rulesDirectory": [
    "node_modules/codelyzer"
  ],
  "rules": {
    "arrow-return-shorthand": true,
    "callable-types": true,
    "class-name": true,
    "comment-format": [
      true,
      "check-space"
    ],
    "curly": true,
    "deprecation": {
      "severity": "warn"
    },
    "eofline": true,
    "forin": true,
    "import-blacklist": [
      true,
      "rxjs/Rx"
    ],
    "import-spacing": true,
    "indent": [
      true,
      "spaces"
    ],
    "interface-over-type-literal": true,
    "label-position": true,
    "max-line-length": [
      true,
      140
    ],
    "member-access": false,
    "member-ordering": [
      true,
      {
        "order": [
          "static-field",
          "instance-field",
          "static-method",
          "instance-method"
        ]
      }
    ],
    "no-arg": true,
    "no-bitwise": true,
    "no-console": [
      true,
      "debug",
      "info",
      "time",
      "timeEnd",
      "trace"
    ],
    "no-construct": true,
    "no-debugger": true,
    "no-duplicate-super": true,
    "no-empty": false,
    "no-empty-interface": true,
    "no-eval": true,
    "no-inferrable-types": [
      true,
      "ignore-params"
    ],
    "no-misused-new": true,
    "no-non-null-assertion": true,
    "no-redundant-jsdoc": true,
    "no-shadowed-variable": true,
    "no-string-literal": false,
    "no-string-throw": true,
    "no-switch-case-fall-through": true,
    "no-trailing-whitespace": true,
    "no-unnecessary-initializer": true,
    "no-unused-expression": true,
    "no-use-before-declare": true,
    "no-var-keyword": true,
    "object-literal-sort-keys": false,
    "one-line": [
      true,
      "check-open-brace",
      "check-catch",
      "check-else",
      "check-whitespace"
    ],
    "prefer-const": true,
    "quotemark": [
      true,
      "single"
    ],
    "radix": true,
    "semicolon": [
      true,
      "always"
    ],
    "triple-equals": [
      true,
      "allow-null-check"
    ],
    "typedef-whitespace": [
      true,
      {
        "call-signature": "nospace",
        "index-signature": "nospace",
        "parameter": "nospace",
        "property-declaration": "nospace",
        "variable-declaration": "nospace"
      }
    ],
    "unified-signatures": true,
    "variable-name": false,
    "whitespace": [
      true,
      "check-branch",
      "check-decl",
      "check-operator",
      "check-separator",
      "check-type"
    ],
    "no-output-on-prefix": true,
    "use-input-property-decorator": true,
    "use-output-property-decorator": true,
    "use-host-property-decorator": true,
    "no-input-rename": true,
    "no-output-rename": true,
    "use-life-cycle-interface": true,
    "use-pipe-transform-interface": true,
    "component-class-suffix": true,
    "directive-class-suffix": true
  }
}
```

### Prettier Rules
```json
{
  "trailingComma": "none",
  "tabWidth": 4,
  "proseWrap": "never",
  "printWidth": 120,
  "singleQuote": true,
  "overrides": [
    {
      "files": "src/**/*.html",
      "options": {
        "parser": "angular"
      }
    }
  ]
}
```

### Editor Config

```
# Editor configuration, see http://editorconfig.org
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 4
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
max_line_length = off
trim_trailing_whitespace = false

```


# License
- A part of StyleGuide from [TypeScript Book](https://github.com/basarat/typescript-book/blob/master/docs/styleguide/styleguide.md)
- Refer to Principles & Code Style from **Mattu, Front-end lead of Scout24**
