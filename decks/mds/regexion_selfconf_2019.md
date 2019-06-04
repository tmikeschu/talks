footer: (TODO slides) @tmikeschu
build-lists: true
slidenumbers: true

#[fit] Dealing

#[fit] with

#[fit] /Regexion/

self.conference
Mike Schutte
June 6, 2019

---

![](https://media.giphy.com/media/W4GT7vC6Whj9K/giphy.gif)

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

- Understand capture groups

---

# Roadmap

- Brief Overview of Regular Expressions

- Why Regex **Should** Be Messy

- Capture Groups

- Lookaheads

---

# Regular Expressions: An Overview

- Born in 1951

- Popularized in 1968

  - Text editor search

  - Lexical analysis

- POSIX, Perl, PCRE

---

# Code That Wants to be Regexed

## && ||

```js
someString.includes(someWord) || someString.includes(someOtherWord);
```

---

# Capture Groups: Keep It Together

`/()/`

---

# Task

## create a function that

- Takes in a name in `First Last` format

- And returns the name in `Last, First` format

---

```js
const harry = "Harry Potter";

function lastFirst(name) {
  // TODO
}
console.log(lastFirst(harry)); // => "Potter, Harry"
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
console.log(lastFirst(harry)); // => "Potter, Harry"
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(harry)); // => "Potter, Harry"
```

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too.

```js
const harry = "Harry James Potter";

fullName(harry); // => "Potter, Harry James"
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
console.log(lastFirst(harry)); // => "Potter, James, Harry"
```

---

# :neutral_face:

---

```js
function lastFirst2(rawName) {
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  return `${last}, ${rest.join(" ")}`;
}

console.log(lastFirst(harry)); // => "Potter, Harry James"
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(harry)); // => "James, Harry Potter"
```

---

# :neutral_face:

---

```js
function lastFirst(name) {
  const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(harry)); // => "Potter, Harry James"
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

- and **sometimes** some extra middle bits in the first bit

---

# That is messy

---

# That is okay

---

#[fit] That is awesome

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too
- ...suffixes too

```js
const harry = "Harry James Potter, Jr.";
lastFirst(name); // => "Potter, Harry James, Jr."
```

---

# :sweat_smile:

---

# Approach #1: Split

```js
function lastFirst2(rawName) {
  const names = name.split(" ");
  const maxIndex = names.length - 1;
  const last = names[maxIndex];
  const rest = names.slice(0, maxIndex);

  return `${last}, ${rest.join(" ")}`;
}

console.log(lastFirst(harry)); // => "Jr., Harry James Potter,"
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

console.log(lastFirst(harry)); // => "Potter, Harry James, Jr."
```

---

# Approach #2: Regex

```js
function lastFirst(name) {
  const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(harry)); // => "Potter, Harry James, Jr."
```

---

# :open_mouth:

---

# :heart_eyes_cat:

---

#[fit] :warning: Change Alert :warning:

---

- ...middle names too
- ...suffixes too
- ...just first name is okay ¯\_(ツ)\_/¯

```js
const harry = "Harry";
lastFirst(harry); // => Harry
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

console.log(lastFirst(harry)); // => "Harry,"
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
  if (output.lastIndexOf(",") === output.length) {
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
+ if (output.lastIndexOf(",") === output.length) {
+   return output.slice(0, output.length - 1);
+ }
  return output
```

---

```js
function lastFirst(rawName) {
  const [name, suffix] = rawName.split(", ");
  const names = nam.split(" ");
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
  const reFirstLast = /(\w+\s*\w*)\s(\w+)/;
  return name.replace(reFirstLast, "$2, $1");
}
console.log(lastFirst(harry)); // => "Harry"
```

---

# :open_mouth:

---

# :heart_eyes_cat:

---

# Review

-

---

# What You Can Do Tomorrow

- If you find yourself

---

#[fit] Thank you!

---

# Resources

- Repl-ish tool: https://regexr.com/
- Cheat sheet: ://www.rexegg.com/regex-quickstart.html
- Wiki: https://en.wikipedia.org/wiki/Regular_expression
- Named capture groups: https://github.com/tc39/proposal-regexp-named-groups
