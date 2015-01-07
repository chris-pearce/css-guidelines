# CSS guidelines

In working on large, long running projects with dozens of developers, it is
important that we all work in a unified way in order to, among other things:

- Keep stylesheets maintainable.
- Keep code transparent and readable.
- Keep stylesheets scalable.

No matter the document, we must always try and keep a common formatting. This
means consistent commenting, consistent syntax and consistent naming.

There are a variety of techniques we must employ in order to satisfy these
goals.

## Contents

- [Whitespace](#whitespace)
    - [80 characters wide](#80-characters-wide)
    - [Text editor configuration](#text-editor-configuration)
        - [Sublime Text](#sublime-text)
- [Formatting](#formatting)
    - [Formatting rules](#formatting-rules)
    - [Declaration order](#declaration-order)
    - [Exceptions and slight deviations](#exceptions-and-slight-deviations)
    - [Preprocessors: additional format considerations](#preprocessors-additional-format-considerations)
    - [Linting](#linting)
- [Naming conventions](#naming-conventions)
    - [BEM](#bem)
    - [Namespacing](#namespacing)
        - [CSS frameworks](#css-frameworks)
        - [State hooks](#state-hooks)
        - [JavaScript hooks](#javascript-hooks)
        - [Test and Tracking hooks](#test-and-tracking-hooks)
            - [Order of the above hooks](#order-of-the-above-hooks)
- [Comments](#comments)
  - [DocBlock-esque](#docblock-esque)
  - [Main title](#main-title)
  - [Intro block](#intro-block)
  - [Section titles](#section-titles)
  - [Sub titles](#sub-titles)
  - [Preprocessor comments](#preprocessor-comments)
  - [Component-Extension pointers](#component-extension-pointers)
  - [Number labelling](#number-labelling)
  - [BEM modifier comments](#bem-modifier-comments)
  - [Settings comments](#settings-comments)
  - [TODO comments](#todo-comments)
  - [Non-JavaScript comments](#non-javascript-comments)
  - [`!important` comments](#important-comments)
  - [Other](#other)
- [Architecture Overview](#architecture-overview)
  - [Breakdown](#breakdown)
- [Writing CSS](#writing-css)
- [Acknowledgements and Further Reading](#acknowledgements-and-further-reading)

---





## Whitespace

Here are our rules for managing whitespace in Sass files:

- Use soft indents (spaces).
- Use 2 characters for the indentation level.
- **Never** mix spaces and tabs for indentation.
- Wrap at 80 characters wide.

### 80 characters wide

Where possible, limit CSS files’ width to 80 characters. Reasons for this include:

- The ability to have multiple files open side by side.
- Viewing CSS on sites like GitHub, or in terminal windows.
- Providing a comfortable line length for comments.

There will be unavoidable exceptions to this rule—such as URLs, or gradient syntax—which shouldn’t be worried about.

### Text editor configuration

Configure your text editor to adhere to the above. At the bare minimum configure your editor to "show invisibles" or to automatically remove end-of-line whitespace.

#### Sublime Text

If you use Sublime Text editor (and you should) then install [this package](https://github.com/SublimeText/TrailingSpaces) and use these settings in your **Settings - User** file which can be found here: **Sublime Text -> Preferences -> Settings User**:

```
"tab_size": 2,
"translate_tabs_to_spaces": true,
"trim_automatic_white_space": true,
"trim_trailing_white_space_on_save": true,
"word_wrap": true,
"wrap_width": 80
```

This is optional:

```
"rulers":
[
  80
],
```




## Formatting

Our chosen CSS code formatting rules ensures that CSS code is:

- Easy to read.
- Easy to clearly comment.
- Minimises the chance of accidentally introducing errors.
- Results in useful diffs and blames.

For reference here is an anatomy of a rule set:

```
[selector] {
  [property]: [value];
  [<- Declaration ->]
}
```

### Formatting rules

- Class names to use BEM notation, see [Naming Conventions -> BEM](#bem), where BEM isn't used then hyphen delimited class names are to be used.
- Use one discrete selector per line in multi-selector rule sets, e.g.

  ```
  .error,
  .success {
    ...
  }
  ```
  *not*
  ```
  .error, .success {
    ...
  }
  ```
- Include a single space before the opening brace of a rule set, e.g.

  ```
  .error,
  .success {
    ...
  }
  ```
  *not*
  ```
  .error,
  .success{
    ...
  }
  ```
- Place the closing brace of a rule set in the same column as the first character of the rule set, e.g.

  ```
  .error,
  .success {
    ...
  }
  ```
  *not*
  ```
  .error,
  .success {
    ...}
  ```
- Properties within rule sets should each reside on their own line except for single-line declaration rule sets, e.g.

  ```
  p {
    margin: 0;
    padding: 0;
  }
  ```
  *not*
  ```
  p {margin: 0; padding: 0;}
  ```
  *single-line declaration rule sets allowed, e.g.*
  ```
  p {margin: 0;}
  ```
- Use one level of indentation for each declaration, e.g.

  ```
  .error {
    border: 0;
    margin: 0;
  }
  ```
  *not*
  ```
  .error {
  border: 0;
  margin: 0;
  }
  ```
- Separate each rule set by a blank line, e.g.

  ```
  .error {
    border: 0;
    margin: 0;
  }

  .success {
    border: 0;
    margin: 0;
  }
  ```
  *not*
  ```
  .error {
    border: 0;
    margin: 0;
  }
  .success {
    border: 0;
    margin: 0;
  }
  ```
- Include a single space after the colon of a property, e.g. `margin: 0;` *not* `margin:0;`.
- Use lowercase and shorthand hex values, e.g. `#aaa` *not* `#aaaaaa`.
- Always use the shortest shorthand form possible for properties that support it, e.g. `margin: 1px;` *not* `margin: 1px 1px 1px 1px;`.
- Always use double quotes, specifically for:
    - String literals e.g. `content: "";`.
    - `url()` e.g. `background: url("img/logo.png");`.
    - Attribute values in selectors e.g. `input[type="checkbox"]`.
- Where allowed, avoid specifying units for zero-values, e.g. `margin: 0;` *not* `margin: 0px;`.
- Commas in lists should be followed by a space, e.g. `color: rgba(0, 0, 0, 0.1);` *not* `color: rgba(0,0,0,0.1);`.
- Include a space before `!important` keyword, e.g. `padding: 10px !important;` *not* `padding: 10px!important;`.
- Property values; `@extend`, `@include`, and `@import` directives; and variable declarations should always end with a semicolon, e.g. `color: #fff;` *not* `color: #fff`.
- Parentheses should not be padded with spaces, e.g. `@include box-shadow(0 2px 2px rgba(0, 0, 0, .2));` *not* `@include box-shadow( 0 2px 2px rgba( 0, 0, 0, .2 ) );`.
- When a decimal mark is needed always include the zero, e.g. `0.25rem` *not* `.25rem`.
- Don't write trailing zeros for numeric values with a decimal point, e.g. `margin: 0.5em;` *not* `margin: 0.500em;`.
- URLs should not contain protocols or domain names, e.g. `background: url('assets/image.png');` *not* `background: url('https://example.com/assets/image.png');`.
- Indent rule sets to mirror the DOM, e.g.

  ```
  .widget,
  .widget-2 {
    padding: 1em;
    border: 1px solid #bada55;
    background-color: #c0ffee;
  }

    .widget__heading {
      font-size: 1.5rem;
      line-height: 1;
      font-weight: bold;
      color: #bada55;
    }
  ```
  Here we can see that `.widget__heading` must be a child of `.widget` as we have indented the `.widget__heading` rule set one   level deeper than `.widget`, as it is in the DOM. This is useful information to developers that can now be gleaned just by a   glance at the indentation of our rule sets.

### Declaration order

Declarations should be ordered in relevance, **NOT** alphabetically.

Example:

```
.selector {
  /* Positioning */
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  /* Display & Box Model */
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 10px solid #333;
  margin: 10px;

  /* Other */
  background-color: #000;
  color: #fff;
  font-family: sans-serif;
  font-size: 1em;
  text-align: right;
}
```

*Comments do not need to be used to label each group of declarations like above*.

### Exceptions and slight deviations

Long, comma-separated property values—such as collections of gradients or shadows can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. Each value after the first should be indented so that it starts at the same level as the first value e.g.

```
.selector {
background-image: linear-gradient(#fff, #ccc),
                  linear-gradient(#f3c, #4ec);
box-shadow: 1px 1px 1px #000,
            2px 2px 1px 1px #ccc inset;
}
```

### Preprocessors: additional format considerations

All our CSS is written with the Sass preprocessor so our conventions should be extended to accommodate the particularities of Sass.

- Limit nesting to 1 level deep. Reassess any nesting more than 2 levels deep. This prevents overly-specific CSS selectors (see [Specificity](http://cssguidelin.es/#specificity)).
- Always use placeholder selectors in `@extend`. Using a class selector with the `@extend` statement statement usually results in more generated CSS than when using a placeholder selector.
- Do not use parent selector references (`&`) when they would otherwise be unnecessary, e.g.

  ```
  .foo {
    > .bar {}
  }
  ```
  *not*
  ```
  .foo {
    & > .bar {}
  }
  ```
- Rule sets should be ordered as follows:
  1. `@extend` declarations.
  2. `@include` declarations without inner `@content`.
  3. Properties.
  4. `@include` declarations with inner `@content`.
  5. Nested rule sets.

  [See](https://github.com/causes/scss-lint/blob/master/lib/scss_lint/linter/README.md#declarationorder)

### Linting

To ensure these rules are met you should use the [`.scss-lint`](https://github.com/causes/scss-lint#usage), [see](https://github.com/chris-pearce/scally#linting). And intergrate linting into some sort of automated testing e.g. setup up a task in a Rakefile.

**N.B.** because we indent rule sets to mirror the DOM the linting tool will flag this as a warning e.g.

    utilities/_u-flex-embed.scss:69 [W] Indentation: Line should be indented 0 spaces, but was indented 2 spaces

You can safely ignore this.

If you use Sublime Text editor then install [this package](https://github.com/attenzione/SublimeLinter-scss-lint) which provides an interface to the `.scss-lint` tool.



## Naming conventions

Always ensure classes are sensibly named; keep them as short as possible but as long as necessary. Ensure any utilities are very vaguely named (e.g. `.u-display-block`, `.u-divider`) to allow for greater reuse. Don’t worry about the amount or length of classes in your markup; gzip will compress well written code incredibly well.

**ID's cannot be used as style hooks**, [see](http://cssguidelin.es/#ids-in-css).

[Further reading](http://cssguidelin.es/#naming-conventions).

### BEM

We use the BEM (Block, Element, Modifier) naming convention. When BEM is not used e.g. [JavaScript hooks](#javascript-hooks) then we use hyphen delimited classes e.g. `.foo-bar`, not `.foo_bar` or `.fooBar`.

BEM is a methodology for naming and classifying CSS selectors in a way to make them a lot more strict, transparent and informative.

The naming convention follows this pattern:

```
.block{}
.block__element{}
.block--modifier{}
```

- `.block` represents the higher level of an abstraction or component.
- `.block__element` represents a descendent of `.block` that helps form `.block` as a whole.
- `.block--modifier` represents a different state or version of `.block`.

[Further reading](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/).

### Namespacing

#### State hooks

Any type of style that is **state** based e.g. a navigation link may be in an active state, an accordion section may be in an expanded state, should be namespaced with `.is-`, e.g. `.is-active`, `.is-expanded`.

State styles 90% of the time have a JavaScript/server-side dependency e.g. to highlight the active link in some primary navigation we will append `.is-active` to the relevant `a` element via server-side code, to style a component differently based on whether it only has one list item we would write some JS to check for this and if it's true append `.is-only-item` to the parent component element.

To keep things consistent we use a set list of common state class names:

- `is-active`
- `is-loaded`
- `is-loading`
- `is-visible`
- `is-only-item`
- `is-only-items-x` - where *x* is the amount of items
- `is-disabled`
- `is-expanded`
- `is-collapsed`

Where possible use these names rather than creating your own and we can always use these names as state styles are always scoped to the thing they're being applied too.

**State hooks do not use BEM.**

#### JavaScript hooks

***Never use a CSS styling class as a JavaScript hook***. Attaching JS behaviour to a styling class means that we can never have one without the other.

If you need to bind to some markup use a JS specific CSS class which is namepsaced with `.js-`. So the naming convention would be something like: `.js-[component]-[what-it's-doing/UI-it's-affecting]`, e.g. `.js-accordion-header`, `.js-drag-and-drop`, `.js-carousel-items`.

This means that we can attach both JS and CSS to classes in our markup but there will never be any troublesome overlap.

A common practice is to use data-* attributes as JS hooks, but this is incorrect. data-* attributes, as per the [spec](http://www.w3.org/TR/2011/WD-html5-20110525/elements.html#embedding-custom-non-visible-data-with-the-data-attributes), are used to store custom data private to the page or application (emphasis mine). data-* attributes are designed to store data, not be bound to.

**JS hooks do not use BEM.**

#### Test and Tracking hooks

In addition to JS hooks we have **Test** and **Tracking** hooks.

Test hooks are used for back-end tests e.g. testing some Ruby logic.

Tracking hooks are used for tracking elements for analytics.

They're simply classes namespaced like so: `.wf-test-` (test) / `.wf-track` (tracking) and follow the same naming convention as JS Hooks e.g. `.wf-test-[component]-[UI-it's-affecting]` / `.wf-track-[component]-[UI-it's-affecting]`.

**Test and Tracking hooks hooks do not use BEM.**

##### Order of the above hooks

The order of the above hooks, within the `class` attribute, should be:

1. Style hooks:
    1. Component hook (no namespace)
    2. Layout hook (`.l-`)
    3. Utility hook (`u-`)
2. JS hooks (`.js-`)
3. Test hooks (`.wf-test-`)
4. Tracking hooks (`.wf-track-`)

Example:

```
<div class="pagination l-container u-text-align-center js-paginate test-pagination ga-pagination">
```





## Comments

We should document and comment our code as much as we possibly can, what may seem or feel transparent and self explanatory to you may not be to another dev. Write a chunk of code then write about it.

[Further reading](http://cssguidelin.es/#commenting).

### DocBlock-esque

For most of our comments we use a [DocBlock](http://en.wikipedia.org/wiki/PHPDoc#DocBlock)*-esque* style comment e.g.

```
/**
 * Settings.
 */
```

A DocBlock comment begins with `/**` and has an `*` at the beginning of every line. Any line within a DocBlock that doesn't begin with a `*` will be ignored, this technique is primarily used when referencing code so that the code can easily be copied to the clipboard e.g.

```
/**
 * N.B. This utility requires that you remove the whitespace between `li`s
 * especially with the Spacing modifiers. One way to remove whitespace is by
 * inserting HTML comments between the opening and closing `li`s e.g.
 *
   <ul class="u-list-inline">
     <li>Lorem</li><!--
     --><li>Aliquam</li><!--
     --><li>Vestibulum</li>
   </ul>
 */
 ```

If you use Sublime Text editor then install [this package](https://sublime.wbond.net/packages/DocBlockr) which makes writing DocBlock style comments super easy.

### Main title

Every `.scss` file should always have a main title comment at the very top of the file and formatted like this:

```
/* ============================================================================
   @COMPONENTS -> PAGINATON
   ========================================================================= */
```

The first part, prefixed with an `@` symbol, represents the location of the file within the CSS architecture (see [Architecture Overview](#architecture-overview)). The second part represents what the `.scss` file is actually styling which in most cases will be a component. It's similar to a breadcrumb pattern, so for a more deeply nested file we'll have this:

```
/* ============================================================================
   @CORE -> MIXINS -> CONVERT PX TO EM/REM
   ========================================================================= */
```

Each main title is prefixed with an `@` symbol to allow us to perform more targeted searches (e.g. `grep`, etc.) and is always in uppercase.

**2 blank lines should always come after a main title comment.**

### Intro block

After the [main title](#main-title) comes the intro block comments which consist of:

- A general description that provides an overview of what the file is doing exactly, be as detailed as you can.
- Any dependencies on other files which should always be prefixed with `N.B.`.
- Sub sections prefixed with `@`:
    - `@todo` - any outstanding tasks.
    - `@demo` - a URL to a demonstration page.
    - `@markup` - markup example(s).
    - `@credit` - URL(s) to credit where the idea came from.
    - `@consideration` - any things to consider.
    - `@example` - Sass code example(s).

Examples:

```
    /**
     * Place any two elements side-by-side, typically for an image- and text-like
     * content.
     *
     * N.B. this utility is dependant on the following utilities:
     *
     * - Clear fix.
     * - New block formatting context.
     *
     * @credit
     * http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code
     */
```

```
    /**
     * Apply percentage based width classes with the option to apply at all the
     * main breakpoints. All the classes are the same as the Width settings found
     * in: Core -> Settings -> Widths, so `$one-whole` would be `.u-one-whole`.
     *
     * N.B. by default we're applying to the Lap breakpoint.
     */
```

```
    /**
     * Creates blocky list items out of a `ul` or `ol` with the option to add a
     * keyline separator between the list items.
     *
     * @consideration
     * The spacer utilities could replace this?
     *
     * @credit
     * https://github.com/inuitcss/objects.list-block/blob/master/_objects.list-block.scss
     */
```

```
    /**
     * A generic drop down utility powered by some JavaScript which toggles a
     * class e.g. `is-visible` on the drop down trigger (the button that makes the
     * drop down visible and invisible) and the target (the actual drop down).
     * This class will be used to make the drop down target visible when the
     * trigger is selected. There is also a version for showing the drop down via
     * the `:hover` pseudo class which is turned off for touch devices.
     *
     * @markup
       <div class="u-drop-down">
         <!-- The trigger -->
         <button class="u-drop-down__trigger"> ... </button>
         <!-- The target -->
         <div class="u-drop-down__target"> ... </div>
       </div>
     */
```

```
    /**
     * Generate percentage classes with the option to apply at different
     * breakpoints e.g. `.u-one-whole` / `.u-lap-one-whole`. The percentage classes
     * are based off the widths defined here: Core -> Settings -> Widths.
     *
     * N.B. the application for this mixin is quite unique so it's only used in a
     * few places in the framework.
     *
     * @example
       @include generate-percentage-classes-at-breakpoints(
         $breakpoints-for-grid-push-classes,
         $scally-type: "l",
         $class-name: "push",
         $css-property: "left"
       );
     */
```

**2 blank lines should always come after an intro block comment.**

### Section titles

Section title comments are used to break up large `.scss` files into their own logical sections so they're easier to read, however because we use compact Sass partials so extensively they're rarely needed. A section title is formatted like this:

```
/* Section title here
   ========================================================================= */
```

**4 blank lines should be between these section titles.**

### Sub titles

Each major chunk of CSS in a file should begin with a sub title comment e.g.

```
/**
 * Grid container.
 */
```

A full stop (period) should end the sub title comment.

**2 blank lines should come before and 1 blank line should come after these sub title comments.**

### Preprocessor comments

Sass allows us to use less verbose CSS comments prefixed with two forward slashes, like so: `// Some comment` e.g.

```
h1, .h1 {
  @include font-size($font-size-heading-1);

  // This is needed to turn off the top margin set in normalize.css
  margin-top: 0;
}
```

This type of comment style is used for when you need to explain what some CSS is doing, when it isn't obvious from the code itself that is, and when none of the aformentioned comment styles are appropriate.

A space should always come after the two forward slashes and when it spans multiple lines then each line should always start with the two forward slashes e.g.

```
// When specifying one breakpoint with an explicit limit, it needs to be
// casted into a list of lists, otherwise the mixin assumes there is a
// breakpoint called 'max'
```

**No blank lines should come after these type of comments.**

### Number labelling

When you want to easily comment on a number of declarations in a rule set then use number labelling e.g.

```
/**
 * Images.
 *
 * 1. Make responsive.
 * 2. So that `alt` text is visually offset if images don't load.
 */

img {
  max-width: 100%; // [1]
  height: auto; // [1]
  font-style: italic; // [2]
}
```

This should only be used when standard inline comments won't work i.e. in the above example it wouldn't be as readable if you had two inline comments for the no.1 declarations e.g.

```
img {
  // Make responsive
  max-width: 100%;
  // Make responsive
  height: auto;
  // So that `alt` text is visually offset if images don't load
  font-style: italic;
}
```

### Component-Extension pointers

When working across multiple partials and in an OOCSS manner, you will often find that rule sets from components that can work in conjunction with each other are not always in the same file. In this scenario we need to include a comment to highlight this so that other developers are aware of the relationship between the files.

This is the format to use:

```
/**
 * Component-Extension pointer.
 *
 * These rules extend `.btn` in Components Buttons.
 */
```

### BEM modifier comments

Whenever you're using BEM and you declare a modifier (see [Naming conventions -> BEM](#bem)) you need to include a comment and if required provide a brief explanation as to what it's doing, e.g.

```
/**
 * Modifier: striped.
 *
 * Applies a background colour to every odd row.
 */

.u-table--striped {
  tbody tr:nth-of-type(odd) td {background-color: $u-table-striped-background-cell-colour;}
}
```

```
/**
 * Modifier: border.
 */

.u-table--border {
  th,
  td {@include to-rem(border, $u-table-border-thickness $u-table-border-style $u-table-border-colour);}
}
```

### Settings comments

Any settings (Sass variables) defined in your Sass partials should always be placed before all other CSS in the `.scss` file and include a comment e.g.

```
/**
 * Settings.
 */

// Apply at these breakpoints
$u-drop-down-breakpoints: $default-breakpoints !default;
```

### TODO comments

To leave reminders of outstanding work for yourself or other developers prefix the comment with `TODO:` e.g.

```
// TODO: `.btn-main-compact-icon` needs more work
```

*If you use Sublime Text editor it will highlight these for you.*

### Non-JavaScript comments

Whenever you write some styles for non-JavaScript users always include a comment like this:

```
// Non-JS users
.no-js .selector {position: static;}
```

### `!important` comments

Whenever you write some styles that needs to use the `!important` keyword always include a comment like this:

```
// N.B. it is okay to use `!important` here as we're doing it pre-emptively i.e. you know you will always want the rule it's applied too to take precedence.
```

It acts as a nice reminder to other devs that you aren't using it incorrectly i.e. reactively.

### Other

1. When commenting on specific declarations (i.e. lines) in a rule set always place the comment on a new line above the declaration (one exception to this is when using [Number labelling](#number-labelling)) e.g.

    ```
    // Left double quotation mark
    content: "\201C";
    content: open-quote;
    ```

    ***not***

    ```
    content: "\201C"; // Left double quotation mark
    content: open-quote;
    ```
2. Use backticks when referencing code e.g.

    ```
    // We need to change the `box-shadow` at this breakpoint
    ```
3. Prefix important attention grabbing comments with **N.B.** e.g.

    ```
    // N.B: have to increase the specificity by chaining the base `.btn` class to make it easy to override non-simple modifiers
    ```
4. When you end a Sass `@if` statement then follow the closing bracket with `// end if`.




---

## Writing CSS

All of the above is about how we structure and form our CSS; they are very quantifiable rules. For how to deal with our attitude and approach to writing CSS we'll link to another set of guidelines:

- [CSS Selectors](http://cssguidelin.es/#css-selectors)
- [Specificity](http://cssguidelin.es/#specificity)
- [Architectural Principles](http://cssguidelin.es/#architectural-principles)





---

## Acknowledgements and Further Reading

- [CSS guidelines](http://cssguidelin.es)
- [Principles of writing consistent, idiomatic CSS](https://github.com/necolas/idiomatic-css)
- [OOCSS code standards](https://github.com/stubbornella/oocss-code-standards)
- [inuitcss](https://github.com/csswizardry/inuit.css)
- [SUIT CSS](http://suitcss.github.io/)
- [SMACCS](http://smacss.com/)
- [SOLID CSS](http://blog.millermedeiros.com/solid-css/)
- [One Module or Two](http://snook.ca/archives/html_and_css/one-module-or-two)
- [Our (CSS) Best Practices Are Killing US](http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/)
- [Keep your CSS selectors short](http://csswizardry.com/2012/05/keep-your-css-selectors-short/)
- [The single responsibility principle applied to CSS](http://csswizardry.com/2012/05/keep-your-css-selectors-short/)
- [Principles for writing good CSS](http://blog.kaelig.fr/post/38377421139/principles-for-writing-good-css)