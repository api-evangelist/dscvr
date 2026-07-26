---
name: Inspect a DSCVR portal and its content
description: Resolve a DSCVR portal (community) by slug or id and read a content item (post or comment) with its creator and portal.
api: graphql/dscvr-graphql.graphql
operations: [portalBySlug, portalById, content]
---

# Inspect a DSCVR portal and its content

Use the DSCVR public GraphQL API to read portals (token-gated communities) and individual content.

- Endpoint: `https://api.dscvr.one/graphql` (POST, `content-type: application/json`)
- Auth: none.

## Steps

1. Resolve a portal with **`portalBySlug(slug: String!)`** (human slug) or **`portalById(id: PortalId!)`**.
2. Read portal fields: `id`, `name`, `slug`, `description`, `memberCount`, `postCount`, `isNsfw`, `createdAt`, `iconUrl`, `coverPhoto`, `owner { username }`, `roles { name }`.
3. Check membership with the field arg `isMember(id: DscvrId!)`.
4. Resolve a single content item with **`content(id: ContentId!)`** and read `body`, `contentType` (POST or COMMENT), `creator { username }`, and `portal { name slug }`.
5. Reaction/comment state is field-level: `userReaction(userId:)`, `hasUserCommented(userId:)`.

## Example

```bash
curl 'https://api.dscvr.one/graphql' -H 'content-type: application/json' \
  --data-raw '{"query":"{ portalBySlug(slug:\"dscvr\"){ name memberCount postCount owner{ username } roles{ name } } }"}'
```

## Notes

- Read-only and idempotent. Unknown slug/id resolves to `null` data.
- See data-model/dscvr-data-model.yml for the User/Portal/Content relationship graph.
