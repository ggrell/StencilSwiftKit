# StencilSwiftKit CHANGELOG

---

## Master

### Bug Fixes

_None_

### Breaking Changes

_None_

### New Features

_None_

### Internal Changes

_None_

## 1.0.0

### Bug Fixes

_None_

### New Features

* Added support for Swift Package Manager.  
  [Krzysztof Zabłocki](https://github.com/krzysztofzablocki)
  [#15](https://github.com/SwiftGen/StencilSwiftKit/issues/15)
* Added `MapNode` to apply a `map` operator to an array.
  You can now use `{% map someArray into result using item %}`
  to do the equivalent of the `result = someArray.map { item in … }` Swift code.  
  [David Jennes](https://github.com/djbe)
  [#11](https://github.com/SwiftGen/StencilSwiftKit/pull/11)
* Add a "parameters parser" able to transform parameters passed as a set of strings
  (`a=1 b.x=2 b.y=3 c=4 c=5`) — typically provided as the command line arguments of a CLI
   — into a Dictionary suitable for Stencil contexts.  
  [David Jennes](https://github.com/djbe)
  [#8](https://github.com/SwiftGen/StencilSwiftKit/pull/8)
* Add a `StencilContext.enrich` function to enrich Stencil contexts with `param` and `env` dictionaries.  
  The `param` dictionary typically contains parameters parsed via the parameters parser above.  
  The `env` dictionary contains all the environment variables. You can thus access them in
  your templates using `env.USER`, `env.LANG`, `env.PRODUCT_MODULE_NAME`, etc.  
  [#19](https://github.com/SwiftGen/StencilSwiftKit/pull/19)

### Internal Changes

* Renamed `SwiftTemplate` to `StencilSwiftTemplate`.  
  [David Jennes](https://github.com/djbe)
  [#14](https://github.com/SwiftGen/StencilSwiftKit/issues/14)
* Refactor stencil swift extensions registration for easier use with an existing `Extension`.  
  [David Jennes](https://github.com/djbe)
  [#16](https://github.com/SwiftGen/StencilSwiftKit/issues/16)
* Refactor stencil node tests to not use templates and output files.  
  [David Jennes](https://github.com/djbe)
  [#17](https://github.com/SwiftGen/StencilSwiftKit/issues/17)

## Pre-1.0.0

_See SwitftGen's own CHANGELOG pre SwiftGen 4.2 version, before the refactoring that led us to split the code in frameworks_
