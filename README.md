<p align="center">
  <img alt="babylon" src="https://raw.githubusercontent.com/babel/logo/master/babylon.png" width="700">
</p>

<p align="center">
  Babylon is a JavaScript parser used in <a href="https://github.com/babel/babel">Babel</a>.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/babylon"><img alt="NPM Version" src="https://img.shields.io/npm/v/babylon.svg?style=flat"></a>
  <a href="https://travis-ci.org/babel/babylon"><img alt="Travis Status" src="https://img.shields.io/travis/babel/babylon/master.svg?style=flat&label=travis"></a>
  <a href="https://codecov.io/gh/babel/babylon"><img alt="Codecov Status" src="https://img.shields.io/codecov/c/github/babel/babylon/master.svg?style=flat"></a>
</p>

 - The latest ECMAScript version enabled by default (ES2017).
 - Comment attachment.
 - Support for JSX, Flow, Typescript (WIP).
 - Support for experimental language proposals (accepting PRs for anything at least [stage-0](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md)).

## Contributing

Check out [contributing.md](https://github.com/babel/babylon/blob/master/CONTRIBUTING.md)

## Credits

Heavily based on [acorn](https://github.com/marijnh/acorn) and [acorn-jsx](https://github.com/RReverser/acorn-jsx),
thanks to the awesome work of [@RReverser](https://github.com/RReverser) and [@marijnh](https://github.com/marijnh).

Significant diversions are expected to occur in the future such as streaming, EBNF definitions, sweet.js integration, interspatial parsing and more.

## API

### `babylon.parse(code, [options])`

### `babylon.parseExpression(code, [options])`

`parse()` parses the provided `code` as an entire ECMAScript program, while
`parseExpression()` tries to parse a single Expression with performance in
mind. When in doubt, use `.parse()`.

### Options

- **allowImportExportEverywhere**: By default, `import` and `export`
  declarations can only appear at a program's top level. Setting this
  option to `true` allows them anywhere where a statement is allowed.

- **allowReturnOutsideFunction**: By default, a return statement at
  the top level raises an error. Set this to `true` to accept such
  code.

- **allowSuperOutsideMethod**: TODO

- **sourceType**: Indicate the mode the code should be parsed in. Can be
  either `"script"` or `"module"`.

- **sourceFilename**: Correlate output AST nodes with their source filename.  Useful when generating code and source maps from the ASTs of multiple input files.

- **startLine**: By default, the first line of code parsed is treated as line 1. You can provide a line number to alternatively start with. Useful for integration with other source tools.

- **plugins**: Array containing the plugins that you want to enable.

- **strictMode**: TODO

- **ranges**: Adds a `ranges` property to each node: `[node.start, node.end]`

### Output

Babylon generates AST according to [Babel AST format][].
It is based on [ESTree spec][] with the following deviations:

> There is now an `estree` plugin which reverts these deviations

- [Literal][] token is replaced with [StringLiteral][], [NumericLiteral][], [BooleanLiteral][], [NullLiteral][], [RegExpLiteral][]
- [Property][] token is replaced with [ObjectProperty][] and [ObjectMethod][]
- [MethodDefinition][] is replaced with [ClassMethod][]
- [Program][] and [BlockStatement][] contain additional `directives` field with [Directive][] and [DirectiveLiteral][]
- [ClassMethod][], [ObjectProperty][], and [ObjectMethod][] value property's properties in [FunctionExpression][] is coerced/brought into the main method node.

AST for JSX code is based on [Facebook JSX AST][] with the addition of one node type:

- `JSXText`

[Babel AST format]: https://github.com/babel/babylon/blob/master/ast/spec.md
[ESTree spec]: https://github.com/estree/estree

[Literal]: https://github.com/estree/estree/blob/master/es5.md#literal
[Property]: https://github.com/estree/estree/blob/master/es5.md#property
[MethodDefinition]: https://github.com/estree/estree/blob/master/es2015.md#methoddefinition

[StringLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#stringliteral
[NumericLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#numericliteral
[BooleanLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#booleanliteral
[NullLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#nullliteral
[RegExpLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#regexpliteral
[ObjectProperty]: https://github.com/babel/babylon/blob/master/ast/spec.md#objectproperty
[ObjectMethod]: https://github.com/babel/babylon/blob/master/ast/spec.md#objectmethod
[ClassMethod]: https://github.com/babel/babylon/blob/master/ast/spec.md#classmethod
[Program]: https://github.com/babel/babylon/blob/master/ast/spec.md#programs
[BlockStatement]: https://github.com/babel/babylon/blob/master/ast/spec.md#blockstatement
[Directive]: https://github.com/babel/babylon/blob/master/ast/spec.md#directive
[DirectiveLiteral]: https://github.com/babel/babylon/blob/master/ast/spec.md#directiveliteral
[FunctionExpression]: https://github.com/babel/babylon/blob/master/ast/spec.md#functionexpression

[Facebook JSX AST]: https://github.com/facebook/jsx/blob/master/AST.md

### Semver

Babylon follows semver in most situations. The only thing to note is that some spec-compliancy bug fixes may be released under patch versions.

For example: We push a fix to early error on something like [#107](https://github.com/babel/babylon/pull/107) - multiple default exports per file. That would be considered a bug fix even though it would cause a build to fail.

### Example

```javascript
require("babylon").parse("code", {
  // parse in strict mode and allow module declarations
  sourceType: "module",

  plugins: [
    // enable jsx and flow syntax
    "jsx",
    "flow"
  ]
});
```

### Plugins

 - `estree`
 - `jsx`
 - `flow`
 - `doExpressions`
 - `objectRestSpread` ([proposal](https://github.com/tc39/proposal-object-rest-spread))
 - `decorators` (Based on an outdated version of the Decorators proposal. Will be removed in a future version of `Babylon`)
 - `classProperties` ([proposal](https://github.com/tc39/proposal-class-public-fields))
 - `classPrivateProperties` ([proposal](https://github.com/tc39/proposal-private-fields))
 - `exportExtensions` ([proposal 1](https://github.com/leebyron/ecmascript-export-default-from)), ([proposal 2](https://github.com/leebyron/ecmascript-export-ns-from))
 - `asyncGenerators` ([proposal](https://github.com/tc39/proposal-async-iteration))
 - `functionBind` ([proposal](https://github.com/zenparsing/es-function-bind))
 - `functionSent`
 - `dynamicImport` ([proposal](https://github.com/tc39/proposal-dynamic-import))
 - `numericSeparator` ([proposal](https://github.com/samuelgoto/proposal-numeric-separator))
 - `importMeta` ([proposal](https://github.com/tc39/proposal-import-meta))

### FAQ

#### Will Babylon support a plugin system?

Previous issues: [babel/babel#1351](https://github.com/babel/babel/issues/1351), [#500](https://github.com/babel/babylon/issues/500).

We currently aren't willing to commit to supporting the API for plugins or the resulting ecosystem (there is already enough work maintaining Babel's own plugin system). It's not clear how to make that API effective, and it would limit out ability to refactor and optimize the codebase.

Our current recommendation for those that want to create their own custom syntax is for users to fork Babylon.

To consume your custom parser, you can add to your `.babelrc` via its npm package name or require it if using JavaScript,

```json
{
  "parserOpts": {
    "parser": "custom-fork-of-babylon-on-npm-here"
  }
}
```
