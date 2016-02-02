SaneScript
===========

> Write code that looks right, run code that works right.

SaneScript specification v0.5.0

Inspired from [this article](https://github.com/rwaldron/tc39-notes/blob/master/es6/2015-01/JSExperimentalDirections.pdf)

See [Features.md](https://github.com/SaneScript/SaneScript/blob/master/Features.md) for the full list of SaneScript features.

See [Details.md](https://github.com/SaneScript/SaneScript/blob/master/Details.md) and [Flags.md](https://github.com/SaneScript/SaneScript/blob/master/Flags.md) for the in-depth specification of SaneScript.

See [here](https://gist.github.com/HyeonuPark/6af6965ca6b91cc6a79c) for the principles of the SaneScript.

# Description

## What's SaneScript?

SaneScript is a programming language which looks just like JavaScript(More specifically, ES2015 and newer) but has different semantic. It's main purpose is to be compiled to JavaScript.

## Another compile-to-js? What for?

JavaScript is a good language, but some part of it is... '*insane*'. ES2015 and later versions of JavaScript brings some new features to fix this, but the new syntax is less readable and requires more typing than traditional way.

The new features can't replace the old ones, for the sake of backward compatibility; But wait, now everybody compiles to JavaScript, then why don't we compile JavaScript to JavaScript?

With SaneScript, you can write code that looks just right, not works just right. Then SaneScript will compile your code to the one that works as expected.

## How about CoffeeScript?

CoffeeScript is a good language. It's syntax can hide lots of JavaScript's ugly parts. But it's not exactly JavaScript, and JavaScript is rapidly changing now. So if you want to use JavaScript's new features in CoffeeScript, you have to wait until the CoffeeScript designer and the compiler maintainer includes it in the CoffeeScript.

SaneScript's syntax is just JavaScript. It means we don't have to make additional changes to adopt JavaScript's future standards.

And there are some people who don't like CoffeeScript's syntax(python-like blocks, parenthesis-less function calls, etc.) but also hate JavaScript's old-and-wrong choices. If you are one of those people, SaneScript is for you!

And also, you can use your comfortable tools for JavaScript, like editors, linters, and even build processes with SaneScript. No more new tools to learn. Just start coding!

# [LICENSE](http://creativecommons.org/licenses/by-sa/3.0/)
![CC BY-SA](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-sa.svg)
