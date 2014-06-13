# TheLadders JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript (forked from Airbnb and modified)*


## Table of Contents

  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)

## Types

  - **Primitives**: When you access a primitive type you work directly on its value

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: When you access a complex type you work on a reference to its value

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
      var args = Array.slice.call(arguments);
      ...
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Strings

  - Use double quotes `""` for strings

    ```javascript
    // bad
    var name = 'Immanuel Kant';

    // good
    var name = "Immanuel Kant";

    // bad
    var fullName = 'Immanuel ' + this.lastName;

    // good
    var fullName = "Immanuel " + this.lastName;
    ```

**[⬆ back to top](#table-of-contents)**


## Functions

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log("You want it to be one way... but it's the other way.");
    })();
    ```

  - Never leave space between parameters/arguments and enclosing parentheses.

    ```javascript
    // bad
    function imbibe( drink ) {
      // ...stuff...
    } 

    // bad
    imbibe( "seltzer" );

    // good
    function imbibe(drink) {
      // ...stuff...
    } 

    // good
    imbibe("seltzer");
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad.
  - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log("Nope.");
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log("Yup.");
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ back to top](#table-of-contents)**



## Properties

  - Use dot notation when accessing properties.

    ```javascript
    var plato = {
      athenian: true,
      age: 30
    };

    // bad
    var isAthenian = plato["athenian"];

    // good
    var isAthenian = plato.athenian;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var plato = {
      athenian: true,
      age: 30
    };

    function getProp(prop) {
      return plato[prop];
    }

    var isAthenian = getProp("athenian");
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

    ```javascript
    // bad
    treatise = new Treatise();

    // good
    var treatise = new Treatise();
    ```

  - Use separate `var` declarations for multiple variables and declare each variable on a new line.

    ```javascript
    // bad
    var books = getBooks(),
        peaceful = true,
        library = "Alexandria";

    // good
    var books = getBooks();
    var peaceful = true;
    var library = "Alexandria";
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log("doing stuff..");

      //..other stuff..

      var name = getName();

      if (name === "test") {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name = getName();

      test();
      console.log("doing stuff..");

      //..other stuff..

      if (name === "test") {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Hoisting

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      read(); // logs "Human reason has the peculiar fate in one species of its cognitions..."

      function read() {
        console.log("Human reason has the peculiar fate in one species of its cognitions...");
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ back to top](#table-of-contents)**


## Conditional Expressions & Equality

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `""`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== "") {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - These shortcuts should not be used when checking for the existence of a property on an object.  

    ```javascript
    // bad - checks truthiness of value of the property, not for the existence of the property
    if (someObject.someProperty) {
      // ...stuff...
    }

    // good - when checking if a property exists on an object or in its prototype chain
    if ("someProperty" in someObject) {
      // ...stuff...
    }

    // good - when checking if a property exists on an object instance
    if (someObject.hasOwnProperty("someProperty")) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ back to top](#table-of-contents)**


## Blocks

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Comments

  - Use `/** ... */` for multiline comments. 

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a new line above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log("fetching type...");
      // set the default type to "no type"
      var type = this.type || "no type";

      return type;
    }

    // good
    function getType() {
      console.log("fetching type...");

      // set the default type to "no type"
      var type = this.type || "no type";

      return type;
    }
    ```

  - Prefixing your comments with FIXME, TODO, or HACK helps other developers quickly understand what kind of problem you're pointing out.  These are different than regular comments because they are actionable. FIXME indicates that a problem that still needs a solution.  TODO describes the form of a solution to a problem. HACK indicates a fast, suboptimal solution that should be improved. 

  - Use `// FIXME:` to annotate problems

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - Use soft tabs set to 4 spaces

    ```javascript
    // bad
    function() {
    ∙var name;
    }

    // bad
    function() {
    ∙∙var name;
    }

    // good
    function() {
    ∙∙∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log("test");
    }

    // good
    function test() {
      console.log("test");
    }

    // bad
    falcon.set("attr",{
      age: "1 year",
      breed: "Maltese"
    });

    // good
    falcon.set("attr", {
      age: "1 year",
      breed: "Maltese"
    });
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - Use indentation when making long method chains.

    ```javascript
    // bad
    $("#items").find(".selected").highlight().end().find(".open").updateCount();

    // good
    $("#items")
      .find(".selected")
        .highlight()
        .end()
      .find(".open")
        .updateCount();

    // bad
    var leds = stage.selectAll(".led").data(data).enter().append("svg:svg").class("led", true)
        .attr("width",  (radius + margin) * 2).append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);

    // good
    var leds = stage.selectAll(".led")
        .data(data)
      .enter().append("svg:svg")
        .class("led", true)
        .attr("width",  (radius + margin) * 2)
      .append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var hero = {
        firstName: "Immanuel"
      , lastName: "Kant"
      , birthPlace: "Koenigsberg"
      , masterWork: "The Critique of Pure Reason"
    };

    // good
    var hero = {
      firstName: "Immanuel",
      lastName: "Kant",
      birthPlace: "Koenigsberg",
      masterWork: "The Critique of Pure Reason"
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = "Ada"
      return name
    })()

    // good
    (function() {
      var name = "Ada";
      return name;
    })();

    // good
    ;(function() {
      var name = "Ada";
      return name;
    })();
    ```

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + "";

    // good
    var totalScore = "" + this.reviewScore;

    // bad
    var totalScore = "" + this.reviewScore + " total score";

    // good
    var totalScore = this.reviewScore + " total score";
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = "4";

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: "Ada Lovelace"
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: "Ada Lovelace"
    });
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: "nope"
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: "yup"
    });
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions.  See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

**[⬆ back to top](#table-of-contents)**


## Accessors

  - Accessor functions on Backbone models do not use the getVal() pattern.

    ```javascript
    // bad
    bowie.getAlbums();

    // good
    bowie.albums();
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!bowie.albums()) {
      return false;
    }

    // good
    if (!bowie.hasAlbums()) {
      return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Writer() {
      console.log("new writer");
    }

    // bad
    Writer.prototype = {
      procrastinate: function() {
        console.log("procrastinating");
      },

      dream: function() {
        console.log("dreaming");
      }
    };

    // good
    Writer.prototype.procrastinate = function() {
      console.log("procrastinating");
    };

    Writer.prototype.dream = function() {
      console.log("dreaming");
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Writer.prototype.resolve = function() {
      this.madeResolution = true;
      return true;
    };

    Writer.prototype.imagineMovingTo = function(destination) {
      this.destination = destination;
    };

    var roommate = new Writer();
    roommate.resolve(); // => true
    roommate.imagineMovingTo("Paris") // => undefined

    // good
    Writer.prototype.resolve = function() {
      this.madeResolution = true;
      return this;
    };

    Writer.prototype.imagineMovingTo = function(destination) {
      this.destination = destination;
      return this;
    };

    var roommate = new Writer();

    roommate.resolve()
      .imagineMovingTo("Paris");
    ```

  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Writer(options) {
      options || (options = {});
      this.name = options.name || "no name";
    }

    Writer.prototype.getName = function getName() {
      return this.name;
    };

    Writer.prototype.toString = function toString() {
      return "Writer - " + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  **[⬆ back to top](#table-of-contents)**


## Modules

  - Always declare `"use strict";` at the top of the module.

**[⬆ back to top](#table-of-contents)**


## jQuery

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = ".sidebar");

    // good
    var $sidebar = ".sidebar");
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      ".sidebar").hide();

      // ...stuff...

      ".sidebar").css({
        "background-color": "pink"
      });
    }

    // good
    function setSidebar() {
      var $sidebar = ".sidebar");
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        "background-color": "pink"
      });
    }
    ```

  - Scope jQuery object queries to Backbone view using this.$el.find()

    ```javascript
    // bad
    $("ul", ".sidebar").hide();

    // bad
    $(".sidebar").find("ul").hide();

    // good
    this.$el.find(".sidebar ul").hide();

    // good
    this.$el.find(".sidebar > ul").hide();

    // good
    $sidebar.find("ul").hide();
    ```

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

**[⬆ back to top](#table-of-contents)**


## Testing

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Performance

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ back to top](#table-of-contents)**


## Resources


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**[⬆ back to top](#table-of-contents)**

## In the Wild

  This is a list of organizations that are using variations of the Airbnb style guide.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

# };
