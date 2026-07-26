---
name: Verify and decode a DSCVR Frame message
description: Decode a signed DSCVR Frame interaction payload into its user, content, button, input, and transaction fields.
api: graphql/dscvr-graphql.graphql
operations: [unpackFrameMessage]
---

# Verify and decode a DSCVR Frame message

When a user interacts with a DSCVR Frame (an inline interactive component), your Frame server
receives a signed message. Use the GraphQL API to unpack and trust it server-side.

- Endpoint: `https://api.dscvr.one/graphql` (POST, `content-type: application/json`)
- Auth: none.

## Steps

1. Take the raw signed `message` string delivered to your Frame callback.
2. Call **`unpackFrameMessage(message: String!)`** — it returns a non-null `FrameMessage`.
3. Read the decoded fields: `user { id username }`, `content { id body }`, `buttonIndex`, `inputText`, `state`, `url`, `timestamp`, and (for transaction frames) `address`, `transactionId`.
4. Use `buttonIndex` and `inputText` to branch your Frame's response; use `state` to carry per-session context.

## Example

```bash
curl 'https://api.dscvr.one/graphql' -H 'content-type: application/json' \
  --data-raw '{"query":"query($m:String!){ unpackFrameMessage(message:$m){ user{ username } buttonIndex inputText state } }","variables":{"m":"<signed-frame-message>"}}'
```

## Notes

- Decode server-side rather than trusting client-supplied fields.
- Frames follow the Open Frames standard (github.com/dscvr-one/open-frames-standard).
- See components/dscvr-components.yml for the Canvas/Frames surface.
