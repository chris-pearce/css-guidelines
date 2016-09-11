# HEAD

## 2.0.0 (September 11, 2016)

* General typo and code example fixes.
* Removed all smart quotes.
* Remove all references to the [Scally CSS Framework](https://github.com/chris-pearce/scally) as I want these guidelines to not be tied to any particular framework/library.
* Changed the empty line rule that comes _after_: **Commenting -> Partial Heading** to be one (1) empty line not two (2).
* Removed the non-mandatory sections: `@author` and `@created` from: **Commenting -> Partial Heading** as I don't see any value in this convention anymore. It's better to use source control e.g. [Git](https://git-scm.com/) for this information.
* Removed the **Commenting -> BEM Modifier Markers** section as I don't see any value in this convention anymore. It's obvious a selector is a BEM modifier from the two (2) hyphens in its name e.g. `.c-card--compact`—certainly if a BEM modifier isn't obvious from the code alone then go ahead and add some comments but no need to use the "Modifier" marker convention.
* Removed the **Naming Conventions -> Mixins And Functions** section as it was mostly referring to the [Scally CSS Framework](https://github.com/chris-pearce/scally).
* Removed the **Naming Conventions -> Spelling** section so it's scalable to any team worldwide, basically this should be decided upon by the team.
* A few updates to the **Tooling** section.
* PR's addressed:
  * https://github.com/chris-pearce/css-guidelines/pull/17

## 1.0.0 (January 7, 2015)
