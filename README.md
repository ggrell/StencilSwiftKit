# StencilSwiftKit

[![CircleCI](https://circleci.com/gh/SwiftGen/StencilSwiftKit/tree/master.svg?style=svg)](https://circleci.com/gh/SwiftGen/StencilSwiftKit/tree/master)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/StencilSwiftKit.svg)](https://img.shields.io/cocoapods/v/StencilSwiftKit.svg)
[![Platform](https://img.shields.io/cocoapods/p/StencilSwiftKit.svg?style=flat)](http://cocoadocs.org/docsets/StencilSwiftKit)
![Swift 3.0](https://img.shields.io/badge/Swift-3.0-orange.svg)

`StencilSwiftKit` is a framework bringing additional [Stencil](https://github.com/kylef/Stencil) nodes & filters dedicated to Swift code generation.

## Tags

* [Macro](Documentation/tag-macro.md) & [Call](Documentation/tag-call.md)
  * `{% macro <Name> <Params> %}…{% endmacro %}`
  * Defines a macro that will be replaced by the nodes inside of this block later when called
  * `{% call <Name> <Args> %}`
  * Calls a previously defined macro, passing it some arguments
* [Set](Documentation/tag-set.md)
  * `{% set <Name> %}…{% endset %}`
  * Renders the nodes inside this block immediately, and stores the result in the `<Name`>  variable of the current context.
* [Map](Documentation/tag-map.md)
  * `{% map <Variable> into <Name> using <ItemName> %}…{% endmap %}`
  * Apply a `map` operator to an array, and store the result into a new array variable `<Name>` in the current context.
  * Inside the map loop, a `maploop` special variable is available (akin to the `forloop` variable in `for` nodes). It exposes `maploop.counter`, `maploop.first`, `maploop.last` and `maploop.item`.

## Filters

* `join`: Deprecated. Will be removed now that the same filter exists in Stencil proper.
* [String filters](Documentation/filters-strings.md):
  * `escapeReservedKeywords`: Escape keywods reserved in the Swift language, by wrapping them inside backticks so that the can be used as regular escape keywords in Swift code.
  * `lowerFirstWord`
  * `snakeToCamelCase` / `snakeToCamelCaseNoPrefix`
  * `camelToSnakeCase`: Transforms text from camelCase to snake_case. By default it converts to lower case, unless a single optional argument is set to "false", "no" or "0".
  * `swiftIdentifier`: Transforms an arbitrary string into a valid Swift identifier (using only valid characters for a Swift identifier as defined in the Swift language reference)
  * `titlecase`
* [Number filters](Documentation/filters-numbers.md):
  * `int255toFloat`
  * `hexToInt`
  * `percent`

## StencilSwiftTemplate

This framework also contains [a `StencilSwiftTemplate` class](https://github.com/SwiftGen/StencilSwiftKit/blob/master/Sources/StencilSwiftTemplate.swift#L10), which is a subclass of `Stencil.Template` dedicated to remove extra newlines when rendering the template.

Indeed, such extra newlines could otherwise be inserted in the generated output because you want your template to be well formatted, and that can end up with Stencil nodes like `{% for … %}` and `{% if … %}` be alone in some lines of your template and that would render into nothing by themselves, generating empty lines in the output.

This template subclass aims to remove those lines generated by using a simple workaround when rendering, until there's an embeded way to handle that in Stencil proper (see [Stencil/#22](https://github.com/kylef/Stencil/issues/22)).


## Stencil.Extension & swiftStencilEnvironment

This framework also contains [helper methods for `Stencil.Extension` and `Stencil.Environment`](https://github.com/SwiftGen/StencilSwiftKit/blob/master/Sources/Environment.swift), to easily register all the tags and filters listed above on an existing `Stencil.Extension`, as well as to easily get a `Stencil.Environment` preconfigured with both those tags & filters `Extension` and the `StencilSwiftTemplate`.

## Parameters

This framework contains an additional parser, meant to parse a list of parameters from the CLI. For example, using [Commander](https://github.com/kylef/Commander), if you receive a `[String]` from a `VariadicOption<String>`, you can use the parser to convert it into a structured dictionary. For example:

```swift
["foo=1", "bar=2", "baz.qux=hello", "baz.items=a", "baz.items=b", "something"]
```

will become

```swift
[
  "foo": "1",
  "bar": "2",
  "baz": [
    "qux": "hello",
    "items": [
      "a",
      "b"
    ]
  ],
  something: true
]
```

For easier use, you can use the `StencilContext.enrich(context:parameters:environment:)` function to add the following variables to a context:

- `param`: the parsed parameters using the parser mentioned above.
- `env`: a dictionary with all available environment variables (such as `PATH`).
