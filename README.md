# PropertyMatcher

Evaluate a conditional expression of regex-based property matches, where the property _values_ to be matched against are provided by a `PropertyMatcher::PropertyProvider` subclass.

## Requirements:
- C++11 support
- [RE2](https://github.com/google/re2)
	
## Supported Operators:
- `(...)` :	logical grouping
- `&&`	:	logical AND
- `||`	:	logical OR
- `!`	:	negation
- `=~`	:	regex binding operator (applies the regex provided by its second operand to the _value_ of the `PROPERTY` provided by its first operand)

## Property Matches:
`PROPERTY_NAME =~ "MATCH_REGEX"`

Generally, you should enclose the regex inside double-quotes (`""`). Literal double-quotes inside the regex string should be escaped as: `\"`

The `PROPERTY_NAME` should not contain whitespace, and *not* be enclosed in quotes.

### Regex Syntax:
For regex syntax, see: https://github.com/google/re2/wiki/Syntax

The regex engine used is RE2.
> RE2 was designed and implemented with an explicit goal of being able to handle regular expressions from untrusted users without risk.
> https://github.com/google/re2/wiki/WhyRE2

## Basic Use:
- Create a `PropertyMatcher::PropertyProvider` subclass that implements the `getPropertyValue` method.
- Call `PropertyMatcher::evaluateConditionString` with a conditional expression string to evaluate and a `PropertyProvider` subclass.

### Handling Errors:
- `PropertyMatcher::evaluateConditionString` will **throw** `std::runtime_error` on error, with a descriptive error message.

## Example Conditional Expressions:
Assuming a `PropertyProvider` that can query values for property names `GIT_BRANCH` and `PLATFORM`
- `GIT_BRANCH =~ "^master$" && PLATFORM =~ "^Windows"`
- `!(GIT_BRANCH =~ "^master$") || (GIT_BRANCH =~ "^master$" && PLATFORM =~ "^Windows")`
