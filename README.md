# HEL

[![GoDoc](https://godoc.org/github.com/hashicorp/hel?status.png)](https://godoc.org/github.com/hashicorp/hel) [![Build Status](https://travis-ci.org/hashicorp/hel.svg?branch=master)](https://travis-ci.org/hashicorp/hel)

HEL (HashiCorp Embedded Language) is a lightweight embedded language used
primarily for configuration interpolation. The goal of HEL is to make a simple
language for interpolations in the various configurations of HashiCorp tools.

HEL is built to interpolate any string, but is in use by HashiCorp primarily
with [HCL](https://github.com/hashicorp/hcl). HCL is _not required_ in any
way for use with HEL.

## Why?

Many of our tools have support for something similar to templates, but
within the configuration itself. The most prominent requirement was in
[Terraform](https://github.com/hashicorp/terraform) where we wanted the
configuration to be able to reference values from elsewhere in the
configuration. Example:

    foo = "hello ${var.world}"

We originally used a full templating language for this, but found it
was too heavy weight. We then moved to very basic regular expression based
string replacement, but found the need for basic arithmetic and function
calls.

Ultimately, we wrote our own mini-language within Terraform itself. As
we built other projects such as [Nomad](https://nomadproject.io) and
[Otto](https://ottoproject.io), the need for basic interpolations arose
again.

Thus HEL was born. It is extracted from Terraform, cleaned up, and
better tested for general purpose use.

## Syntax

For a complete grammar, please see the parser itself. A high-level overview
of the syntax and grammer is listed here.

Code begins within `${` and `}`. Outside of this, text is treated
literally. For example, `foo` is a valid HEL program that is just the
string "foo", but `foo ${bar}` is an HEL program that is the string "foo "
concatened with the value of `bar`. For the remainder of the syntax
docs, we'll assume you're within `${}`.

  * Identifiers are any text in the format of `[a-zA-Z0-9-.]`. Example
    identifiers: `foo`, `var.foo`, `foo-bar`.

  * Strings are double quoted and can contain any UTF-8 characters.

  * Witin strings, further interpolations can be opened with `${}`.

  * Strings are double-quoted and can contain any UTF-8 characters.
    Example: `"Hello, World"`

  * Numbers are assumed to be base 10. If you prefix a number with 0x,
    it is treated as a hexadecimal. If it is prefixed with 0, it is
    treated as an octal. Numbers can be in scientific notation: "1e10".

  * Boolean values: `true`, `false`

