# Informal MSC-F0003: PMP Extended Profiles

it's very important that PMPs have bios. however PMPs _must_ have the option of being private, aka only discoverable if a user has received a PMP.

## proposal

Cry about not being able to implement this.

## Alternatives Considered

- Link to a ([MSC4201](https://github.com/matrix-org/matrix-spec-proposals/pull/4201)) Profiles-As-Rooms V2 in a PMP.
- Add PMPs to extended profiles, but encrypt them and provide a decryption key
  - (This could still reveal that you _have_ PMPs, and possibly even their identifiers)
- Upload all extended profile data to PMPs directly
  - Upload extended profile data to an attachment, then link that in a PMP
  - Upload extended profile data to pastebin.com, then link that in a PMP
- Add a new event for updating a PMP extended profile
- Add a new endpoint for querying PMP extended profiles
- Add a state event with all PMPs sent by you in a room, and this can contain extended profile data. This would allow you to manage which PMPs are visible, in theory.
  - Not considered because people get mad at me when i propose state events
- Give up
