# Privacy Policy — lol_stack

Last updated: [DATE]

lol_stack is a free Discord bot module operated by [TON NOM / RUNIA CORP] for a
single private Discord community. This policy describes exactly what it stores,
why, and how to have it erased.

## What we store

The bot only stores data for members who explicitly ran `/lol_stack link`.

| Data | Why |
|---|---|
| Discord user ID | To know which Discord member a Riot account belongs to |
| Riot PUUID | Stable identifier used for every subsequent Riot API call |
| Riot ID (game name and tag line) | Displayed in command output so members recognise each other |
| Cached ranked entry (tier, division, LP, queue) | Input of the balancing rating; refreshed at most once per hour |
| Cached recent match summary and champion mastery | Role detection and recent-form display; refreshed at most every six / twenty-four hours |
| Internal in-house Elo and in-house game records (participants, winning side, timestamp) | The internal rating, computed from our own games, not from Riot data |

## What we do not store

No email address, no password, no Riot account credentials, no IP address, no
message content, no location data, no payment information. The bot never asks
for a Riot account login and could not use one.

Every piece of Riot-sourced data we hold is public information already visible on
any League of Legends statistics website.

## How it is stored and who sees it

Data is kept in a JSON file on the machine hosting the bot, accessible only to
the bot operator. It is **not** shared with, sold to, or transmitted to any third
party. There is no advertising, no analytics, no tracking and no monetization of
any kind.

Command output is posted in the Discord server the command was run in, and is
visible to the members of that server.

## Retention and erasure

Data is kept as long as the link exists.

- `/lol_stack unlink` deletes the member's mapping, cached Riot data and
  personal rating immediately and permanently.
- Leaving the Discord server, or the bot being removed from it, has the same
  effect: the records are dropped.
- You may also ask for erasure, or for a copy of the data held about you, by
  contacting [TON EMAIL]. We answer within 30 days.

Under the GDPR you have a right of access, rectification, erasure, restriction
and objection over this data. `/lol_stack unlink` satisfies the right to erasure
instantly; for anything else, use the contact address above.

## Children

The bot follows Discord's Terms of Service and is not intended for anyone below
the minimum age required to hold a Discord account.

## Changes

Any change to this policy is published in this document, with the date at the
top updated accordingly, and announced in the Discord server.

## Contact

[TON NOM / RUNIA CORP] — [TON EMAIL]

---

lol_stack isn't endorsed by Riot Games and doesn't reflect the views or opinions
of Riot Games or anyone officially involved in producing or managing Riot Games
properties.
