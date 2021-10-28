<!--#region:intro-->
# Regular Expression Conditionals for ECMAScript

<!--#endregion:intro-->

<!--#region:status-->
## Status

**Stage:** 0  
**Champion:** Ron Buckton ([@rbuckton](https://github.com/rbuckton))  

_For detailed status of this proposal see [TODO](#todo), below._  
<!--#endregion:status-->

<!--#region:authors-->
## Authors

* Ron Buckton ([@rbuckton](https://github.com/rbuckton))  
<!--#endregion:authors-->

<!--#region:motivations-->
# Motivations

From https://github.com/rbuckton/proposal-regexp-features:
> ECMAScript regular expressions have slowly improved over the years to adopt new 
> functionality commonly present in other languages, including:
> 
> - Unicode Support
> - Named Capture Groups
> - Match Indices
> 
> However, a large majority of other languages and libraries have a common set of 
> features that ECMAScript regular expressions currently lack.
> Some of these features improve performance in degenerative cases such as backtracking 
> in complex patterns. Some of these features introduce
> new tools for developers to write more powerful regular expressions.
> 
> As a result, ECMAScript developers wishing to leverage these capabilities are left with 
> few options, relying on native bindings to third-party
> libraries in environments such as NodeJS, or server-side evaluation.
> 
> There are numerous applications for extending the ECMAScript regular expression feature 
> set, including:
> 
> - In-browser support for TextMate grammars for web based editors/IDEs.
> - Improved performance for expressions through possessive quantifiers and backtracking 
>   control.
> - RegExp-based parsers that can support balanced brackets/parens.
> - Documenting complex patterns *in the pattern itself*.
> - Improved readability through the use of multi-line patterns and insignificant 
>   whitespace.

> NOTE: See https://github.com/rbuckton/proposal-regexp-features for an overview of
> how this proposal fits into other possible future features for Regular Expressions.

Regular Expression Conditionals, as implemented in multiple other engines, provide increased
flexibility with regards to complex matching in alternatives.

<!--#endregion:motivations-->

<!--#region:prior-art-->
# Prior Art 

* [Perl](https://rbuckton.github.io/regexp-features/engines/perl.html#feature-conditional-expressions)  
* [PCRE](https://rbuckton.github.io/regexp-features/engines/pcre.html#feature-conditional-expressions)  
* [Boost.Regex](https://rbuckton.github.io/regexp-features/engines/boost.regex.html#feature-conditional-expressions)  
* [.NET](https://rbuckton.github.io/regexp-features/engines/dotnet.html#feature-conditional-expressions)  
* [Oniguruma](https://rbuckton.github.io/regexp-features/engines/oniguruma.html#feature-conditional-expressions)  
* [Glib/GRegex](https://rbuckton.github.io/regexp-features/engines/glib-gregex.html#feature-conditional-expressions)  

See https://rbuckton.github.io/regexp-features/features/conditional-expressions.html for additional information.
<!--#endregion:prior-art-->

<!--#region:syntax-->
# Syntax

A Conditional Expression checks a condition and evaluates its first alternative if the condition is true; otherwise, it evaluates its second alternative.

- `(?(condition)yes-pattern|no-pattern)` &mdash; Matches yes-pattern if condition is true; otherwise, matches no-pattern.
- `(?(condition)yes-pattern)` &mdash; Matches yes-pattern if condition is true; otherwise, matches the empty string.

> NOTE: This has no conflicts with existing syntax, as ECMAScript currently produces an error for this syntax in both `u` and non-`u` modes.

### Conditions

The following conditions are proposed:

- `(?=test-pattern)` &mdash; Evaluates to **true** if a positive lookahead for test-pattern matches; Otherwise, evaluates to **false**.
- `(?<=test-pattern)` &mdash; Evaluates to **true** if a positive lookbehind for test-pattern matches; Otherwise, evaluates to **false**.
- `(?!test-pattern)` &mdash; Evaluates to **true** if a negative lookahead for test-pattern matches; Otherwise, evaluates to **false**.
- `(?<!test-pattern)` &mdash; Evaluates to **true** if a negative lookbehind for test-pattern matches; Otherwise, evaluates to **false**.
- `(n)` &mdash; Evaluates to **true** if the capture group at offset _n_ was successfully matched; Otherwise, evaluates to **false**.
- `(<name>)` &mdash; Evaluates to **true** if the named capture group with the provided _name_ was successfully matched; Otherwise, evaluates to **false**.

The following conditions are out of scope but may be considered in a future proposal:

- `(DEFINE)` &mdash; Always evaluates to **false**. This allows you to define Subroutines.
- `(R)` &mdash; Evaluates to **true** if inside a recursive expression; Otherwise, evaluates to **false**.
- `(Rn)` &mdash; Evaluates to **true** if inside a recursive expression for the capture group at offset n; Otherwise, evaluates to **false**.
- `(R&name)` &mdash; Evaluates to **true** if inside a recursive expression for the named capture group with the provided name; Otherwise, evaluates to **false**.

<!--#endregion:syntax-->

<!--#region:semantics-->
<!-- # Semantics -->


<!--#endregion:semantics-->

<!--#region:examples-->
# Examples

```js
// conditional using lookahead:
const re1 = /^(?(?=\{)\{[0-9a-f]+\}|[0-9a-f]{4})$/
re1.test("0000"); // true
re1.test("{0}"); // true
re1.test("{00000000}"); // true

// match optional brackets
const re2 = /(?<open-bracket>\[)?(?<content>[^\]]+)(?(<open-bracket>)\]))/;
re1.test("abc"); // true
re1.test("[abc]"); // true
re1.test("[abc"); // false
```

<!--#endregion:examples-->

<!--#region:api-->
<!--
# API

> TODO: Provide description of High-level API.
-->
<!--#endregion:api-->

<!--#region:grammar-->
<!-- # Grammar

```grammarkdown
``` -->
<!--#endregion:grammar-->

<!--#region:references-->
<!-- # References

> TODO: Provide links to other specifications, etc.

* [Title](url)   -->
<!--#endregion:references-->

# History

- October 27, 2021 &mdash; Proposed for Stage 1 ([slides](https://1drv.ms/p/s!AjgWTO11Fk-Tkfl9fQQBfWQjIgf1QQ?e=jFynF5))
  - Outcome: Remained at Stage 0

<!--#region:todo-->
# TODO

The following is a high-level list of tasks to progress through each stage of the [TC39 proposal process](https://tc39.github.io/process-document/):

### Stage 1 Entrance Criteria

* [x] Identified a "[champion][Champion]" who will advance the addition.  
* [x] [Prose][Prose] outlining the problem or need and the general shape of a solution.  
* [x] Illustrative [examples][Examples] of usage.  
* [ ] ~~High-level [API][API].~~  

### Stage 2 Entrance Criteria

* [ ] [Initial specification text][Specification].  
* [ ] [Transpiler support][Transpiler] (_Optional_).  

### Stage 3 Entrance Criteria

* [ ] [Complete specification text][Specification].  
* [ ] Designated reviewers have [signed off][Stage3ReviewerSignOff] on the current spec text.  
* [ ] The ECMAScript editor has [signed off][Stage3EditorSignOff] on the current spec text.  

### Stage 4 Entrance Criteria

* [ ] [Test262](https://github.com/tc39/test262) acceptance tests have been written for mainline usage scenarios and [merged][Test262PullRequest].  
* [ ] Two compatible implementations which pass the acceptance tests: [\[1\]][Implementation1], [\[2\]][Implementation2].  
* [ ] A [pull request][Ecma262PullRequest] has been sent to tc39/ecma262 with the integrated spec text.  
* [ ] The ECMAScript editor has signed off on the [pull request][Ecma262PullRequest].  
<!--#endregion:todo-->

<!-- The following links are used throughout the README: -->

[Process]: https://tc39.es/process-document/
[Proposals]: https://github.com/tc39/proposals/
[Grammarkdown]: http://github.com/rbuckton/grammarkdown#readme
[Champion]: #status
[Prose]: #motivations
[Examples]: #examples
[API]: #api
[Specification]: https://rbuckton.github.io/proposal-regexp-conditionals

[Transpiler]: #todo
[Stage3ReviewerSignOff]: #todo
[Stage3EditorSignOff]: #todo
[Test262PullRequest]: #todo
[Implementation1]: #todo
[Implementation2]: #todo
[Ecma262PullRequest]: #todo
