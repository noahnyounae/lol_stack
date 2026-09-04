# lol_stack — In-House Team Balancer for Discord

**lol_stack** is a module of a free Discord bot that helps a single private
community of amateur League of Legends players organize fair 5v5 in-house
(custom) games.

Picking teams by hand ends in one stacked side and a 15-minute stomp. lol_stack
splits ten players into two teams of near-equal strength, using each player's
Riot ranked standing combined with an internal Elo built from the results of our
own in-house games.

## Scope

| | |
|---|---|
| Platform | Discord bot (slash commands) |
| Audience | One private Discord server, [NOM DU SERVEUR], ~[NOMBRE] members |
| Region | [EUW / EUNE] |
| Cost | Free. No ads, no monetization, no paid tier, no donations |
| Distribution | Not publicly invitable — the bot runs on a single server |

## Commands

| Command | What it does |
|---|---|
| `/lol_stack link <riot_id>` | Links the caller's own Riot ID (`Name#TAG`) to their Discord account |
| `/lol_stack unlink` | Removes the link and deletes every piece of data we hold about that player |
| `/lol_stack profile [member]` | Shows a linked player's current ranked tier, their internal in-house Elo and a summary of their recent ranked matches |
| `/lol_stack balance` | Takes the ten linked players in the caller's voice channel and posts the two most balanced teams |
| `/lol_stack result <winning team>` | Records the outcome of an in-house game and updates the internal Elo of the twenty involved ratings |
| `/lol_stack leaderboard` | Internal in-house standings of the server |

## How the balancing works

Each player gets a single numeric rating that blends two sources:

1. **Riot ranked standing** — tier, division and LP of the current Solo/Duo
   queue entry, mapped onto a linear scale.
2. **Internal in-house Elo** — a classic Elo rating updated after every in-house
   game reported through `/lol_stack result`.

The blend starts as pure Riot rank for a new player and shifts towards the
internal Elo as they accumulate in-house games, because in-houses are played
with friends, off-role and off-meta, where ranked standing predicts less and
less. The bot then evaluates every way of splitting the ten players into two
teams of five and keeps the split with the smallest rating gap.

The full formula, including how unranked players and role constraints are
handled, is documented in [BALANCING.md](BALANCING.md).

## Riot API usage

| API | Why we call it |
|---|---|
| **ACCOUNT-V1** | Resolve the Riot ID given by the player to a PUUID at link time |
| **SUMMONER-V4** | Resolve the account on the platform the community plays on |
| **LEAGUE-V4** | Ranked entries — the tier/division/LP that seed the balancing rating and are shown by `/lol_stack profile` |
| **MATCH-V5** | Recent ranked matches, used to detect a player's usual role and to display a short recent-form summary |
| **CHAMPION-MASTERY-V4** | Champion pool, used as a hint for role assignment inside a balanced team |

Calls are made only in reaction to a command typed by a member — the bot never
polls the API in the background.

**Caching and rate limits.** Ranked entries are cached for one hour per player,
match history for six hours, and champion mastery for twenty-four hours.
Concurrent requests for the same player are collapsed into a single call. A
`/lol_stack balance` on ten players therefore costs at most ten cached-or-fresh
ranked lookups, and in practice far fewer. `429` responses are honoured with the
`Retry-After` header before any retry, and the bot backs off rather than
queueing indefinitely.

## Data and privacy

We store the Discord ID ↔ PUUID mapping, a short-lived cache of public ranked
data, and the results of our own in-house games. We store no credentials, no
email addresses and no personal data beyond that. `/lol_stack unlink` erases a
player's record immediately.

Full details, including retention and how to request erasure, are in
[PRIVACY.md](PRIVACY.md).

## Compliance

- The bot is free and will stay free. It shows no advertising and sells nothing.
- It provides no in-game advantage and no automation of gameplay: it reads
  public ranked data after the fact, the same way a stats website does.
- It does not interact with the game client, does not modify game files and does
  not scrape unofficial endpoints.
- Displayed data always names the player it belongs to, inside the Discord
  server the request was made in.

## Contact

contact@runiacorp.eu

---

lol_stack isn't endorsed by Riot Games and doesn't reflect the views or opinions
of Riot Games or anyone officially involved in producing or managing Riot Games
properties. Riot Games and all associated properties are trademarks or
registered trademarks of Riot Games, Inc.
