# MSC-F0004: PK MSC4611 PMP import


## Proposal

introduce new behavior in client.


### PK format:

```
{
  members: [ { /* Object of type PkMember */ } ],
  /* other fields we don't care about*/
}
```

Here's a sample `PkMember`:

```json
{
  "id": "zzwnuo",
  "uuid": "13dcc755-88be-4983-b511-75a0a4efedd7",
  "name": "Alice",
  "display_name": "alice",
  "color": "0fa5c1",
  "birthday": null,
  "pronouns": "she/her",
  "avatar_url": "https://cdn.pluralkit.me/images/dj/lpswiuinwil4ruaxh5cgfupa.webp",
  "webhook_avatar_url": null,
  "banner": null,
  "description": "Alice from Bob System. or whatever",
  "created": "2026-04-03T00:25:00.718119Z",
  "keep_proxy": false,
  "tts": false,
  "autoproxy_enabled": true,
  "message_count": 751,
  "last_message_timestamp": "2026-06-16T18:21:35.930361Z",
  "proxy_tags": [
    {
      "prefix": "a;",
      "suffix": null
    }
  ],
  "privacy": {
    "visibility": "public",
    "name_privacy": "public",
    "description_privacy": "public",
    "banner_privacy": "public",
    "birthday_privacy": "public",
    "pronoun_privacy": "public",
    "avatar_privacy": "public",
    "metadata_privacy": "public",
    "proxy_privacy": "public"
  }
}
```

### Projecting to an MSC4611 format:

Here's a sample member from MSC4611 (with extensions MSC-F0002; MSC4522; MSC4247):

`fi.mau.msc4461.per_message_profiles.v2`

```json
{
  "id": "black_cat",
  "displayname": "🐈‍⬛",
  "avatar_url": "mxc://maunium.net/hgXsKqlmRfpKvCZdUoWDkFQo",
  "eu.she-a.color": {
    "on_dark": "#a761e8",
    "on_light": "#4b167c"
  },
  "io.fsky.nyx.pronouns": [
    {
      "language": "en",
      "summary": "they"
    }
  ],
  "trigger": {
    "net.f0rest.circumfix": [
      {
        "prefix": "[",
        "suffix": "]"
      }
    ],
    "net.f0rest.suffix": [],
    "prefix": [
      "n;"
    ]
  }
}
```

Here's a proposal on how to perform this projection.

1. Generate a new object: `{}`.
2. Give it a random id: `{ id: nanoid() }`.
3. Set `displayname` to PK's `display_name`.
4. Upload the PK avatar to obtain an mxc url, set to `avatar_url`.
5. Name color: pick best option (`on_dark` should be a light color, vice versa, unset other option or set it to a sensible option (e.g. black, white, etc))
6. Pronouns: do the obvious thing
7. Proxy tags: project onto trigger. if a `{prefix, suffix}` pair, project onto circumfix, `{suffix}` -> suffix, etc.
8. Add a new key: `net.f0rest.pkimport`, and set it's value to an object.
  a. Add PK fields:
    - `id`
    - `uuid`
    - `description` (for future usage)

### "Updating" an import

On importing, you must check for PMPs that contain `net.f0rest.pkimport`.
If you encounter these, you must _replace_ this PMP on import with new values where specified,
instead of creating a new PMP.

## Security Considerations

idk
