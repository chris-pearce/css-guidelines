# CSS Guidelines




## Contents

- [Terminology And Conventions Used In These Guidelines](#terminology-and-conventions-used-in-these-guidelines)
- [General Principles](#general-principles)
    - [Acknowledgements And Credit](#acknowledgements-and-credit)
- [Syntax And Formatting](#syntax-and-formatting)
    - [CSS Ruleset](#css-ruleset)
    - [Sass Specifics](#sass-specifics)
    - [Declaration Order](#declaration-order)
    - [More On Ordering](#more-on-ordering)
    - [80 Characters Wide](#80-characters-wide)
    - [Meaningful Whitespace](#meaningful-whitespace)
- [Commenting](#commenting)
    - [DocBlock-*esque*](#docblock-esque)
    - [Partial Heading](#partial-heading)
    - [Partial Intro](#partial-intro)
    - [Inline Comments](#inline-comments)
    - [Number Labelling](#number-labelling)
    - [Component-extension Pointers](#component-extension-pointers)
    - [BEM Modifier Markers](#bem-modifier-markers)
    - [Section Comment](#section-comment)
- [Naming Conventions](#naming-conventions)
    - [BEM-like Naming](#bem-like-naming)
    - [Hyphen-delimited](#hyphen-delimited)
    - [Namespace](#namespace)
        - [What We Namespace](#what-we-namespace)
        - [The Namespaces](#the-namespaces)
            - [JS Hooks](#js-hooks)
            - [State Hooks](#state-hooks)
            - [Server-side Hooks](#server-side-hooks)
            - [QA Hooks](#qa-hooks)
            - [Tracking Hooks](#tracking-hooks)
    - [Settings](#settings)
        - [Global Settings](#global-settings)
        - [Local Settings](#local-settings)
    - [Mixins And Functions](#mixins-and-functions)
    - [Sizes And Sides](#sizes-and-sides)
    - [Spelling](#spelling)
- [Tooling](#tooling)
    - [Text Editor Configuration](#text-editor-configuration)
        - [Sublime Text](#sublime-text)
    - [Linting](#linting)
    - [EditorConfig](#editorconfig)
    - [Sublime Text Snippets](#sublime-text-snippets)
- [Writing And Architecturing CSS](#writing-and-architecturing-css)
    - [Further Reading](#further-reading)




## Terminology And Conventions Used In These Guidelines

- Sass variable = setting
- [Scally CSS framework](https://github.com/chris-pearce/scally) = Scally
- An ellipsis (`…`) = a placeholder for code used in a code example block
- Written for a team




## General Principles

> "Part of being a good steward to a successful project is realizing that writing code for yourself is a Bad Idea™. If thousands of people are using your code, then write your code for maximum clarity, not your personal preference of how to get clever within the spec." — [Idan Gazit](http://gazit.me/)

In working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to—among other things—

- keep stylesheets maintainable;
- keep code transparent, sane, consistent, and readable;
- keep stylesheets scalable.

No matter the document, we must always try and keep a common formatting. This
means:

- [consistent syntax](#syntax-and-formatting);
- [consistent and meaningful commenting](#commenting);
- and [consistent naming](#naming-conventions).

All our CSS is written in [Sass](http://sass-lang.com/). Sass provides a lot of incredibly powerful features, however, we don’t want to become a platform to showcase Sass’ capabilities; we should strive to keep the Sass we write as simple and CSS-like as possible.

### Acknowledgements And Credit

We’ve borrowed (shamelessly) a lot of our guidelines from [Harry Roberts](http://csswizardry.com/) excellent [CSS Guidelines](http://cssguidelin.es/), and some other sources of inspiration:

- [Sass Guidelines](http://sass-guidelin.es/)
- [Idiomatic CSS](https://github.com/necolas/idiomatic-css)




## Syntax And Formatting

Starting right at the top, we want

- four (4) space indents, no tabs;
- [80 character wide](#80-characters-wide) columns;
- multi-line CSS;
- [meaningful use of whitespace](#meaningful-whitespace).

**N.B.** our [Tooling](#tooling) will automate and enforce most of our syntax and formatting rules.

### CSS Ruleset

For your reference here is an anatomy of a CSS ruleset:

```
[selector] {
    [property]: [value];
    [<- Declaration ->]
}
```

Our chosen format for how CSS rulesets should be written

- each selector on its own new line;
- a space before the opening brace (`{`);
- the opening brace (`{`) on the same line as the last selector;
- a space after the colon (`:`);
- each declaration on its own new line;
- each declaration indented by four (4) spaces;
- a trailing semi-colon (`;`) at the end of all declarations;
- the closing brace (`}`) on its own new line;
- long, comma-separated property values—such as collections of gradients or shadows—arranged across multiple new lines, making sure all values are indented at the same level as the first;

###### :heavy_multiplication_x: NOT GOOD

```scss
// Not having each selector on its own line
.selector-1, .selector-2
// Not having the opening brace (`{`) on the same line as the last selector
{
    // Not having each declaration on its own line
    background-color: $color-brand; background-image: linear-gradient($color-white, $color-grey-mercury), linear-gradient($color-black, $color-grey-alabaster);
    // Not using new lines for the comma-separated property values and not
    // indenting all the values at the same level as the first
    box-shadow: 1px 1px 1px $color-black, 2px 2px 1px 1px $color-grey-mercury inset;
    // Not using a space after the colon (`:`)
    color:$color-text-base;
  // Not indenting a declaration with four (4) spaces
  display: block;
    // Not using a trailing semi-colon (`;`) at the end of the declaration
    // and not having the closing brace (`}`) on its own new line
    padding: rem($spacing-base) }
```

###### :heavy_check_mark: GOOD

```scss
.selector-1,
.selector-2 {
    background-color: $color-brand;
    background-image: linear-gradient($color-white, $color-grey-mercury),
                      linear-gradient($color-black, $color-grey-alabaster);
    box-shadow: 1px 1px 1px $color-black,
                2px 2px 1px 1px $color-grey-mercury inset;
    color: $color-text-base;
    display: block;
    padding: rem($spacing-base);
}
```

Adding to the above, we also want to pay attention to

- use lowercase and preferably the shorthand version for all hexadecimal units—if you wish to use the longhand version then this is fine;
- use shorthands for properties and property values where it makes sense; where it doesn’t make sense is using a shorthand property that makes you declare zero-values, here it is better to be explicit even if it means more lines of CSS;
- use single quotes for strings, `url()` values, etc;
- wrap attribute selector values in double quotes;
- include a space after each comma in comma-separated values;
- parentheses should not be padded with spaces;
- when a decimal mark is needed always include the zero;
- use double colons (`::`) for pseudo elements;
- use relative units—pretty much everything is `rem` based and outside of font sizing `em`s may be used for spacing within a component for scaling based on font size, but this should be checked to see what the current approach is as we want consistency with this;
- use the `px` unit for fixed-sized things—this is rare nowadays as we’re building responsive layouts—and for the following CSS properties:
  - `border-radius`
  - `border`
  - `box-shadow`
  - `text-shadow`

###### :heavy_multiplication_x: NOT GOOD

```scss
.selector {
    // Not using single quotes around the `url()` value
    background-image: url(/path/to/image);
    // Not using lowercase for the hexidecimal unit
    color: #EEEEEE;
    // Not using spaces after each comma in the `rgba` value and padding the
    // parentheses with spaces
    color: rgba( 0,0,0,0.8 );
    // Not including a zero before the decimal mark
    font-size: .98rem;
    // Not using shorthand for the property value
    padding: rem(12) rem(12) rem(12) rem(12);
    // Declaring zero-values due to shorthand property being used and not removing the unit from a zero-value
    margin: 0rem rem(6);

    // Not using the double colon format
    &:before {
        // Not using shorthand for the property
        background: $color-black;
        // Not using single quotes around the `content` string
        content: '✌';
    }

    // Not using double quotes around the attribute selector value
    &[type=text] {
        …
    }
}
```

###### :heavy_check_mark: GOOD

```scss
.selector-1 {
    background-image: url('/path/to/image');
    color: #eee;
    color: rgba(0, 0, 0, 0.8);
    font-size: 0.98rem;
    padding: rem(12);
    margin-left: rem(6);
    margin-right: rem(6);

    &::before {
        background-color: $color-black;
        content: '✌';
    }

    &[type="text"] {
        …
    }
}
```

To expand on this rule: *use shorthands for properties and property values where it makes sense*, treating your properties this way makes for more maintainable and robust CSS.

A good example of this is using the shorthand `background-color` property instead of the longhand `background` property when you only need to declare a colour. Using the shorthand `background-color` property means we now don’t have to worry about potentially having to override all of the properties that come bundled with the longhand `background` property, which are:

```scss
background-image: initial;
background-position-x: initial;
background-position-y: initial;
background-size: initial;
background-repeat-x: initial;
background-repeat-y: initial;
background-attachment: initial;
background-origin: initial;
background-clip: initial;
background-color: transparent;
```

*Phew!*

### Sass Specifics

Syntax and formatting rules specifically for Sass code

- only use the parent selector reference (`&`) for these use cases
    - appending it to pseudo classes;
    - appending it to pseudo elements;
    - appending it to [State hooks](#state-hooks) in order to chain it to the selector it’s referencing;
    - referencing itself when combined with any of the above to avoid duplicating CSS properties;
- only use silent placeholder selectors with the `@extend` directive, however, we'd rather avoid `@extend` as much as possible;
- avoid selector nesting, if you have to nest then limit it to one level deep;
- when adding a unit to a number stored in a setting, you have to multiply the number by 1 unit;
- hexadecimal units should not exist outside of global and local partial settings i.e. all colours need to be stored in a setting using a meaningful name;
- always use colour settings for `rgb` values;
- avoid writing parentheses for argument-less mixins;
- all Sass functions and mixins to use the [SassDoc](http://sassdoc.com/) documentation guidelines;
- for conditional statements:
    - no parentheses unless they are necessary;
    - always an empty new line before `@if`;
    - `@else` statements on the same line as previous closing brace (`}`);
    - always an empty new line after the last closing brace (`}`) unless the next line is a closing brace (`}`);

###### :heavy_multiplication_x: NOT GOOD

```scss
.selector {
    // Extending a class selector
    @extend .selector;
    // Including parentheses for an argument-less mixin
    @include h-text-truncate();
    // Not using colour settings
    background-color: rgba(0, 0, 0, 0.5);
    color: #eee;
    // Not multiplying the unit to a number stored in a setting
    padding: $padding-setting + rem;

    // Using the parent selector (`&`) to chain to the selector it’s
    // referencing when it is not a State hook
    &.selector--modifier {
        …
    }

    // Using the parent selector (`&`) to target a class that is applied at a
    // higher level in the DOM
    .parent-selector & {
        …
    }

    // Nesting selectors more than one level deep
    .nested__selector .nested__selector {
        …
    }
}

// An incorrectly formatted conditional statement
@if ($support-legacy == true) {
    …
}
@else {
    …
}

// Not using SassDoc documentation guidelines
@function strip-unit($number) {
    @if type-of($number) == "number" and not unitless($number) {
        @return $number / ($number * 0 + 1);
    }
    @return $number;
}
```

###### :heavy_check_mark: GOOD

```scss
.selector {
    @extend %selector;
    @include h-text-truncate;
    background-color: rgba($color-black, 0.5);
    color: $color-text-base;
    padding: $padding-setting * 1rem;
}

.selector--modifier {
    …
}

.parent-selector .selector {
    …
}

.nested__selector {
    …
}

@if $support-legacy {
    …
} @else {
    …
}

/// Remove a unit from a number.
///
/// @author Chris Pearce
///
/// @access private
///
/// @param {Number [unit]} $number — Number to remove unit from
///
/// @returns {Number}
///
/// @todo Add @exception rules, see: https://gist.github.com/terkel/4373420
///
/// @example scss - Usage
///     strip-unit(24px)
///     strip-unit(2.3em)

@function strip-unit($number) {
    @if type-of($number) == "number" and not unitless($number) {
        @return $number / ($number * 0 + 1);
    }
    @return $number;
}

// Correct use of the parent selector reference (`&`)
.selector {
    …
    
    &,
    &:hover,
    &:focus {
        color: $color-text-base;
    }
    
    &:hover,
    &:focus {
        …
    }

    &::before {
        …
    }

    &.is-active {
        …
    }
}
```

### Declaration Order

Properties should be ordered alphabetically, that’s it.

###### :heavy_multiplication_x: NOT GOOD

```scss
.selector {
    display: inline-block;
    padding-right: rem(10);
    padding-left: rem(10);
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 10;
}
```

###### :heavy_check_mark: GOOD

```scss
.selector {
    bottom: 0;
    display: inline-block;
    left: 0;
    padding-left: rem(10);
    padding-right: rem(10);
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
}
```

### More On Ordering

Because we use Sass we can have more than just declarations between the opening and closing braces (`{ }`) of a ruleset therefore we want to follow a specific order, which is

1. extend calls (`@extend`);
2. mixin calls (`@include`) *with no* `@content`;
3. declarations;
4. pseudo classes—combining [State hooks](#state-hooks) here is fine;
5. pseudo elements;
5. mixin calls (`@include`) *with* `@content`—mainly being media queries;
6. nested selectors, limit to one level deep or best to avoid in most cases;

###### :heavy_multiplication_x: NOT GOOD

```scss
.selector {
    // Declarations should not come first and should all be grouped together
    bottom: 0;
    display: inline-block;
    left: 0;
    position: absolute;
    right: 0;
    // Mixin calls should come after extend calls and not mixed in with
    // declarations
    @include hidpi-bg-img("path/to/image/image.png", 32px);
    top: 0;
    z-index: 10;
    // Extend calls should come first
    @extend %some-silent-placeholder-selector;

    // Pseudo elements should not come before pseudo classes
    &::before {
        …
    }

    &:hover,
    &:focus,
    &.is-active {
        …
    }

    // Nested selector should come last
    .nested-selector {
        …
    }

    @media (min-width: bp(lap)) {
        …
    }
}
```

###### :heavy_check_mark: GOOD

```scss
.selector {
    @extend %some-silent-placeholder-selector;
    @include hidpi-bg-img("path/to/image/image.png", 32px);
    bottom: 0;
    display: inline-block;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;

    &:hover,
    &:focus,
    &.is-active {
        …
    }

    &::before {
        …
    }

    @media (min-width: bp(lap)) {
        …
    }

    .nested-selector {
        …
    }
}
```

### 80 Characters Wide

Where possible, limit CSS files’ width to 80 characters. Reasons for this include

- the ability to have multiple files open side by side;
- viewing CSS on sites like GitHub, or in terminal windows;
- providing a comfortable line length for comments.

There will be unavoidable exceptions to this rule—such as URLs, or gradient syntax—which shouldn’t be worried about.

### Meaningful Whitespace

Whitespace is free, use it liberally and judiciously as it greatly improves the readability of our CSS.

We want our rulesets, the [discrete sections within our rulesets](#more-on-ordering) and the discrete sections within our partials, to *always* have at least one (1) empty line between them—two (2) empty lines is also fine but never more than two (2). One (1) empty line should *always* be between setting declarations, partial import directives, and a [DocBlock-*esque*](#docblock-esque) style comment and its subject.

Four (4) empty lines are needed between entirely new sections (see [Section Comment](#section-comment)) however this will hardly ever be needed as we use highly concentrated partials—the most common application for this is within a global stylesheet.

###### :heavy_multiplication_x: NOT GOOD

```scss
/* ============================================================================
   COMPONENTS -> SEARCH INPUT
   ========================================================================= */
/**
 * A component for the most common type of search input which has deep rounded
 * corners, shadows, and a background image of a magnifying glass icon
 * positioned to the left or right side. Visit: /subscribers/ to see an
 * example.
 * @markup
   <form action="" class="c-search-input [modifier]">
       <label for="[id.value]" class="c-search-input__label  h-hide-visually">Label text</label>
       <input type="search" id="[value]" class="c-search-input__input">
       <span class="c-search-input__icon"></span>
  </form>
 */

/**
 * Settings.
 */

$c-search-input-padding-top: 5;
$c-search-input-padding-bottom: $c-search-input-padding-top + 1;

.c-search-input {
    position: relative;
}




/**
 * The input.
 */
.c-search-input__input {
    border: 1px solid $color-grey-alto-4;
    box-shadow: $input-box-shadow;
    color: $color-grey-tundora;
    padding: rem($c-search-input-padding-top);
    padding-bottom: rem($c-search-input-padding-bottom);
    transition: $input-transition;
    &:focus {
        border-color: $color-blue-pigeon-post;
        box-shadow: $input-box-shadow-focus;
        // Can remove this as we're providing a good custom outline
        outline: none;
    }
}
/**
 * The magnify icon.
 */

.c-search-input__icon {
  background: url('/img/icons/icon-small/search.png') no-repeat;
  height: $c-search-input-icon-size;
  left: $c-search-input-icon-position-left;
  width: $c-search-input-icon-size;
}
/**
 * Modifier: when icon is positioned on the right side.
 */
.c-search-input--icon-right .c-search-input__icon {
    left: auto;
    right: $c-search-input-icon-position-left;
}
```

###### :heavy_check_mark: GOOD

```scss
/* ============================================================================
   COMPONENTS -> SEARCH INPUT
   ========================================================================= */


/**
 * A component for the most common type of search input which has deep rounded
 * corners, shadows, and a background image of a magnifying glass icon
 * positioned to the left or right side. Visit: /subscribers/ to see an
 * example.
 *
 * @markup
   <form action="" class="c-search-input [modifier]">
       <label for="[id.value]" class="c-search-input__label  h-hide-visually">Label text</label>
       <input type="search" id="[value]" class="c-search-input__input">
       <span class="c-search-input__icon"></span>
  </form>
 */


/**
 * Settings.
 */

$c-search-input-padding-top: 5;

$c-search-input-padding-bottom: $c-search-input-padding-top + 1;


.c-search-input {
    position: relative;
}


/**
 * The input.
 */

.c-search-input__input {
    border: 1px solid $color-grey-alto-4;
    box-shadow: $input-box-shadow;
    color: $color-grey-tundora;
    padding: rem($c-search-input-padding-top);
    padding-bottom: rem($c-search-input-padding-bottom);
    transition: $input-transition;

    &:focus {
        border-color: $color-blue-pigeon-post;
        box-shadow: $input-box-shadow-focus;
        // Can remove this as we're providing a good custom outline
        outline: none;
    }
}


/**
 * The magnify icon.
 */

.c-search-input__icon {
  background: url('/img/icons/icon-small/search.png') no-repeat;
  height: $c-search-input-icon-size;
  left: $c-search-input-icon-position-left;
  width: $c-search-input-icon-size;
}


/**
 * Modifier: when icon is positioned on the right side.
 */

.c-search-input--icon-right .c-search-input__icon {
    left: auto;
    right: $c-search-input-icon-position-left;
}
```




## Commenting

Well commented code is *extremely* important and we really need to be heavily commenting our CSS. Take time to describe components, how they work, their limitations, and the way they are constructed. We don’t want to leave others in the team guessing as to the purpose of uncommon or non-obvious code.

We want to be using a comment style that is simple and consistent.

Starting right at the top, we want

- to place comments on a new line above their subject;
- to keep line-length to a sensible maximum: [80 columns](#80-characters-wide);
- to make liberal use of comments to break CSS code into discrete sections;
- to use ‘sentence case’ comments and consistent text indentation.

### DocBlock-*esque*

The default style of comment we use is a [DocBlock](http://en.wikipedia.org/wiki/PHPDoc#DocBlock)*-esque* style comment. This type of comment begins with `/**` and ends with `*/` and has an `*` at the beginning of every line except when referencing code—the absence of the `*` is so that the code can easily be copied.

###### :heavy_check_mark: GOOD

```scss
/**
 * A component for the most common type of search input which has deep rounded
 * corners, shadows, and a background image of a magnifying glass icon
 * positioned to the left or right side. Visit: /subscribers/ to see an
 * example.
 *
 * @markup
   <form action="" class="c-search-input [modifier]">
       <label for="[id.value]" class="c-search-input__label  u-hide-visually">Label text</label>
       <input type="search" id="[value]" class="c-search-input__input">
       <span class="c-search-input__icon"></span>
  </form>
 */
```

We should be commenting each discrete piece of CSS in a partial and it should start with a DocBlock*-esque* style comment. One (1) or two (2) empty lines are always used between each discrete piece and one (1) empty line used between the comment and its subject.

###### :heavy_check_mark: GOOD

```scss
/**
 * The underlay which is the wrapper of the modal dialog which uses Flexbox
 * to position the modal dialog smack in the middle of the viewport,
 * includes a fall-back for non-supporting Flexbox browsers.
 */

.c-modal-dialog-underlay {
    …
}

/**
 * Fall-back for non-supporting Flexbox browsers, using the Modernizr
 * feature detection technique via the `:not` CSS selector to get around
 * any lag of the Modernizr JS loading.
 */

html:not(.flexbox) .c-modal-dialog-underlay {
    …
}
```

### Partial Heading

Every partial needs a partial heading type comment at the very top of the file.

###### :heavy_check_mark: GOOD

```scss
/* ============================================================================
   CORE -> MIXINS -> CONVERT PX TO EM/REM
   ========================================================================= */
// [empty line]
// [empty line]
```

Breaking this down we have

- uppercase for text;
- a breadcrumb pattern showing where the partial belongs in the CSS architecture, the last part being the same name as the partial filename;
- two (2) empty lines to come after.

### Partial Intro

A partial intro type comment follows a [Partial Heading](#partial-heading) type comment, think of this like a books Prologue/Preface/Introduction.

It should be structured like this

- a description—be as detailed as you can here;
- any attention grabbing comments prefix with ‘N.B.’;
- mandatory sections prefixed with `@`
    - `@markup`: if applicable the markup that the CSS applies too—this is typically for components—*this should always come last*—when we have a living style guide this will no longer be required;
    - `@demo`: if applicable any URL(s) to demonstration(s);
    - `@credit`: if applicable any URL(s) to credit where any ideas came from;
- non-mandatory sections prefixed with `@`
    - `@author` and `@created`: the *original* author of the file and the date the file was created;
    - `@todo`: any outstanding tasks;
    - `@consideration`: any things to consider;
- One (1) empty line between each of the above sections and two (2) empty lines coming after the intro.

###### :heavy_check_mark: GOOD

```scss
/**
 * A generic drop down helper powered by some JavaScript which toggles a
 * class e.g. `is-visible` on the drop down trigger (the button that makes the
 * drop down visible and invisible) and the target (the actual drop down).
 * This class will be used to make the drop down target visible when the
 * trigger is selected. There is also a version for showing the drop down via
 * the `:hover` pseudo class which is turned off for touch devices.
 *
 * N.B. this helper is dependant on the Align helper.
 *
 * @author
 * Chris Pearce
 *
 * @created
 * 12/08/2015
 *
 * @credit
 * http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code
 *
 * @markup
   <div class="h-drop-down [modifier(s)]">
     <!-- The trigger -->
     <button class="h-drop-down__trigger">[text node]</button>
     <!-- The target -->
     <div class="h-drop-down__target">[child elements]</div>
   </div>
 */
// [empty line]
// [empty line]
```

In the `@markup` section we want to be indicating that the component has [BEM](#bem-like-naming) *Modifier(s)*. We do this by including a `[modifier]` placeholder, like in the example above, if there are more than one then simply add `(s)` to the end of the “modifier” word, like so: `[modifier(s)]`. Also we want to use sensible placeholders between elements, so if an element only has text as its child then use `[text node]`, if an element features other elements that are not part of the component then use `[child elements]`, like in the example above.

### Inline Comments

Sass allows us to use less verbose CSS comments prefixed with two forward slashes.

This type of comment can be used for inline type comments, typically when you want to comment above a declaration within a ruleset or when you want to use a *sub* type comment under a [DocBlock*-esque*](#docblock-esque) type comment.

These comments should always be above their subject with a space coming after the two forward slashes.

###### :heavy_check_mark: GOOD

```scss
/**
 * Settings.
 */

// Colours
$c-pagination-background-color:   #000;

$c-pagination-foreground-color:   #333;

// Padding
$c-pagination-padding-left:       $spacing-base;

$c-pagination-padding-right:      $spacing-half;


h1 {
  font-size: rem($font-size-heading-1);
  // This is needed to turn off the top margin set in normalize.css
  margin-top: 0;
  // This is needed to fix a stupid bug in IE 9 ಠ╭╮ಠ
  width: 100%;
}
```

### Number Labelling

A number labelling type comment is used when [Inline Comments](#inline-comments) would need to be repeated.

**N.B.** this is the only time when comments—the numbers—don’t need to be on a new line above their subject.

###### :heavy_multiplication_x: NOT GOOD

```scss
img {
  // Make responsive
  max-width: 100%;
  // Make responsive
  height: auto;
  // So that `alt` text is visually offset if images don't load
  font-style: italic;
}
```

###### :heavy_check_mark: GOOD

```scss
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

### Component-extension Pointers

Sometimes you may need to apply some styles to a component within the context of another component when these two components exist in their own individual partials. For example, you may have a Button component that needs some unique padding applied when used in the context of a Search component.

*Make sure that this type of treatment is needed and that its not just a [BEM](#bem-like-naming) Modifier of your component.*

When this occurs we want to document this relationship across partials using a simple Component–extension Pointer type comment, and we want to include the comment in the partial where the component styles are being extended. So using the example above the comment will live in the Search component partial.

###### :heavy_check_mark: GOOD

```scss
/**
 * Extend `.c-button` in Components -> Button.
 */

.c-search .c-button {
    …
}
```

### BEM Modifier Markers

Whenever you declare a [BEM](#bem-like-naming) *Modifier* include a [DocBlock*-esque*](#docblock-esque) type comment.

It should be structured like this

- the word ‘Modifier’ plus a colon (`:`), followed by an empty space and the name of the *Modifier*;
- if applicable an explanation of what the *Modifier* is achieving, one (1) empty line comes before this.

###### :heavy_check_mark: GOOD

```scss
/**
 * Modifier: striped.
 *
 * Applies a background colour to every odd row.
 */

.c-table--striped {
  tr:nth-of-type(odd) td {
    background-color: $c-table-striped-background-cell-colour;
  }
}
```

### Section Comment

When we have really long stylesheets we need a comment that stands out more than a [DocBlock*-esque*](#docblock-esque) type comment.

This type of comment is rare due to us working in highly concentrated partials—the most common application for this is within a global stylesheet. Four (4) empty lines come between each section and one (1) empty line between the comment and its subject.

###### :heavy_check_mark: GOOD

```scss
/* Some section
   ========================================================================= */

    …
// [empty line]
// [empty line]
// [empty line]
// [empty line]
/* Some section
   ========================================================================= */
```




## Naming Conventions

We always want to ensure that all of our classes and settings are meaningfully named and adhering to our set conventions and to not worry about the length of our class names as gzip will compress well written code incredibly well.

Starting right at the top, we use

- lowercase;
- [BEM-like naming](#bem-like-naming) for most classes;
- [hyphen-delimited](#hyphen-delimited) for everything else;
- [namespaces](#namespace) for almost everything.

We don’t want

- CamelCase;
- underscores with the exception of being used in BEM *Element* selectors and the *Hack* [namespace](#namespace);
- `id`s, **never**.

### BEM-like Naming

We apply BEM as described in the [BEM-like naming](http://cssguidelin.es/#bem-like-naming) section of [Harry Roberts](http://csswizardry.com/) excellent [CSS Guidelines](http://cssguidelin.es/). For further reading check out this article: [MindBEMding – getting your head ’round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/), again by Harry Roberts.

So rather than rehashing everything about BEM please read the above resources.

### Hyphen-delimited

We use hyphen-delimited for:

- Settings
- Mixins
- Functions
- Animation names
- Filenames
- [State hooks](#state-hooks)
- [JS hooks](#js-hooks)
- [Server-side hooks](#server-side-hooks)
- [QA hooks](#qa-hooks)
- [Tracking hooks](#tracking-hooks)

### Namespace

Mostly everything in our CSS and in [Scally](https://github.com/chris-pearce/scally) is namespaced so that our CSS is as transparent and self-documenting as possible.

#### What We Namespace

- Classes
- Settings
- Mixins
- Functions
- Filenames

They are all prefixed with a letter followed by a hyphen.

#### The Namespaces

Taken from [Scally](https://github.com/chris-pearce/scally):

- `l-`: [Layout Modules](https://github.com/chris-pearce/scally/tree/master/layout)

    *Examples:*
    - `.l-grid`
    - `$l-grid-gutter`
    - `_l-grid.scss`

- `c-`: [Components](https://github.com/chris-pearce/scally/tree/master/components)

    *Examples:*
    - `.c-button`
    - `$c-button-foreground-color`
    - `_c-button.scss`

Taken from [Harry Roberts](http://csswizardry.com/) article [More Transparent UI Code with Namespaces](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)—*visit the linked sections to learn more*:

- `h-`: [Helpers](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/#component-namespaces-u-), **N.B.**, we use the name **Helpers** instead of **Utility**.

    *Examples:*
    - `.h-text-small`
    - `$h-text-small-size`
    - `_h-text-sizes.scss`

- `t-`: [Themes](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/#theme-namespaces-t-)

    *Examples:*
    - `.t-dark`
    - `$t-dark-background-color`
    - `_t-dark.scss`

- `s-`: [Scoped](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/#scope-namespaces-s-)

    *Examples:*
    - `.s-subtext-alignment`
    - `$s-subtext-alignment-bottom-spacing`
    - `_s-subtext-alignment.scss`

And the rest:

- `js-`: [JS hooks](#js-hooks)
- `is-`/`has-`: [State hooks](#state-hooks)
- `ss-`: [Server-side hooks](#server-side-hooks)
- `qa-`: [QA hooks](#qa-hooks)
- `track-`: [Tracking hooks](#tracking-hooks)

##### JS Hooks

Rather than rehashing everything about JS hooks let’s instead defer to the [JavaScript Hooks](http://cssguidelin.es/#javascript-hooks) section in [Harry Roberts](http://csswizardry.com/) excellent [CSS Guidelines](http://cssguidelin.es/).

JS hooks—and [Server-side](#server-side-hooks), [QA](#qa-hooks), and [Tracking hooks](#tracking-hooks)—should always be scoped to the component it is part of to avoid clashing with other hooks of the same category, and to keep them highly readable—not much different to how we structure our classes for styling.

###### :heavy_multiplication_x: NOT GOOD

- `js-trigger`
- `js-target`
- `js-list`
- `js-close-button`

###### :heavy_check_mark: GOOD

- `js-drop-down-menu-trigger`
- `js-drop-down-menu-target`
- `js-drop-down-menu-list`
- `js-drop-down-menu-close-button`

##### State Hooks

Certain styles are *state* based, for example

- a navigation link may be in an active state;
- an accordion section may be in an expanded state;
- a drop down menu may be in a visible state;
- a third party widget may be in a state where it has finished loading.

Styles that are needed to be applied to an elements state are applied by a class using either the `is-` or `has-` namespace. For example, we could use the following state hooks for the four examples listed above:

- `is-active`
- `is-expanded`
- `is-visible`
- `has-loaded`

JavaScript or server-side logic is typically used to apply state hooks. A classic example is using server-side logic to apply a state hook to highlight the active link in a primary navigation component, the server-side code would append the `.is-active` state hook to the relevant `a` element.

To keep things consistent and to reduce the cognitive load of having to come up with names for state hooks we should try to stick with a set list of common state hook names, something like:

- `is-active`
- `has-loaded`
- `is-loading`
- `is-visible`
- `is-disabled`
- `is-expanded`
- `is-collapsed`

We can safely use a set list of state hook names because they are always scoped to the selector they are being applied too i.e. we never have to worry about state hook styles clashing with other state hook styles.

We achieve this by chaining the state hook to the selector it needs to be applied too, for example:

```scss
.nav-main__link {
  color: red;

  &.is-active {
    color: blue;
  }
}
```

Which compiles too:

```css
.nav-main__link {
  color: red;
}

.nav-main__link.is-active {
  color: blue;
}
```

##### Server-side Hooks

Server-side hooks are classes namespaced with `ss-` that are used to hook into the DOM for the purpose of referencing DOM elements in server-side code, for example: C#.

You the front end developer will typically not be applying these hooks.

##### QA Hooks

QA hooks are classes namespaced with `qa-` that are used to hook into the DOM for the purpose of applying automated UI tests.

You the front end developer will typically not be applying these hooks.

##### Tracking Hooks

Tracking hooks are classes namespaced with `track-` that are used to hook into the DOM for the purpose of collecting data for analytics.

You the front end developer will typically not be applying these hooks.

#### Settings

##### Global Settings

Global settings—settings defined in the global settings partial (`_settings.scss`)—are namespaced with `g-` ("g" stands for "global") then followed by the name of the group they are a part of as the first word then after that any sub-groups.

###### :heavy_multiplication_x: NOT GOOD

```scss
$color-error: #dc322f;

$color-success: #859900;

$color-warning: #b58900;

$color-information: #268bd2;
```

###### :heavy_check_mark: GOOD

```scss
$g-color-state-error: #dc322f;

$g-color-state-success: #859900;

$g-color-state-warning: #b58900;

$g-color-state-information: #268bd2;
```

##### Local Settings

Local settings—settings defined in your partials outside of the global settings partial (`_settings.scss`) —should start with their relevant [namespace](#namespace), followed by the name of what it belongs too, for example, a component, a helper, etc., followed by what the setting is targeting. The main goal is to make your settings as readable as possible.

###### :heavy_multiplication_x: NOT GOOD

```scss
/**
 * Settings.
 */

// Colours
$c-drop-down-menu-background: $g-color-white;

$c-drop-down-menu-color-outline: rgba($g-color-black, 0.19);

$c-link-color: $g-color-grey-rolling-stone;

// Widths and heights
$c-drop-down-width: 194px;

$drop-down-menu-width-narrow: 170px;

$arrow-width: 13;

$arrow-height: 7;

// Padding
$c-drop-down-menu-padding-all: 7;

$c-drop-down-menu-link-padding-left-padding-right: 15px;

$c-drop-down-menu-link-padding-top-padding-bottom: 8px;
```

###### :heavy_check_mark: GOOD

```scss
/**
 * Settings.
 */

// Colours
$c-drop-down-menu-background-color: $g-color-white;

$c-drop-down-menu-outline-color: rgba($g=color-black, 0.19);

$c-drop-down-menu-link-color: $g-color-grey-rolling-stone;

// Widths and heights
$c-drop-down-menu-width: 194px;

$c-drop-down-menu-width-narrow: 170px;

$c-drop-down-menu-arrow-width: 13;

$c-drop-down-menu-arrow-height: 7;

// Padding
$c-drop-down-menu-padding: 7;

$c-drop-down-menu-link-padding-sides: 15px;

$c-drop-down-menu-link-padding-ends: 8px;
```

### Mixins And Functions

There are other things we can name in Sass like functions and mixins however it’s rare we will have to create any as [Scally](https://github.com/chris-pearce/scally/) should provide everything we need in that regard. If we do need to create new generic mixins and functions then we should strive to get them into Scally.

If any non-generic mixins and functions are needed i.e. a mixin may be helpful to only a specific component, then these are to reside in the component partial and treated as *local* and adequately [namespaced](#namespace).

### Sizes And Sides

We use a naming convention for sizes and for targeting an element’s top and bottom, left and right sides, which we use to keep things consistent and to reduce the cognitive load in coming up with our own names.

The main naming convention for sizes:

- Tiny
- Small
- Large
- Huge

And occasionally for something really small or big we use:

- Micro
- Mega

The base size is simply *Base*.

###### :heavy_check_mark: GOOD

```scss
.c-pagination--tiny {
  padding: rem($c-pagination-padding-tiny);
}

.c-pagination--small {
  padding: rem($c-pagination-padding-small);
}

.c-pagination--large {
  padding: rem($c-pagination-padding-large);
}

.c-pagination--huge {
  padding: rem($c-pagination-padding-huge);
}
```

A secondary naming convention for sizes is the `x` pattern where `x` is short for "extra".

###### :heavy_check_mark: GOOD

```scss
.c-pagination--small {
  padding: rem($c-pagination-padding-small);
}

.c-pagination--x-small {
  padding: rem($c-pagination-padding-x-small);
}

.c-pagination--xx-small {
  padding: rem($c-pagination-padding-xx-small);
}

.c-pagination--xxx-small {
  padding: rem($c-pagination-padding-xxx-small);
}
```

The `x` naming convention is better suited when you have *many* sizes as it scales more sensibly. This pattern is rarely needed and it is probably a good time to reassess your component before implementing.

Another naming convention we use is *Ends* and *Sides* which are used to target either an element’s top and bottom (ends) or its left and right (sides).

###### :heavy_check_mark: GOOD

```scss
.c-pagination--padding-only-ends {
  padding-bottom: rem($c-pagination-padding);
  padding-top: rem($c-pagination-padding);
}

.c-pagination--padding-only-sides {
  padding-left: rem($c-pagination-padding);
  padding-right: rem($c-pagination-padding);
}
```

### Containers

The names we give to classes that act as containers should be *container* e.g. `.l-container`, we shouldn’t use *wrap*.

And for inner containers we should simply use *inner* e.g. `.c-card__inner`.

### Spelling

We want to stick with British English spelling not American with the exception of the word **colour** but only in code, not required anywhere else, e.g. in comments.

###### :heavy_multiplication_x: NOT GOOD

```scss
.c-panel__customize {
    …
}

// Brand color
$g-colour-brand: #444;
```

###### :heavy_check_mark: GOOD

```scss
.c-panel__customise {
    …
}

// Brand colour
$g-color-brand: #444;
```




## Tooling

Making it easier to apply our guidelines and to prevent wasting time picking apart nonconforming code in code review (PR’s).

### Linting

Coming soon.

### EditorConfig

We use an EditorConfig (`.editorconfig`) file which enforces our indenting rules.

Please install the relevant plugin for your text editor: <http://editorconfig.org/#download>.

You can view the file [here](https://git.campmon.com/Freshview/createsend.com/blob/master/.editorconfig) and there is a [wiki page](https://campmon.com/wiki/display/DEV/EditorConfig) explaining more about it.

### Text Editor Configuration

You should configure your text editor to adhere to as much of our guidelines as possible.

At the bare minimum configure your editor to *show invisibles* or to automatically remove end-of-line whitespace.

#### Sublime Text

If you use Sublime Text editor then install [this package](https://github.com/SublimeText/TrailingSpaces) and use these settings in your `Settings — User` file which can be found here: `Sublime Text -> Preferences -> Settings — User`:

```json
"tab_size": 4,
"translate_tabs_to_spaces": true,
"trim_automatic_white_space": true,
"trim_trailing_white_space_on_save": true,
"word_wrap": true,
"wrap_width": 80
```

Also install [this package](https://sublime.wbond.net/packages/DocBlockr) to make writing DocBlock style comments super easy.

### Sublime Text Snippets

Coming soon.




## Writing And Architecturing CSS

All of the above is about how we structure and form our CSS; they are very quantifiable rules.

For how to deal with our attitude and approach to writing and architecturing CSS I am yet to document this—it is coming soon so watch this space.

In the interim we can link to relevant parts of [Harry Roberts](http://csswizardry.com/) excellent [CSS Guidelines](http://cssguidelin.es/):

- [CSS Selectors](http://cssguidelin.es/#css-selectors)
- [Specificity](http://cssguidelin.es/#specificity)
- [Architectural Principles](http://cssguidelin.es/#architectural-principles)

### Further Reading

- [SOLID CSS](http://blog.millermedeiros.com/solid-css/)
- [One Module or Two](http://snook.ca/archives/html_and_css/one-module-or-two)
- [Our (CSS) Best Practices Are Killing US](http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/)
- [Keep your CSS selectors short](http://csswizardry.com/2012/05/keep-your-css-selectors-short/)
- [The single responsibility principle applied to CSS](http://csswizardry.com/2012/04/the-single-responsibility-principle-applied-to-css/)
- [Principles for writing good CSS](http://blog.kaelig.fr/post/38377421139/principles-for-writing-good-css)
- [DOM Hooks—Smarter ways to target elements in the DOM](http://presentation.chris-pearce.me/DOM-hooks/)
