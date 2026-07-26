---
name: Look up a DSCVR user profile and social graph
description: Resolve a DSCVR user by username or id and read their social-graph stats, points, streak, and connected wallets.
api: graphql/dscvr-graphql.graphql
operations: [userByName, user]
---

# Look up a DSCVR user profile

Use the DSCVR public GraphQL API to resolve a user and read their SocialFi profile.

- Endpoint: `https://api.dscvr.one/graphql` (POST, `content-type: application/json`)
- Auth: none — queries run with logged-out-user permissions.

## Steps

1. If you have a username, call **`userByName(name: String!)`**. If you have a `DscvrId`, call **`user(id: DscvrId!)`**.
2. Select the profile fields you need: `id`, `username`, `bio`, `followerCount`, `followingCount`, `postCount`, `dscvrPoints`, `iconUrl`, `createdAt`.
3. For gamification, read `streak { dayCount multiplierCount }`.
4. For on-chain identity, read `wallets { address isPrimary walletType walletChainType }`.
5. Relationship checks are field-level and take another user's id: `isFollower(userId:)`, `isFollowing(userId:)`, `isPortalMember(portalId:)`.

## Example

```bash
curl 'https://api.dscvr.one/graphql' -H 'content-type: application/json' \
  --data-raw '{"query":"{ userByName(name:\"development\"){ id username followerCount dscvrPoints streak{ dayCount } wallets{ address walletChainType } } }"}'
```

## Notes

- Read-only and idempotent; no rate-limit headers documented (see conventions/dscvr-conventions.yml).
- Errors arrive in a top-level GraphQL `errors[]` array; a missing user resolves to `null` data, not an error.
- Disable client-side caching for real-time accuracy.
