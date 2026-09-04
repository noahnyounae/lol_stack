# How lol_stack balances teams

## 1. Riot ranked standing → a number

The Solo/Duo ranked entry of a player is flattened onto a linear scale, so that
one division is worth 100 points and one tier 400:

```
rank_points = 400 * tier_index + 100 * division_index + league_points

tier_index      IRON 0 · BRONZE 1 · SILVER 2 · GOLD 3 · EMERALD 4
                PLATINUM 5 · DIAMOND 6 · MASTER 7 · GRANDMASTER 8 · CHALLENGER 9
division_index  IV 0 · III 1 · II 2 · I 3   (apex tiers have no division: 0)
```

A Gold II player on 40 LP is therefore `400*3 + 100*2 + 40 = 1440`.

Players with no Solo/Duo entry fall back to their Flex entry; players with
neither start at the median rating of the linked members of the server, and are
flagged as provisional in the output so the group knows the split is a guess.

## 2. In-house Elo

Every linked player also carries an internal Elo, seeded at their `rank_points`
on the first `/lol_stack balance` they take part in, then updated after each game
reported with `/lol_stack result`:

```
expected_A = 1 / (1 + 10 ** ((elo_B - elo_A) / 400))
elo_A     += K * (score_A - expected_A)          K = 32, score = 1 win / 0 loss
```

Team Elo is the mean of its five players, and every player on a team receives the
same update. Beating a stronger team therefore moves a rating more than beating a
weaker one, which is the point: it corrects the players whose ranked standing
does not describe how they actually play in-house.

## 3. The blended rating

```
w      = 1 / (1 + games_played / 10)
rating = w * rank_points + (1 - w) * internal_elo
```

A player who has never played an in-house is rated purely on their Riot rank
(`w = 1`). After ten in-houses the two sources weigh the same, and the internal
Elo keeps taking over from there. In-houses are played off-role, with friends, in
a voice channel — ranked standing predicts the outcome less and less as we
accumulate our own evidence.

## 4. The split

Ten players can be split into two teams of five in
`C(10,5) / 2 = 126` distinct ways. That is small enough to enumerate exhaustively,
so the bot evaluates every split and keeps the one minimising

```
cost = |mean_rating(team_A) - mean_rating(team_B)|
```

When enough players have declared a preferred role — inferred from their recent
MATCH-V5 games and their CHAMPION-MASTERY-V4 pool — splits that leave a team
without a viable assignment for the five positions are discarded before the cost
is compared, and the roles are then assigned inside each team.

The bot posts both teams, the average rating of each and the resulting gap. It
proposes; the group is free to ignore it and reroll.
