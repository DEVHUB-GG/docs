---
description: >-
  Server exports for granting, revoking and reading gym memberships from your
  own resources.
---

# 🔌 Exports

Three server exports, all about memberships. They are the supported way for another resource to hand out a gym pass, take one away, or ask whether a player holds one.

```lua
exports['devhub_gym2']:GrantMembership(source, gymId, days)
exports['devhub_gym2']:RevokeMembership(source, gymId)
exports['devhub_gym2']:GetMembership(source, gymId)
```

{% hint style="warning" %}
**Server side only, and on purpose.** There is no client event for these. One would let any player hand themselves a free pass, because the client would be picking both the gym and the duration. Call them from a server file.
{% endhint %}

***

## <mark style="color:yellow;">**What all three share**</mark>

### The player argument

Every export accepts the player in two forms:

| Form | Type | Works for |
| --- | --- | --- |
| Server id | number | A player who is online. |
| Framework identifier | string | Online **and** offline. The change is written straight to the database and is waiting when they next connect. |

### The gym argument

`gymId` is the zone id: the **ID** printed on the zone card in the creator's [zones.md](gym-creator/zones.md "mention") list. It is the database row and never changes.

Scope follows the same rule as the rest of the membership system:

* A standalone zone holds a pass **for itself**.
* Every location of one business shares **a single pass**. Passing any zone id belonging to that business reaches the same record, so you do not have to know which branch the player used.

### One pass per gym

A player holds at most one membership per gym at a time. That is why `GrantMembership` extends rather than stacks, and why `GetMembership` returns a single entry instead of a list.

***

## <mark style="color:yellow;">**GrantMembership**</mark>

Gives a pass without charging the player.

```lua
local granted, reason = exports['devhub_gym2']:GrantMembership(source, gymId, days)
```

| Parameter | Type | Meaning |
| --- | --- | --- |
| `target` | number \| string | Server id or identifier. |
| `gymId` | number | Zone id. |
| `days` | number | How many days to grant. Must be above zero. |
| `packageId` | number \| nil | Optional fourth argument. Cosmetic only: it marks which package card the Membership tab shows as the one the player holds. Leave it out and the pass still works exactly the same. |

**Returns** `true`, or `false` plus a reason string.

{% hint style="info" %}
**It extends, it does not replace.** Calling it twice with `7` leaves the player with 14 days, counted from whichever is later: their current expiry or now. So a grant to someone who already paid never shortens what they bought.
{% endhint %}

If the player is online, their gym menu is refreshed immediately. There is nothing to relog for.

**Example**, a seven day pass as a job perk:

```lua
RegisterCommand('gymperk', function(source)
    local granted, reason = exports['devhub_gym2']:GrantMembership(source, 1, 7)
    if not granted then
        print(('could not grant the gym pass: %s'):format(reason))
    end
end, true)
```

***

## <mark style="color:yellow;">**RevokeMembership**</mark>

Removes the pass covering this gym, whether it had expired or not.

```lua
local removed, reason = exports['devhub_gym2']:RevokeMembership(source, gymId)
```

| Parameter | Type | Meaning |
| --- | --- | --- |
| `target` | number \| string | Server id or identifier. |
| `gymId` | number | Zone id. |

**Returns** `true`, or `false` plus a reason string. A player who never had a pass for this gym comes back as `false, "player had no membership for this gym"`, so you can tell "removed it" apart from "there was nothing to remove".

***

## <mark style="color:yellow;">**GetMembership**</mark>

Reads the active pass covering this gym.

```lua
local membership = exports['devhub_gym2']:GetMembership(source, gymId)
```

**Returns** `nil` when there is no pass, or the pass has already expired. That is the same answer the gym itself gives at the machine, so a `nil` here means the player would be turned away.

Otherwise it returns:

| Field | Type | Meaning |
| --- | --- | --- |
| `gymId` | number | The zone the pass was bought at. |
| `businessId` | number \| nil | The business it is shared across, `nil` for a standalone zone. |
| `typeId` | number \| nil | The membership package id, when one was recorded. |
| `expiresAt` | number | Unix timestamp when it runs out. |
| `purchasedAt` | number | Unix timestamp of the original purchase. |
| `secondsLeft` | number | Time remaining, already worked out for you. |

This one is read only. Asking about a player who has never trained does not create a database row for them.

**Example**, letting a doorman check the pass:

```lua
local membership = exports['devhub_gym2']:GetMembership(source, 1)

if not membership then
    print('no active pass')
else
    print(('%d days left'):format(math.floor(membership.secondsLeft / 86400)))
end
```

***

## <mark style="color:yellow;">**Why a call fails**</mark>

Every failure comes back as a reason string rather than an error, so a mistake in your resource cannot break the gym.

| Reason | What happened |
| --- | --- |
| `gym system not ready yet` | The gym is still loading its database tables. Called too early in the boot. |
| `unknown gym id` | No zone with that id. Check the **ID** on the zone card. |
| `days must be a positive number` | `GrantMembership` was given `0`, a negative number or something that is not a number. |
| `player not found` | The server id is not online, or the identifier does not resolve. |
| `membership was not stored` | The write was rejected, almost always a `days` value that worked out to zero. |
| `player had no membership for this gym` | `RevokeMembership` found nothing to remove. |

***

## <mark style="color:yellow;">**Looking for hooks instead?**</mark>

Exports are for pushing something **into** the gym. To react to what happens **inside** it, gating a set before it starts or logging one when it ends, use the hooks in [server.lua.md](configuration/server.lua.md "mention").
