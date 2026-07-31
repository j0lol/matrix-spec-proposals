# Informal MSC-F0001: PMP Assocations

users have pmps. users want a 'default' pmp for a room and accountwide.


## proposal

add an account key

`m.pmpassociation`

```json
{
    "type": "m.pmpassociation",
    "content": {
        "associations": {
            "account": {
                "profileId": "cat"
            },
            "room": {
                "!foo:example.com": {
                    "profileId": "dog",
                    "useFallback": false
                },
                "!bar:example.com": {
                    "profileId": "dog",
                },
                "!baz:example.com": {
                    "useFallback": true
                }
            }
        }
    }
}
```

- the profile ids are from msc4461
- use fallback designates whether the data-mx-fallback key will be attached to PMPs. 

## Potential issues

idk

## Alternatives

idk

## Security considerations

idk

## Unstable prefix

`net.f0rest.pmpassociation`

## Dependencies

- msc4461 storing pmps for users
- msc4144 pmps (obv)

