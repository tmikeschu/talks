footer: 81 @tmikeschu
build-lists: true
slidenumbers: true

#[fit] Dealing

#[fit] with

#[fit] /Regexion/

self.conference
Mike Schutte
June 8, 2019

---

- @tmikeschu

- :house_with_garden:

- :sunrise_over_mountains:

- :guitar: :microphone: :musical_keyboard:

- :runner: :bike:

- ![inline](https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/TED_three_letter_logo.svg/1200px-TED_three_letter_logo.svg.png)

^ My name is Mike Schutte. I live three miles east of here in West Village where
my partner Hannah and I look after our lovely 119 year old baby made of bricks,
wood, and a basement that takes in water. I wrote my first program in the
summer of 2016 and have loved it ever since. I am a front end engineer at TED,
and my training is based on Ruby and Ruby on Rails.

---

![](https://stemcenter.utexas.edu/sites/default/files/styles/utexas_hero_photo_image/public/TEALS%20Logo_news.jpg?itok=g2gu02Ei)

---

# Victory Conditions

- Feel at piece with the craziness of regex

- Have a few strategies for deciding when to use regex

- Be like "wow capture groups are _amazing_"

---

# Roadmap

- My soapbox: why regex should be messy

- Brief overview of regular expressions

* When to use regex

* Capture groups

---

Disclaimer

![inline](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Unofficial_JavaScript_logo_2.svg/1200px-Unofficial_JavaScript_logo_2.svg.png)

---

me using regex

![](https://media.giphy.com/media/W4GT7vC6Whj9K/giphy.gif)

---

## How to deal with

# /regexion/:

---

# Strings and Language

- Typos

- Duplication

- Patterns

- Formats

- Repetition

^ An unmessy tool would be a square peg in a round hole.

---

# Accept it

- Regex is messy

- because string data is messy

- because language is messy

^ Instead of being frustrated with regex, we should be grateful for its capacity
to parse our hot mess of a human abstraction called language.

---

# Regular Expressions: An Overview

- Born in 1951

- Popularized in 1968

  - Text editor search

  - Lexical analysis

- POSIX, Perl, PCRE

- Finite state machine

---

![inline](https://i.ytimg.com/vi/hprXxJHQVfQ/maxresdefault.jpg)

https://www.youtube.com/watch?v=hprXxJHQVfQ

---

# Code That Wants to be Regexed

---

## If you ask more than one question about a string...

---

## Ditch `&&` and `||` for `//`

---

```diff
- someString.startsWith(":") &&
-   someString.split("").some(char => Boolean(Number(char)))
+ /^:.*\d/.test(someString)

- someString.includes("someWord") || someString.includes("someOtherWord");
+ /someWord|someOtherWord/.test(someString)
```

---

(Array of characters).context < (string).context

---

> Regex forces you to consider the string as (more of) a whole

---

# Methods to use

---

- Change format

  - String.prototype.replace (=> String)

- Get substring(s)

  - String.prototype.match (=> Array)

- Assert string qualities

  - Regex.prototype.test (=> Boolean)

- Stateful search

  - Regex.prototype.exec (=> Array)

---

You know what those parentheses in regular expressions are, right?

```js
/(\d+)/;
```

![inline](https://media.giphy.com/media/l2SpKR1aR4FMBFQwE/giphy.gif)

---

### Capture Groups: Keep It Together

## `/()/`

^ I originally wanted to cover other advanced regex topics in this talk, but as
I developed the following lesson, I realized that the power of regex is not in
a single comparison with traditional string methods. The power of regex is in
its capacity to adapt to changing requirements. Regex has grown on me for the
same reason functional programming has: something that was hard to learn at
first has made me more equipped to write more flexible software.

---

- Is familiarity worth rigidity?

- Is difficulty worth flexibility?

---

Is difficulty worth flexibility? :white_check_mark:

---

# Task

## create a function that

- Takes in a name in `First Last` format

- And returns the name in `Last, First` format

^ This is a case where the meaningful values are the same, we just change the
way they are formatted. This is a minor difference for us as humans and our big
awesome brains, but this change really puts standard programming logic to the
test.

---

```js
const albus = "Albus Dumbledore";

function lastFirst(name) {
  // TODO
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus"
```

---

# Approach #1: Split

```js
function lastFirst(name) {
  return name
    .split(" ")
    .reverse()
    .join(", ");
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus"
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus"
```

---

[^_]

```js
someString.replace(/(cats)(dogs)/, (full, group1, group2) => {
  // do stuff with the groups
});
```

[^_]: `https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#Specifying_a_function_as_a_parameter`

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too.

```js
const albus = "Albus Percival Dumbledore";

fullName(albus); // => "Dumbledore, Albus Percival"
```

---

# Approach #1: Split

```js
function lastFirst(name) {
  return name
    .split(" ")
    .reverse()
    .join(", ");
}
console.log(lastFirst(albus)); // => "Dumbledore, Percival, Albus"
```

---

# :neutral_face:

---

```js
function lastFirst(rawName) {
  const names = rawName.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  return `${last}, ${rest.join(" ")}`;
}

console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival"
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(albus)); // => "Percival, Albus Dumbledore"
```

^ the `Dumbledore` bit is untouched.

---

# :neutral_face:

---

```js
function lastFirst(name) {
  const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival"
```

---

```diff
- /(\w+)\s(\w+)/;
+ /(\w+\s*\w*)\s(\w+)/;
```

---

# Comparison

## Split

- Calculating indices

- Accommodating for zero-based counting

- Array.prototype methods

- String interpolation

---

# Comparison

## Regex

- Patterns

- There is a first bit

- and a last bit

- and _sometimes_ extra middle bits in the first bit

---

# That is messy

---

# That is okay

---

#[fit] That is _awesome_

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too
- ...multiple middle names

```js
const albus = "Albus Percival Wulfric Brian Dumbledore";
lastFirst(albus); // => "Dumbledore, Albus Percival Wulfric Brian"
```

---

# Approach #1: Split

```js
function lastFirst(rawName) {
  const names = rawName.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  return `${last}, ${rest.join(" ")}`;
}

console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival Wulfric Brian"
```

---

# :heart_eyes_cat:

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(albus)); // => "Wulfric, Albus Percival Brian Dumbledore"
```

---

# :neutral_face:

---

```diff
- const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
+ const reFirstLast = /(\w+(\s\w+)*)\s(\w+)/;
```

---

```js
function lastFirst(name) {
  const reFirstLast = /(\w+(\s\w+)*)\s(\w+)/;
  return name.replace(reFirstLast, "$3, $1");
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival Wulfric Brian"
```

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too
- ...multiple middle names
- ...suffixes too

```js
const albus = "Albus Percival Wulfric Brian Dumbledore, Jr.";
lastFirst(albus); // => "Dumbledore, Albus Percival Wulfric Brian, Jr."
```

---

# :sweat_smile:

---

# Approach #1: Split

```js
function lastFirst(rawName) {
  const names = rawName.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  return `${last}, ${rest.join(" ")}`;
}

console.log(lastFirst(albus)); // => "Jr., Albus Percival Wulfric Brian Dumbledore,"
```

---

# :neutral_face:

---

```js
function lastFirst(rawName) {
  const [name, suffix] = rawName.split(", ");
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  const output = `${last}, ${rest.join(" ")}`;
  if (suffix) {
    return `${output}, ${suffix}`;
  }
  return output;
}

console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival, Jr."
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+(\s\w+)*)\s(\w+)/;
  return name.replace(reFirstLast, "$3, $1");
}
console.log(lastFirst(albus)); // => "Dumbledore, Albus Percival, Jr."
```

---

# :open_mouth:

---

# :heart_eyes_cat:

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too
- ...multiple middle names
- ...suffixes too
- ...just first name is okay ¯\_(ツ)\_/¯

```js
const albus = "Albus";
lastFirst(albus); // => "Albus"
```

---

# Approach 1: Split

```js
function lastFirst(rawName) {
  const [name, suffix] = rawName.split(", ");
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  const output = `${last}, ${rest.join(" ")}`;
  if (suffix) {
    return `${output}, ${suffix}`;
  }
  return output;
}

console.log(lastFirst(albus)); // => "Albus,"
```

---

# :neutral_face:

---

```js
function lastFirst(rawName) {
  const [name, suffix] = rawName.split(", ");
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  const output = `${last}, ${rest.join(" ")}`;
  if (suffix) {
    return `${output}, ${suffix}`;
  }
  if (output.endsWith(",")) {
    return output.slice(0, output.length - 1);
  }
  return output;
}
```

---

# :neutral_face:

```diff
  const output = `${last}, ${rest.join(" ")}`;
  if (suffix) {
    return `${output}, ${suffix}`;
  }
+ if (output.endsWith(",")) {
+   return output.slice(0, output.length - 1);
+ }
  return output
```

---

```js
function lastFirst(rawName) {
  const [name, suffix] = rawName.split(", ");
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex).join(" ");

  let output = last;
  if (Boolean(rest)) {
    output += `, ${rest}`;
  }

  if (suffix) {
    output += `, ${suffix}`;
  }
  return output;
}
```

^ We are rebuilding a string step by step manually. It feels icky!

---

# :neutral_face:

```diff
- const output = `${last}, ${rest.join(" ")}`;
- if (suffix) {
-   return `${output}, ${suffix}`;
- }
- if (output.lastIndexOf(",") === output.length) {
-   return output.slice(0, output.length - 1);
- }
+ let output = last;
+ if (Boolean(rest)) {
+   output += `, ${rest}`;
+ }

+ if (suffix) {
+   output += `, ${suffix}`;
+ }
  return output;
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+(\s\w+)*)\s(\w+)/;
  return name.replace(reFirstLast, "$3, $1");
}
console.log(lastFirst(albus)); // => "Albus"
```

---

# :open_mouth:

---

# :heart_eyes_cat:

---

```js
const reFirstLast = /(\w+(\s\w+)*)\s(\w+)/;
```

^ If you're like me, you don't find that very pleasant to look at.

---

## flexibility > readability

---

# ...about "readability"

- Is German readable?

---

### Review

- Rich history specifically designed for analyzing and searching text

- Regex is messy because string data is messy because language is messy

- If you have more than one question about your string...

- Capture groups are great for manipulating substrings

---

Go forth and parse your strings!

---

Embrace the /pain/!

---

Don't fear /regexion/!

---

Response /gracefully/ to change :smile:

---

#[fit] Thank you!

---

# Resources

- Repl-ish tool: https://regexr.com/
- Cheat sheet: ://www.rexegg.com/regex-quickstart.html
- Wiki: https://en.wikipedia.org/wiki/Regular_expression
- Named capture groups: https://github.com/tc39/proposal-regexp-named-groups
