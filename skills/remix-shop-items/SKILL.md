---
name: remix-shop-items
description: Create, update, delete, and integrate Remix shop items. Use when a game needs Bits items, consumables, one-time unlocks, tier unlocks, store icons, or purchase handling in @remix-gg/sdk code.
---

# Manage Shop Items Workflow

## Overview

This skill guides you through creating, managing, and integrating shop items
for games on the Remix platform.

## Prerequisites

- A game must already exist.
- Reuse `gameId` from `.remix-settings.json` or `.remix-cli.json` when available.
- Use the official CLI or MCP toolchain when possible instead of handwritten HTTP.

## Steps

### 1. Check for Existing Game ID

Read `.remix-settings.json` or `.remix-cli.json` in the current directory. If either exists and contains a `gameId`, use that value.

Only if neither file exists or neither has a `gameId` should you
follow the **upload-game** workflow to create one.

### 2. Manage Shop Items

#### List All Items

Always list current items first so you reuse the right `itemId`, avoid duplicate slugs, and see whether an item is already `PENDING`, `ACTIVE`, or `INACTIVE`.

#### Create an Item

``` 
POST https://api.remix.gg/v1/games/{gameId}/items
Authorization: Bearer $REMIX_API_KEY
Content-Type: application/json

{
  "name": "Extra Life",
  "slug": "extra-life",
  "itemType": "CONSUMABLE",
  "bitsCost": 50,
  "description": "Spend one to restore a life after game over.",
  "iconUrl": "https://asset-url.png"
}
```

- `itemType` is one of `CONSUMABLE`, `ONE_TIME`, or `TIER_UNLOCK`
- `bitsCost` is required for `CONSUMABLE` and `ONE_TIME`
- `tier` is required for `TIER_UNLOCK`
- new items start as `PENDING`
- when using MCP, `createShopItem` can auto-generate an icon if `iconUrl` is omitted for `CONSUMABLE` or `ONE_TIME`

#### Update an Item

```
POST https://api.remix.gg/v1/games/{gameId}/items/{itemId}
Authorization: Bearer $REMIX_API_KEY
Content-Type: application/json

{ "bitsCost": 75, "name": "Extra Life Pack" }
```

Accepts a partial body -- only include fields you want to change.

#### Delete an Item

```
DELETE https://api.remix.gg/v1/games/{gameId}/items/{itemId}
Authorization: Bearer $REMIX_API_KEY
```

### 3. Integrate Shop Items in Game Code

Add the RemixSDK shop integration to your game:

```js
const sdk = window.RemixSDK
await sdk.ready()

function getConsumableBalance(slug, spentKey) {
  const purchased = sdk.getItemPurchaseCount(slug)
  const spent = Number(sdk.gameState?.[spentKey] || 0)
  return Math.max(0, purchased - spent)
}

async function buyExtraLife() {
  const result = await sdk.purchase({ item: 'extra-life' })
  if (!result.success) showPurchaseError()
}

sdk.onPurchaseComplete(({ success, item }) => {
  if (!success || item !== 'extra-life') return

  const currentState = sdk.gameState || {}
  const spent = Number(currentState.extraLivesSpent || 0)
  sdk.singlePlayer.actions.saveGameState({
    gameState: {
      ...currentState,
      extraLivesSpent: Math.max(0, spent - 1),
    },
  })
})
```

## Wrong Patterns

Do NOT do any of the following:

- **Granting items without listening to `onPurchaseComplete`.** Always wait for
  the callback before giving the player anything. The purchase may fail or be
  cancelled.

- **Using `singlePlayer.actions.purchase(...)` in new code.** Call `sdk.purchase({ item: 'slug' })`.

- **Using `localStorage` to track purchases.** The platform handles purchase
  state. Use `hasItem()`, `getItemPurchaseCount()`, `inventory`, and `shopItems`.

- **Hardcoding prices or item metadata in game state.** Read item metadata from
  the platform and only persist usage counters in `gameState`.

## Tips

- Use `CONSUMABLE` for repeatable purchases, `ONE_TIME` for permanent unlocks,
  and `TIER_UNLOCK` for progression-style items.
- Run `sdk.onPurchaseComplete(...)` even if you also await `sdk.purchase(...)`.
- Use `sdk.getShopItem(slug)` or `sdk.shopItems` when the game needs current
  price or icon metadata.
