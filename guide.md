# Subtext guide

- **Version**: 1.0.0-neno
- **Authors**: Gordon Brander, polyrainbow

Subtext is markup for note-taking. It aims to add just the tiniest bit of structure on top of plain text, to make notes more useful.

## Introduction

Subtext is a text-based, line-oriented hypertext format, designed for note-taking.

Subtext markup is made up of ordinary lines of text, which are interpreted as a list of blocks. Lines that are prefixed with magic "sigil" characters are treated as special blocks. Lines without sigils are treated as text blocks. Empty lines are ignored. Here's a quick sample:

````
# Heading

Plain text.

- List item
- List item

> Quoted text

URLs like https://example.com are automatically linked.

You can also link to local pages using short /slashlinks or [[Wikilinks]].

```
fn this_is_a_code_block() {
  return;
}
```

$this-is-a-key value pair
````

Subtext is barely there—just the tiniest bit of formatting on top of plain text. More importantly, Subtext adds structure to plain text so you can build tools to do more with it. Things like:

- Excerpt a document by taking the first text block
- Select all quotes from a collection of documents
- Select all links, and generate a link graph for a collection of documents
- Find all backlinks and append them to the document as links

Subtext is designed to be used in a wide range of settings, from simple command-line utilities, to advanced clients. It is extremely easy to parse, and can be rendered in a single pass, or even streamed.

## Blocks

### Text blocks

One line is one block in Subtext. This means you write text the way you would anyway. For example, here are two blocks:

```
Delays in systems cause waves.

You can only smooth them out with stocks that match the scale of the delay-waves.
```

One line is one block, so blank lines are optional. This is also two blocks:

```
Block 1
Block 2
```

Blank lines have no meaning in Subtext, but can be added between any blocks, for source code readability.

Usually, it's nice to put a blank line between text blocks for readability reasons.

### Heading blocks

Heading blocks start with a `#`

```
# This is a heading

Text underneath.
```

The space after the `#` is optional, but it's nice to include it for readability.

```
#This is also a heading
```

### List blocks

List blocks start with a `-`

```
- List item
- List item
- List item
```

The space after the `-` is optional, but it's nice to include it for readability.

### Quote blocks

Quote blocks start with a `>`

```
> Life is not a problem to be solved, but a reality to be experienced.
```

The space after the `>` is optional, but it's nice to include it for readability.

## Links

Subtext will detect any http(s) URLs, and automatically link them.

```
Links like https://example.com will be automatically linked.
```

Note you must include the protocol (the `http://` or `https://` bit) for Subtext
to sniff out the URL.

Subtext parsers can automatically detect `http` links this way, but not every protocol will work.

## Slashlinks

Subtext also has a shortcut for linking to local pages, called "slashlinks". Slashlinks are like `#hashtags`, or `@mentions` except instead of starting with a `#` or an `@`, they start with a `/`, like this: `/link`

Here are some examples of slashlinks:

```
This is a /slashlink. A few more:

/evolution
/requisite-variety
/DesignPatterns
```

Slashlinks can't include spaces. It is considered good practice to use dashes `-` instead. Also, slashlinks are case-insensitive. That is, `/Evolution` and `/evolution` reference the same thing. Feel free to write whatever looks better to you.

```
/whole-earth-catalog, a collection of /DIY books and tools.
```

Slashlinks can also include hierarchical sub-parts, just like a URL. This can be useful when two concepts from different fields share the same name.

```
/journal/2021-10-09
/vaclav-smil/energy-and-civilization
/climate/carbon-sinks
```

## Key-value pairs

A key-value block is a `$` followed by any alphanumeric string, optionally followed by at
least one whitespace character and a value (any kind of string).

Example key-value blocks:
```
$check-back-on 1st of January 2050
$location   [[Berlin]]
$liked
```

Key, described as a regular expression:

```
/[\p{L}\p{M}\d\-_]+/u
```

Key-value pairs are a fundamental primitive with a wide range of potential use-cases for tooling. Like any other type of block, key-value blocks could be gathered by key into lists, concatenated, or collected using a first- or last-key-wins to get simple key/value data.

- You could execute queries such as: “list all questions (`Q:` blocks) in my notes”.
- You could transform a collection of notes into a sparse table by treating each note as a row, and treating keys as columns. Denser tabular data can be had by filtering notes to only include those with a particular set of keys, and then concatenating, JSON-encoding, or dropping duplicate keys. Tada! CSV.
- You could include headmatter in the body of a note. This can make it easier to integrate notes with static site generators, or other tools.

## Code blocks

Code blocks are different to all other blocks in that they consist of more than
one line.

Example code blocks:

````
```
fn this_is_a_code_block() {
  return;
}
```

```javascript
function thisIsAnotherCodeBlock() {
  return;
}
```
````