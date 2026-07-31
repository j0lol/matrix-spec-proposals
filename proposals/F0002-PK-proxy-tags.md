# Informal MSC-F0002: PMP PK Proxy tags

pluralkit has these things called proxy tags, which are like `[[text]]`. at first this looks like a find-and-replace problem, but under the hood pk turns this into prefix/suffix (maybe, it seems, from their export format, but who knows.)

we have some msc work here. [msc4461](https://github.com/matrix-org/matrix-spec-proposals/pull/4461) adds 'prefixes' which do what you think they would do. in pk land this looks like `f;text` -> foo: "text".

## proposal

extend `m.per_message_profiles`.

```json
{
  "type": "m.per_message_profiles",
  "content": {
    "profiles": [
      {
        "id": "cat",
        "displayname": "Cat 🐈️",
        "trigger": {
          "prefix": ["meow ", "cat: "],
          "suffix": ["-cat", "-C"]
        }
      },
      {
        "id": "black_cat",
        "displayname": "🐈‍⬛",
        "avatar_url": "mxc://maunium.net/hgXsKqlmRfpKvCZdUoWDkFQo",
        "trigger": {
          "prefix": ["mrrp:"],
          "circumfix": [{"prefix": "{", "suffix": "}"}]
        }
      }
    ]
  }
}
```

## proxying behavior

there's some stuff that needs to be defined here, specifically about priority.

lumin said this way better here so i'm going to paraphrase that

### circumfix ~~proxies~~ triggers

For an example input `{{{Hello World}}}`, and circumfix proxies `{text}` and `{{text}}`, the tag with the longest content wins. the scoring is quite simple: $\text{len}(p) + \text{len}(s)$. if both are equal, explode the user's computer.

### Suffix-prefix ambiguity

For an example input `fooHello Worldbar`, and proxies `footext` and `textbar`, these proxies are considered to have equal 'weight' and must therefore not be proxied. whether this is presented as an error or simply sent unchanged is left to the discretion of the client.

### Same-class ambiguity

For an example input `HelloWorld`, and proxies `Htext` and `Hellotext`, the latter must be proxied. This is because the second has more specificity/weight/whatever.

### Rough implementation guide

1. Always match circumfix proxies first.
2. If an ambiguity happens, do not continue matching and error/ignore immediately
3. Match suffixes and prefixes together, consider weighting while doing this.

# security considerations etc who cares

# unstable prefix

`net.f0rest.suffix` and `net.f0rest.circumfix` must be used inside the record
