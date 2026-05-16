# WeatherPlugin — README

## Overview
A RocketMod plugin for Unturned that lets players **vote** to change the time of day or clear rain. Any player can start a vote, and it passes when the majority (50%+) of online players vote in favour within 60 seconds. A 240-second cooldown prevents vote spam.

---

## Commands

| Command | Aliases | Description | Permission |
|---|---|---|---|
| `/cv d` | `/votetime d` | Vote to set time to **Day** | `weather.cv` |
| `/cv n` | `/votetime n` | Vote to set time to **Night** | `weather.cv` |
| `/cv nr` | `/votetime nr` | Vote to clear **Rain** | `weather.cv` |
| `/cv` | — | Show active vote status | `weather.cv` |

---

## How Voting Works

**Starting a vote:**
```
Player types:  /cv d
Server says:   [Vote] PlayerName started a vote: Set Day!
               Type /cv d to vote. Need 3/5 votes. (60s)
```

**Other players vote:**
```
Player2 types: /cv d
Server says:   [CV] Vote cast! (2/3)

Player3 types: /cv d
Server says:   [Vote] Vote passed! Applying: Day.
```

**If vote times out (60 seconds, not enough votes):**
```
Server says:   [Vote] Vote for 'Day' failed — not enough votes (1/3 needed).
```

**If you try to start another vote too soon:**
```
Server says:   [CV] You must wait 187s before starting another vote.
```

---

## Vote Rules

- **Majority required** — votes needed = ⌈(online players × 50%)⌉, minimum 1
- **60 second timeout** — if majority not reached, vote is cancelled automatically
- **240 second cooldown** — applies to the player who *starts* a vote (not to players who just cast a vote)
- **Only one vote at a time** — if a vote is already active for a different option, players are told to support the existing vote instead
- **No double voting** — each player can only vote once per active vote

---

## Permissions

Add `weather.cv` to any group that should be able to vote. Typically this goes in the **default** (all players) group.

```xml
<Group>
  <Id>default</Id>
  <Permissions>
    <Permission Cooldown="0">weather.cv</Permission>
  </Permissions>
</Group>
```

> The cooldown is handled in-plugin (240 seconds). The `Cooldown="0"` in Rocket permissions just means Rocket itself won't add an extra cooldown on top.

---

## Dependencies

These DLLs must be present on your server and referenced in `WeatherPlugin.csproj` before building:

| File | Location |
|---|---|
| `Rocket.API.dll` | `Modules\Rocket.Unturned\` |
| `Rocket.Core.dll` | `Modules\Rocket.Unturned\` |
| `Rocket.Unturned.dll` | `Modules\Rocket.Unturned\` |
| `Assembly-CSharp.dll` | `Unturned_Data\Managed\` |
| `UnityEngine.dll` | `Unturned_Data\Managed\` |
| `UnityEngine.CoreModule.dll` | `Unturned_Data\Managed\` |
| `com.rlabrecque.steamworks.net.dll` | `Unturned_Data\Managed\` |

