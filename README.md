# Divisio JavaScript Style Guide = () => (

*Our mostly reasonable approach to JavaScript*

Keep in mind that some rules will automatically be fixed with [Prettier](https://prettier.io/), and others will not. You should still take good care of the code. This guide is available in other languages too. See [Translation](#translation)

Other Style Guides

  - [React](react/)

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [References](#references)
  1. [Objects](#objects)  
  1. [Arrays](#arrays) 
  1. [Destructuring](#destructuring) 
  1. [Strings](#strings) 
  1. [Functions](#functions) 
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks) Needs code format and team review
  1. [Control Statements](#control-statements) Needs code format and team review
  1. [Comments](#comments) Needs code format and team review
  1. [Whitespace](#whitespace) Needs code format and team review
  1. [Commas](#commas) Needs code format and team review
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion) Needs code format and team review
  1. [Naming Conventions](#naming-conventions) Needs code format and team review
  1. [Accessors](#accessors) Needs code format and team review
  1. [Events](#events) Needs code format and team review
  1. [jQuery](#jquery) Needs code format and team review
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility) Needs code format and team review
  1. [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles) Needs code format and team review
  1. [Testing](#testing) Needs code format and team review
  1. [Translation](#translation) Needs code format and team review

## Basic Rules

  - Do not use `;` at the end of each line.
  - Always name **everything** in english as a pattern.

**[⬆ back to top](#table-of-contents)**

## References

  - Use `const` for all of your references, and avoid using `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1
    var b = 2

    // good
    const a = 1
    const b = 2
    ```

  - If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1
    if (true) {
      count += 1
    }

    // good, use the let.
    let count = 1
    if (true) {
      count += 1
    }
    ```

    > **Note**: Note that both `let` and `const` are block-scoped.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1
      const b = 1
    }
    console.log(a) // ReferenceError
    console.log(b) // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - Use the literal syntax for object creation. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object()

    // good
    const item = {}
    ```

  - Use computed property names when creating objects with dynamic property names.

    > Why? They allow you to define all the properties of an object in one place.

    ```javascript

    const getKey = k => `a key named ${k}`

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    }
  
    obj[getKey('enabled')] = true

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    }
    ```

  - Use object method shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value
      },
    }

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value
      },
    }
    ```

    > **Note**: This rule does not flag arrow functions inside of object literals. The following  will not warn!

    ```javascript
    /*eslint object-shorthand: "error"*/
    /*eslint-env es6*/

    var foo = {
        x: y => y
    }
    ```

  - Use property value shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Why? It is shorter and descriptive.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker'

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    }

    // good
    const obj = {
      lukeSkywalker,
    }
    ```

  - Group your shorthand properties at the beginning of your object declaration.

    > Why? It’s easier to tell which properties are using the shorthand.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker'
    const lukeSkywalker = 'Luke Skywalker'

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    }

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    }
    ```

  - Only quote properties that are invalid identifiers. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    }

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    }
    ```

  - Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`).

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key))

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key))

    // best
    const has = Object.prototype.hasOwnProperty // cache the lookup once, in module scope.
    console.log(has.call(object, key))
    /* or */
    import has from 'has' // https://www.npmjs.com/package/has
    console.log(has(object, key))
    ```

  - Prefer the object spread operator over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

    ```javascript
    // very bad
    const original = { a: 1, b: 2 }
    const copy = Object.assign(original, { c: 3 }) // this mutates `original`
    delete copy.a // so does this

    // bad
    const original = { a: 1, b: 2 }
    const copy = Object.assign({}, original, { c: 3 }) // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 }
    const copy = { ...original, c: 3 } // copy => { a: 1, b: 2, c: 3 }

    const { a, ...rest } = copy // rest => { b: 2, c: 3 }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array()

    // good
    const items = []
    ```

  - Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

    ```javascript
    const someStack = []

    // bad
    someStack[someStack.length] = 'abracadabra'

    // good
    someStack.push('abracadabra')
    ```

  - Use array spreads `...` to copy arrays.

    ```javascript
    // bad
    const len = items.length
    const itemsCopy = []
    let i

    for (i = 0 i < len i += 1) {
      itemsCopy[i] = items[i]
    }

    // good
    const itemsCopy = [...items]
    ```

  - To convert an iterable object to an array, use spreads `...` instead of [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo')

    // good
    const nodes = Array.from(foo)

    // best
    const nodes = [...foo]
    ```

  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) for converting an array-like object to an array.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 }

    // bad
    const arr = Array.prototype.slice.call(arrLike)

    // good
    const arr = Array.from(arrLike)
    ```

  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) instead of spread `...` for mapping over iterables, because it avoids creating an intermediate array.

    ```javascript
    // bad
    const baz = [...foo].map(bar)

    // good
    const baz = Array.from(foo, bar)
    ```

  - Use return statements in array method callbacks. It’s ok to omit the return if the function body consists of a single statement returning an expression without side effects. eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map(x => {
      const y = x + 1
      return x * y
    })

    // good
    [1, 2, 3].map(x => x + 1)

    // bad - no returned value means `acc` becomes undefined after the first iteration
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item)
    })

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item)
      return flatten
    })

    // bad
    inbox.filter(msg => {
      const { subject, author } = msg
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee'
      } else {
        return false
      }
    })

    // good
    inbox.filter(msg => {
      const { subject, author } = msg
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee'
      }

      return false
    })
    ```

  - Use line breaks after open and before close array brackets if an array has multiple lines

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ]

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }]

    const numberInArray = [
      1, 2,
    ]

    // good
    const arr = [[0, 1], [2, 3], [4, 5]]

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ]

    const numberInArray = [
      1,
      2,
    ]
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  - Use object destructuring when accessing and using multiple properties of an object. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // bad
    const getFullName = user => {
      const firstName = user.firstName
      const lastName = user.lastName

      return `${firstName} ${lastName}`
    }

    // good
    const getFullName = user => {
      const { firstName, lastName } = user
      return `${firstName} ${lastName}`
    }

    // best
    const getFullName = ({ firstName, lastName }) => `${firstName} ${lastName}`
    ```

  - Use array destructuring. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4]

    // bad
    const first = arr[0]
    const second = arr[1]

    // good
    const [first, second] = arr
    ```

  - Use object destructuring for multiple return values, not array destructuring.

    > Why? You can add new properties over time or change the order of things without breaking call sites.

    ```javascript
    // bad
    const processInput = input => {
      // ...
      return [left, right, top, bottom]
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input)

    // good
    const processInput = input => {
      // ...
      return { left, right, top, bottom }
    }

    // the caller selects only the data he needs
    const { left, top } = processInput(input)
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - Use single quotes `''` for strings. eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // bad
    const name = "Capt. Janeway"

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`

    // good
    const name = 'Capt. Janeway'
    ```

  - Strings that cause the line to go over 100 characters should **not** be written across multiple lines using string concatenation.

    > Why? Broken strings are painful to work with and make code less searchable.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.'

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.'

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'
    ```

  - When programmatically building up strings, use template strings instead of concatenation. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // bad
    const sayHi = name => 'How are you, ' + name + '?'

    // bad
    const sayHi = name => ['How are you, ', name, '?'].join()

    // bad
    const sayHi = name => `How are you, ${ name }?`

    // good
    const sayHi = name => `How are you, ${name}?`
    ```

  - Never use `eval()` on a string, it opens too many vulnerabilities. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  - Do not unnecessarily escape characters in strings. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > Why? Backslashes harm readability, thus they should only be present when necessary.

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"'

    // good
    const foo = '\'this\' is "quoted"'
    const foo = `my name is '${name}'`
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  - Prefer to use arrow functions when it's possible. See [Arrow Functions](#arrow-functions)

  - Use named function expressions instead of function declarations. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to explicitly name the expression, regardless of whether or not the name is inferred from the containing variable (which is often the case in modern browsers or when using compilers such as Babel). This eliminates any assumptions made about the Error’s call stack. ([Discussion](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // bad
    const foo = function () {
      // ...
    }

    // good
    // lexical name distinguished from the variable-referenced invocation(s)
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    }
    ```

  - Wrap immediately invoked function expressions in parentheses. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.')
    }())
    ```

  - Never declare a function in a non-function block (`if`, `while`, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  > **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement.

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.')
      }
    }

    // good
    let test
    if (currentUser) {
      test = () => {
        console.log('Yup.')
      }
    }
    ```

  - Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  - Never use `arguments`, opt to use rest syntax `...` instead. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Why? `...` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`.

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments)
      return args.join('')
    }

    // good
    function concatenateAll(...args) {
      return args.join('')
    }
    ```

  - Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // really bad
    function handleThings(opts) {
      // We shouldn’t mutate function arguments.
      opts = opts || {}
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {}
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - Avoid side effects with default parameters.

    > Why? They are confusing to reason about.

    ```javascript
    var b = 1
  
    // bad
    function count(a = b++) {
      console.log(a)
    }
  
    count()  // 1
    count()  // 2
    count(3) // 3
    count()  // 3
    ```

  - Always put default parameters last.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  - Never use the Function constructor to create a new function. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > Why? Creating a function in this way evaluates a string similarly to `eval()`, which opens vulnerabilities.

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b')

    // still bad
    var subtract = Function('a', 'b', 'return a - b')
    ```

  - Spacing in a function signature. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name.

    ```javascript
    // bad
    const f = function(){}
    const g = function (){}
    const h = function() {}

    // good
    const x = function () {}
    const y = function a() {}
    ```

  - Never mutate parameters. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller.

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1
    }

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1
    }
    ```

  - Never reassign parameters. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

    ```javascript
    // bad
    function f1(a) {
      a = 1
      // ...
    }

    function f2(a) {
      if (!a) { a = 1 }
      // ...
    }

    // good
    function f3(a) {
      const b = a || 1
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  - Prefer the use of the spread operator `...` to call variadic functions. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Why? It’s cleaner, you don’t need to supply a context, and you can not easily compose `new` with `apply`.

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5]
    console.log.apply(console, x)

    // good
    const x = [1, 2, 3, 4, 5]
    console.log(...x)

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]))

    // good
    new Date(...[2016, 8, 5])
    ```

  - Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, with a trailing comma on the last item. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // good
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
      bar,
      baz)

    // good
    console.log(
      foo,
      bar,
      baz,
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Arrow Functions

  - Always prefer to use arrow functions instead of normal functions if you can.
    > Why? Consistency is always a good thing, and the syntax becomes more concise.

  - When you must use an anonymous function (as when passing an inline callback), use arrow function notation. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want.

    > Why not? If you have a fairly complicated function, you might move that logic out into its own named function expression.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1
      return x * y
    })

    // good
    [1, 2, 3].map(x => {
      const y = x + 1
      return x * y
    })
    ```

  - If the function body consists of a single statement returning an [expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) without side effects, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > Why? Syntactic sugar. It reads well when multiple functions are chained together.

    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1
      `A string containing the ${nextNumber}.`
    })

    // good
    [1, 2, 3].map(number => `A string containing the ${number + 1}.`)

    // good
    [1, 2, 3].map(number => {
      const nextNumber = number + 1
      return `A string containing the ${nextNumber}.`
    })

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }))
    ```

  - If there is only one argument, do not include parentheses around arguments for clarity and consistency.

    ```javascript
    // bad
    [1, 2, 3].map(x => x * x)

    // good
    [1, 2, 3].map(x => x * x)


    // bad
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ))

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ))
    ```

  - Avoid confusing arrow function syntax (`=>`) with comparison operators (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // bad
    const itemHeight = item => item.height <= 256 ? item.largeSize : item.smallSize

    // bad
    const itemHeight = item => item.height >= 256 ? item.largeSize : item.smallSize

    // good
    const itemHeight = item => (item.height <= 256 ? item.largeSize : item.smallSize)

    // good
    const itemHeight = item => {
      const { height, largeSize, smallSize } = item
      return height <= 256 ? largeSize : smallSize
    }
    ```

  - Enforce the location of arrow function bodies with implicit returns. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // bad
    foo =>
      bar

    foo =>
      (bar)

    // good
    foo => bar
    foo => (bar)
    foo => (
       bar
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

  - Class make your code more concise and self-documenting, and it's a great feature of ES6. But just because you have a hammer that doesn't mean everything is now a nail. Try to figure out what works best and be aware of the caveats before deciding whether you need a class or not. You can see the next topics as suggestions, not rules.

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents]
    }

    Queue.prototype.pop = function () {
      const value = this.queue[0]
      this.queue.splice(0, 1)
      return value
    }

    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents]
      }

      pop() {
        const value = this.queue[0]
        this.queue.splice(0, 1)
        return value
      }
    }
    ```

  - Use `extends` for inheritance.

    > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // bad
    const inherits = require('inherits')

    function PeekableQueue(contents) {
      Queue.apply(this, contents)
    }

    inherits(PeekableQueue, Queue)

    PeekableQueue.prototype.peek = function () {
      return this.queue[0]
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0]
      }
    }
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true
      return true
    }

    Jedi.prototype.setHeight = function (height) {
      this.height = height
    }

    const luke = new Jedi()
    luke.jump() // => true
    luke.setHeight(20) // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true
        return this
      }

      setHeight(height) {
        this.height = height
        return this
      }
    }

    const luke = new Jedi()

    luke.jump()
      .setHeight(20)
    ```

  - It’s okay to write a custom `toString()` method, just make sure it works successfully and causes no side effects.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name'
      }

      getName() {
        return this.name
      }

      toString() {
        return `Jedi - ${this.getName()}`
      }
    }
    ```

  - Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args)
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args)
        this.name = 'Rey'
      }
    }
    ```

  - Avoid duplicate class members. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Why? Duplicate class member declarations will silently prefer the last one - having duplicates is almost certainly a bug.

    ```javascript
    // bad
    class Foo {
      bar() { return 1 }
      bar() { return 2 }
    }

    // good
    class Foo {
      bar() { return 1 }
    }

    // good
    class Foo {
      bar() { return 2 }
    }
    ```

  - Class methods should use `this` or be made into a static method unless an external library or framework requires to use specific non-static methods. Being an instance method should indicate that it behaves differently based on properties of the receiver. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // bad
    class Foo {
      bar() {
        console.log('bar')
      }
    }

    // good - this is used
    class Foo {
      bar() {
        console.log(this.bar)
      }
    }

    // good - constructor is exempt
    class Foo {
      constructor() {
        // ...
      }
    }

    // good - static methods aren't expected to use this
    class Foo {
      static bar() {
        console.log('bar')
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Modules

  - Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

    > Why? Modules are the future, let’s start using the future now.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide')
    module.exports = AirbnbStyleGuide.es6

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide'
    export default AirbnbStyleGuide.es6

    // best
    import { es6 } from './AirbnbStyleGuide'
    export default es6
    ```

  - Do not use wildcard imports.

    > Why? This makes sure you have a single default export.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide'

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide'
    ```

  - And do not export directly from an import.

    > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

    ```javascript
    // bad
    // es6.js
    export { es6 as default } from './AirbnbStyleGuide'

    // good
    // es6.js
    import { es6 } from './AirbnbStyleGuide'
    export default es6
    ```

  - Only import from a path in one place.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > Why? Having multiple lines that import from the same path can make code harder to maintain.

    ```javascript
    // bad
    import foo from 'foo'
    // … some other imports … //
    import { named1, named2 } from 'foo'

    // good
    import foo, { named1, named2 } from 'foo'

    // good
    import foo, {
      named1,
      named2,
    } from 'foo'
    ```

  - Do not export mutable bindings.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Why? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported.

    ```javascript
    // bad
    let foo = 3
    export { foo }

    // good
    const foo = 3
    export { foo }
    ```

  - In modules with a single export, prefer default export over named export. Always export after name it. 
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Why? To encourage more files that only ever export one thing, which is better for readability and maintainability.

    ```javascript
    // bad
    export function foo() {}

    // still bad
    export default function foo() {}

    // good
    const foo = function foo() {}

    export default foo
    ```

  - Put all `import`s above non-import statements.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Why? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

    ```javascript
    // bad
    import foo from 'foo'
    foo.init()

    import bar from 'bar'

    // good
    import foo from 'foo'
    import bar from 'bar'

    foo.init()
    ```

  - Multiline imports should be indented just like multiline array and object literals.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > Why? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas.

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path'

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path'
    ```

  - Disallow Webpack loader syntax in module import statements.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Why? Since using Webpack syntax in the imports couples the code to a module bundler. Prefer using the loader syntax in `webpack.config.js`.

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss'
    import barCss from 'style!css!bar.css'

    // good
    import fooSass from 'foo.scss'
    import barCss from 'bar.css'
    ```

  - Do not include JavaScript filename extensions
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > Why? Including extensions inhibits refactoring, and inappropriately hardcodes implementation details of the module you're importing in every consumer.

    ```javascript
    // bad
    import foo from './foo.js'
    import bar from './bar.jsx'
    import baz from './baz/index.jsx'

    // good
    import foo from './foo'
    import bar from './bar'
    import baz from './baz'
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators

  - Prefer JavaScript’s higher-order functions instead of loops like `for-in` or `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects.

    > Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... to iterate over arrays, and `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5]

    // bad
    let sum = 0
    for (let num of numbers) {
      sum += num
    }
    sum === 15

    // good
    let sum = 0
    numbers.forEach(num => {
      sum += num
    })
    sum === 15

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0)
    sum === 15

    // bad
    const increasedByOne = []
    for (let i = 0 i < numbers.length i++) {
      increasedByOne.push(numbers[i] + 1)
    }

    // good
    const increasedByOne = []
    numbers.forEach(num => {
      increasedByOne.push(num + 1)
    })

    // best (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1)
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  - Use dot notation when accessing properties. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    }

    // bad
    const isJedi = luke['jedi']

    // good
    const isJedi = luke.jedi
    ```

  - Use bracket notation `[]` when accessing properties with a variable.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    }

    function getProp(prop) {
      return luke[prop]
    }

    const isJedi = getProp('jedi')
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  - Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower()

    // good
    const superPower = new SuperPower()
    ```

  - Use one `const` or `let` declaration per variable or assignment. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Why? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once. Same thing when a error pops up related to this declaration chain

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z'

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true
        dragonball = 'z'

    // good
    const items = getItems()
    const goSportsTeam = true
    const dragonball = 'z'
    ```

  - Group all your `const`s and then group all your `let`s.

    > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true

    // bad
    let i
    const items = getItems()
    let dragonball

    // good
    const goSportsTeam = true
    const items = getItems()
    let dragonball
    let i
    let length
    ```

  - Assign variables where you need them, but place them in a reasonable place.

    > Why? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // bad - unnecessary function call
    function checkName(hasName) {
      const name = getName()

      if (hasName === 'test') {
        return false
      }

      if (name === 'test') {
        this.setName('')
        return false
      }

      return name
    }

    // good
    function checkName(hasName) {
      if (hasName === 'test') {
        return false
      }

      const name = getName()

      if (name === 'test') {
        this.setName('')
        return false
      }

      return name
    }
    ```

  - Don’t chain variable assignments. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > Why? Chaining variable assignments creates implicit global variables.

    ```javascript
    // bad
    (() => {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) )
      // The let keyword only applies to variable a variables b and c become
      // global variables.
      let a = b = c = 1
    })()

    console.log(a) // throws ReferenceError
    console.log(b) // 1
    console.log(c) // 1

    // good
    (() => {
      let a = 1
      let b = a
      let c = a
    })()

    console.log(a) // throws ReferenceError
    console.log(b) // throws ReferenceError
    console.log(c) // throws ReferenceError

    // the same applies for `const`
    ```

  - Avoid using unary increments and decrements (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Why? Per the [eslint documentation](https://eslint.org/docs/rules/no-plusplus), unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

    ```javascript
    // bad

    const array = [1, 2, 3]
    let num = 1
    num++
    --num

    let sum = 0
    let truthyCount = 0
    for (let i = 0 i < array.length i++) {
      let value = array[i]
      sum += value
      if (value) {
        truthyCount++
      }
    }

    // good

    const array = [1, 2, 3]
    let num = 1
    num += 1
    num -= 1

    const sum = array.reduce((a, b) => a + b, 0)
    const truthyCount = array.filter(Boolean).length
    ```

  - Avoid linebreaks before or after `=` in an assignment. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len.html), surround the value in parens. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > Why? Linebreaks surrounding `=` can obfuscate the value of an assignment.

    ```javascript
    // bad
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName()

    // bad
    const foo
      = 'superLongLongLongLongLongLongLongLongString'

    // good
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    )

    // good
    const foo = 'superLongLongLongLongLongLongLongLongString'
    ```

  - Disallow unused variables. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Why? Variables that are declared and not used anywhere in the code are most likely an error due to incomplete refactoring. Such variables take up space in the code and can lead to confusion by readers.

    ```javascript
    // bad

    const some_unused_var = 42

    // Write-only variables are not considered as used.
    let y = 10
    y = 5

    // A read for a modification of itself is not considered as used.
    let z = 0
    z = z + 1

    // Unused function arguments.
    const getX = (x, y) => x

    // good

    const getXPlusY = (x, y) => x + y

    const x = 1
    const y = a + 2

    alert(getXPlusY(x, y))

    // 'type' is ignored even if unused because it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    const { type, ...coords } = data
    // 'coords' is now the 'data' object without its 'type' property.
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  - `var` declarations get hoisted to the top of their closest enclosing function scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone).

    ```javascript
    // we know this wouldn’t work (assuming there
    // is no notDefined global variable)
    const = example = () => {
      console.log(notDefined) // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    const example = () => {
      console.log(declaredButNotAssigned) // => undefined
      var declaredButNotAssigned = true
    }

    // the interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    const example = () => {
      let declaredButNotAssigned
      console.log(declaredButNotAssigned) // => undefined
      declaredButNotAssigned = true
    }

    // using const and let
    const example = () => {
      console.log(declaredButNotAssigned) // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned) // => throws a ReferenceError
      const declaredButNotAssigned = true
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    const example = () => {
      console.log(anonymous) // => undefined

      anonymous() // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression')
      }
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      superPower() // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying')
      }
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      var named = function named() {
        console.log('named')
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower() // => Flying

      function superPower() {
        console.log('Flying')
      }
    }
    
    // Remember that it doesn't happen with arrow functions
    const example = () => {
      superPower() // => ReferenceError:

      const superPower = () => {
        console.log('Flying')
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) Use `===` and `!==` over `==` and `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    - **Objects** evaluate to **true**
    - **Undefined** evaluates to **false**
    - **Null** evaluates to **false**
    - **Booleans** evaluate to **the value of the boolean**
    - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) Use shortcuts for booleans, but explicit comparisons for strings and numbers.

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) For more information see [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > Why? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1
        break
      case 2:
        const y = 2
        break
      case 3:
        function f() {
          // ...
        }
        break
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1
        break
      }
      case 2: {
        const y = 2
        break
      }
      case 3: {
        function f() {
          // ...
        }
        break
      }
      case 4:
        bar()
        break
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Ternaries should not be nested and generally be single line expressions. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) Avoid unneeded ternary statements. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // bad
    const foo = a ? a : b
    const bar = c ? true : false
    const baz = c ? false : true

    // good
    const foo = a || b
    const bar = !!c
    const baz = !c
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: `+`, `-`, and `**` since their precedence is broadly understood. We recommend enclosing `/` and `*` in parentheses because their precedence can be ambiguous when they are mixed.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > Why? This improves readability and clarifies the developer’s intention.

    ```javascript
    // bad
    const foo = a && b < 0 || c > 0 || d + 1 === 0

    // bad
    const bar = a ** b - 5 % d

    // bad
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d
    }

    // bad
    const bar = a + b / c * d

    // good
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0)

    // good
    const bar = a ** b - (5 % d)

    // good
    if (a || (b && c)) {
      return d
    }

    // good
    const bar = a + (b / c) * d
    ```

**[⬆ back to top](#table-of-contents)**

## Blocks

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) Use braces with all multiline blocks. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // bad
    if (test)
      return false

    // good
    if (test) return false

    // good
    if (test) {
      return false
    }

    // bad
    function foo() { return false }

    // good
    function bar() {
      return false
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) If you’re using multiline blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // bad
    if (test) {
      thing1()
      thing2()
    }
    else {
      thing3()
    }

    // good
    if (test) {
      thing1()
      thing2()
    } else {
      thing3()
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // bad
    function foo() {
      if (x) {
        return x
      } else {
        return y
      }
    }

    // bad
    function cats() {
      if (x) {
        return x
      } else if (y) {
        return y
      }
    }

    // bad
    function dogs() {
      if (x) {
        return x
      } else {
        if (y) {
          return y
        }
      }
    }

    // good
    function foo() {
      if (x) {
        return x
      }

      return y
    }

    // good
    function cats() {
      if (x) {
        return x
      }

      if (y) {
        return y
      }
    }

    // good
    function dogs(x) {
      if (x) {
        if (z) {
          return y
        }
      } else {
        return z
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Control Statements

  <a name="control-statements"></a>
  - [17.1](#control-statements) In case your control statement (`if`, `while` etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.

    > Why? Requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1()
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1()
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1()
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1()
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1()
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1()
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1()
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [17.2](#control-statements--value-selection) Don't use selection operators in place of control statements.

    ```javascript
    // bad
    !isRunning && startRunning()

    // good
    if (!isRunning) {
      startRunning()
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) Use `/** ... */` for multiline comments.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

    ```javascript
    // bad
    const active = true  // is current tab

    // good
    // is current tab
    const active = true

    // bad
    function getType() {
      console.log('fetching type...')
      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }

    // good
    function getType() {
      console.log('fetching type...')

      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) Start all comments with a space to make it easier to read. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true

    // good
    // is current tab
    const active = true

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Use `// FIXME:` to annotate problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super()

        // FIXME: shouldn’t use a global here
        total = 0
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) Use `// TODO:` to annotate solutions to problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super()

        // TODO: total should be configurable by an options param
        this.total = 0
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) Use soft tabs (space character) set to 2 spaces. eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙let name
    }

    // bad
    function bar() {
    ∙let name
    }

    // good
    function baz() {
    ∙∙let name
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) Place 1 space before the leading brace. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // bad
    function test(){
      console.log('test')
    }

    // good
    function test() {
      console.log('test')
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    })

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    })
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // bad
    if(isJedi) {
      fight ()
    }

    // good
    if (isJedi) {
      fight()
    }

    // bad
    function fight () {
      console.log ('Swooosh!')
    }

    // good
    function fight() {
      console.log('Swooosh!')
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) Set off operators with spaces. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // bad
    const x=y+5

    // good
    const x = y + 5
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) End files with a single newline character. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6
    ```

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6↵
    ↵
    ```

    ```javascript
    // good
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains) Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount()

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount()

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount()

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led)

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led)

    // good
    const leds = stage.selectAll('.led').data(data)
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement.

    ```javascript
    // bad
    if (foo) {
      return bar
    }
    return baz

    // good
    if (foo) {
      return bar
    }

    return baz

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    }
    return obj

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    }

    return obj

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ]
    return arr

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ]

    return arr
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) Do not pad your blocks with blank lines. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // bad
    function bar() {

      console.log(foo)

    }

    // bad
    if (baz) {

      console.log(qux)
    } else {
      console.log(foo)

    }

    // bad
    class Foo {

      constructor(bar) {
        this.bar = bar
      }
    }

    // good
    function bar() {
      console.log(foo)
    }

    // good
    if (baz) {
      console.log(qux)
    } else {
      console.log(foo)
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a>
  - [19.9](#whitespace--no-multiple-blanks) Do not use multiple blank lines to pad your code. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName


        this.email = email


        this.setAge(birthday)
      }


      setAge(birthday) {
        const today = new Date()


        const age = this.getAge(today, birthday)


        this.age = age
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // good
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName
        this.email = email
        this.setAge(birthday)
      }

      setAge(birthday) {
        const today = new Date()
        const age = getAge(today, birthday)
        this.age = age
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.10](#whitespace--in-parens) Do not add spaces inside parentheses. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // bad
    function bar( foo ) {
      return foo
    }

    // good
    function bar(foo) {
      return foo
    }

    // bad
    if ( foo ) {
      console.log(foo)
    }

    // good
    if (foo) {
      console.log(foo)
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.11](#whitespace--in-brackets) Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ]
    console.log(foo[ 0 ])

    // good
    const foo = [1, 2, 3]
    console.log(foo[0])
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.12](#whitespace--in-braces) Add spaces inside curly braces. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // bad
    const foo = {clark: 'kent'}

    // good
    const foo = { clark: 'kent' }
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.13](#whitespace--max-len) Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Why? This ensures readability and maintainability.

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'))

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'))
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.14](#whitespace--block-spacing) Require consistent spacing inside an open block token and the next token on the same line. This rule also enforces consistent spacing inside a close block token and previous token on the same line. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // bad
    function foo() {return true}
    if (foo) { bar = 0}

    // good
    function foo() { return true }
    if (foo) { bar = 0 }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) Avoid spaces before commas and require a space after commas. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // bad
    var foo = 1,bar = 2
    var arr = [1 , 2]

    // good
    var foo = 1, bar = 2
    var arr = [1, 2]
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) Enforce spacing inside of computed property brackets. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // bad
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // good
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) Avoid spaces between functions and their invocations. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // bad
    func ()

    func
    ()

    // good
    func()
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) Enforce spacing between keys and values in object literal properties. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // bad
    var obj = { foo : 42 }
    var obj2 = { foo:42 }

    // good
    var obj = { foo: 42 }
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) Avoid trailing spaces at the end of lines. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) Avoid multiple empty lines, only allow one newline at the end of files, and avoid a newline at the beginning of files. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad - multiple empty lines
    var x = 1


    var y = 2

    // bad - 2+ newlines at end of file
    var x = 1
    var y = 2


    // bad - 1+ newline(s) at beginning of file

    var x = 1
    var y = 2

    // good
    var x = 1
    var y = 2

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ back to top](#table-of-contents)**

## Commas

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) Leading commas: **Nope.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ]

    // good
    const story = [
      once,
      upon,
      aTime,
    ]

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    }

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    }
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) Additional trailing comma: **Yup.** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don’t have to worry about the [trailing comma problem](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers.

    ```diff
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    }

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    }
    ```

    ```javascript
    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    }

    const heroes = [
      'Batman',
      'Superman'
    ]

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    }

    const heroes = [
      'Batman',
      'Superman',
    ]

    // bad
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // good
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // good (note that a comma must not appear after a "rest" element)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // bad
    createHero(
      firstName,
      lastName,
      inventorOf
    )

    // good
    createHero(
      firstName,
      lastName,
      inventorOf,
    )

    // good (note that a comma must not appear after a "rest" element)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  - **Nope.** But here you can some 

    > Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether or not it should regard that line break as the end of a statement.

    ```javascript
    // bad - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // good
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader'
    })

    // good
    const reaction = "No! That’s impossible!"
    async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }()

    // good
    const foo = () => {
      return 'search your feelings, you know it to be foo'
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) Perform type coercion at the beginning of the statement.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) Strings: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9

    // bad
    const totalScore = new String(this.reviewScore) // typeof totalScore is "object" not "string"

    // bad
    const totalScore = this.reviewScore + '' // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString() // isn’t guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore)
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4'

    // bad
    const val = new Number(inputValue)

    // bad
    const val = +inputValue

    // bad
    const val = inputValue >> 0

    // bad
    const val = parseInt(inputValue)

    // good
    const val = Number(inputValue)

    // good
    const val = parseInt(inputValue, 10)
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](https://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you’re doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](https://es5.github.io/#x4.3.19), but bitshift operations always return a 32-bit integer ([source](https://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 // => 2147483647
    2147483648 >> 0 // => -2147483648
    2147483649 >> 0 // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) Booleans: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0

    // bad
    const hasAge = new Boolean(age)

    // good
    const hasAge = Boolean(age)

    // best
    const hasAge = !!age
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase) Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // bad
    const OBJEcttsssss = {}
    const this_is_my_object = {}
    function c() {}

    // good
    const thisIsMyObject = {}
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase) Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name
    }

    const bad = new user({
      name: 'nope',
    })

    // good
    class User {
      constructor(options) {
        this.name = options.name
      }
    }

    const good = new User({
      name: 'yup',
    })
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) Do not use trailing or leading underscores. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tldr: if you want something to be “private”, it must not be observably present.

    ```javascript
    // bad
    this.__firstName__ = 'Panda'
    this.firstName_ = 'Panda'
    this._firstName = 'Panda'

    // good
    this.firstName = 'Panda'

    // good, in environments where WeakMaps are available
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap()
    firstNames.set(this, 'Panda')
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this) Don’t save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

    ```javascript
    // bad
    function foo() {
      const self = this
      return function () {
        console.log(self)
      }
    }

    // bad
    function foo() {
      const that = this
      return function () {
        console.log(that)
      }
    }

    // good
    function foo() {
      return () => {
        console.log(this)
      }
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) A base filename should exactly match the name of its default export.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox

    // file 2 contents
    export default function fortyTwo() { return 42 }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox' // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo' // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory' // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box' // PascalCase import/export, snake_case filename
    import forty_two from './forty_two' // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory' // snake_case import, camelCase export
    import index from './inside_directory/index' // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index' // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox' // PascalCase export/import/filename
    import fortyTwo from './fortyTwo' // camelCase export/import/filename
    import insideDirectory from './insideDirectory' // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [23.7](#naming--camelCase-default-export) Use camelCase when you export-default a function. Your filename should be identical to your function’s name.

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) Use PascalCase when you export a constructor / class / singleton / function library / bare object.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    }

    export default AirbnbStyleGuide
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) Acronyms and initialisms should always be all uppercased, or all lowercased.

    > Why? Names are for readability, not to appease a computer algorithm.

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer'

    // bad
    const HttpRequests = [
      // ...
    ]

    // good
    import SMSContainer from './containers/SMSContainer'

    // good
    const HTTPRequests = [
      // ...
    ]

    // also good
    const httpRequests = [
      // ...
    ]

    // best
    import TextMessageContainer from './containers/TextMessageContainer'

    // best
    const requests = [
      // ...
    ]
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.

    > Why? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.
    - What about all `const` variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants however.
    - What about exported objects? - Uppercase at the top level of export (e.g. `EXPORTED_OBJECT.key`) and maintain that all nested properties do not change.

    ```javascript
    // bad
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file'

    // bad
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased'

    // bad
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables'

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY'

    // better in most cases
    export const API_KEY = 'SOMEKEY'

    // ---

    // bad - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    }

    // good
    export const MAPPING = {
      key: 'value'
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Accessors

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) Accessor functions for properties are not required.

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) Do not use JavaScript getters/setters as they cause unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, use `getVal()` and `setVal('hello')`.

    ```javascript
    // bad
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // good
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false
    }

    // good
    if (!dragon.hasAge()) {
      return false
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [24.4](#accessors--consistent) It’s okay to create `get()` and `set()` functions, but be consistent.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue'
        this.set('lightsaber', lightsaber)
      }

      set(key, val) {
        this[key] = val
      }

      get(key) {
        return this[key]
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Events

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass an object literal (also known as a "hash") instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id)

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    })
    ```

    prefer:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingID: listing.id })

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    })
    ```

  **[⬆ back to top](#table-of-contents)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [26.1](#jquery--dollar-prefix) Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    const sidebar = $('.sidebar')

    // good
    const $sidebar = $('.sidebar')

    // good
    const $sidebarBtn = $('.sidebar-btn')
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [26.2](#jquery--cache) Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide()

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      })
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar')
      $sidebar.hide()

      // ...

      $sidebar.css({
        'background-color': 'pink',
      })
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [26.3](#jquery--queries) For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [26.4](#jquery--find) Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide()

    // bad
    $('.sidebar').find('ul').hide()

    // good
    $('.sidebar ul').hide()

    // good
    $('.sidebar > ul').hide()

    // good
    $sidebar.find('ul').hide()
    ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript 5 Compatibility

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [27.1](#es5-compat--kangax) Refer to [Kangax](https://twitter.com/kangax/)’s ES5 [compatibility table](https://kangax.github.io/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6+ (ES 2015+) Styles

  <a name="es6-styles"></a><a name="27.1"></a>
  - [28.1](#es6-styles) This is a collection of links to the various ES6+ features.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#classes--constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) Do not use [TC39 proposals](https://github.com/tc39/proposals) that have not reached stage 3.

    > Why? [They are not finalized](https://tc39.github.io/process-document/), and they are subject to change or to be withdrawn entirely. We want to use JavaScript, and proposals are not JavaScript yet.

**[⬆ back to top](#table-of-contents)**

## Standard Library

  The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
  contains utilities that are functionally broken but remain for legacy reasons.

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) Use `Number.isNaN` instead of global `isNaN`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Why? The global `isNaN` coerces non-numbers to numbers, returning true for anything that coerces to NaN.
    > If this behavior is desired, make it explicit.

    ```javascript
    // bad
    isNaN('1.2') // false
    isNaN('1.2.3') // true

    // good
    Number.isNaN('1.2.3') // false
    Number.isNaN(Number('1.2.3')) // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) Use `Number.isFinite` instead of global `isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Why? The global `isFinite` coerces non-numbers to numbers, returning true for anything that coerces to a finite number.
    > If this behavior is desired, make it explicit.

    ```javascript
    // bad
    isFinite('2e3') // true

    // good
    Number.isFinite('2e3') // false
    Number.isFinite(parseInt('2e3', 10)) // true
    ```

**[⬆ back to top](#table-of-contents)**

## Testing

  - **Seriously, we need to do tests!**

**[⬆ back to top](#table-of-contents)**

## Translation

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)

**[⬆ back to top](#table-of-contents)**

# )