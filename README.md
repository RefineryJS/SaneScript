SaneScript
===========

> JavaScript as it should be

SaneScript v0.4.0

Inspired from [this article](https://github.com/rwaldron/tc39-notes/blob/master/es6/2015-01/JSExperimentalDirections.pdf)

See [Features.md](https://github.com/SaneScript/SaneScript/blob/master/Features.md) for the full list of the SaneScript features.

See [Details.md](https://github.com/SaneScript/SaneScript/blob/master/Details.md) and [Flags.md](https://github.com/SaneScript/SaneScript/blob/master/Flags.md) for the deep inside of the SaneScript specification.

See [here](https://gist.github.com/HyeonuPark/6af6965ca6b91cc6a79c) for the rules of the SaneScript specification.

# Description

## What's SaneScript?

SaneScript is a programming language which has same syntax as JavaScript, especially ES2015 and newer, but slightly different semantic. It's main purpose is to compile to JavaScript.

## Another compile-to-js? Why does it even needed?

JavaScript is a good language, but some point of it is insane. Many of those bad points can be replaced by new things from ES2015 or newer, but using new syntax is less readable and need more typing then traditional way.

They cannot be replaced due to backward compatibility. But wait, now everybody compiles to JavaScript, then why don't we compile JavaScript to JavaScript?

## How about CoffeeScript?

CoffeeScript is a good language. It's syntax can hide lots of JavaScript's ugly parts. But it's not a JavaScript at all, and JavaScript is rapidly changing now. For example, if you want to use async-await keywords in CoffeeScript, you should just wait until language designer and compiler maintainer decide to include that feature to CoffeeScript.

In SaneScript, it's syntax is just JavaScript. It means we don't need any configuration to adapt JavaScript's future standard.

And there must be some people who don't like CoffeeScript's syntax, such as python-like block and parenthesis-less function call, but also hate JavaScript's old-and-wrong choices. If you're such a person, SaneScript is for you!

And also, you can use your comfortable tools for JavaScript like editors and linters. No more configuration for it. Just start coding!

# [LICENSE](http://creativecommons.org/licenses/by-sa/3.0/)
![CC BY-SA](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-sa.svg)
