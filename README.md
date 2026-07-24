# titans-soccer-sub-tracker
Sub tracker for soccer team.  
Soccer Sub Tracker — README

A pitch-side app for running substitutions fairly during a game. Tracks playing time per player, handles the two-goalie full-half rule, keeps a sensible mix of defenders and attackers on the field, and reminds you when it's time to sub.

Your roster, roles, and game format are saved on the device (so they carry over week to week) — only the live game clock and minutes reset when you start a new game.

1. Setting up before a game
Roster

Add each player once. For each player, set:

Playing this week — untick anyone who's out.
Primary role and Secondary role (Defender or Attacker) — most kids will have one main position and a backup. These drive the on-field balance logic described below.
This week's goalies

Pick two players marked as playing:

Goalie 1 — in goal for the 1st half, then plays outfield for the entire 2nd half (no rotation, no rest).
Goalie 2 — plays outfield for the entire 1st half (no rotation), then goes in goal for the 2nd half.

Both goalies are effectively on the field the whole game — they never sit.

Starting lineup

Once both goalies are set, tap 🎲 Shuffle starting lineup to randomly pick who starts among the remaining players. The shuffle always guarantees the starting XI includes at least:

one player whose primary role is Defender
one player whose secondary role is Defender
one player whose primary role is Attacker
one player whose secondary role is Attacker

(The two goalies' roles don't count toward this — the guarantee is about the players actually rotating through the outfield spots.) If you don't shuffle, players start in the order they were added.

Changing the roster, roles, playing status, goalies, or field size clears the shuffle, since it's no longer valid — just tap it again.

Game format
Half length (minutes) — same both halves.
Players on field (incl. goalie) — set this to match whatever format you're playing (5-a-side, 7-a-side, 9-a-side, etc). Everything else scales off this number.
Sub window is calculated automatically (see logic below) and shown for reference — you don't set it directly.
2. During the game
The pitch view

Shows:

The current goalkeeper (full half, no sub).
The current outfield anchor — the other goalie, playing outfield the whole half, also no sub.
The rotating outfield spots — however many are left once you take out the keeper and the anchor (e.g. 7-a-side = 1 keeper + 1 anchor + 5 rotating).
The bench

Ranked by least time played first. When you tap Make substitution, every eligible bench player comes on at once (not one at a time), swapping out the rotating players who've had the most time — so a full-bench sub happens in one tap rather than several.

A bench player is skipped (greyed out, "Hit fair share") if they've already reached their fair-share cap for the game (see below).

Role safety check

If applying the suggested substitution would leave the pitch with no primary defender and/or no primary attacker, you'll get a warning before it goes through:

"No primary defender left on the pitch. Are you sure you want to make this substitution?"

You can proceed anyway or cancel — the app won't silently rearrange the swap for you.

Sub alerts

When the sub window has elapsed, you'll get a pulsing red banner ("🔄 Time to substitute"), a phone vibration, and a buzzer sound — tap the banner to dismiss it. This resets once you actually make a substitution.

Halftime

Tap Halftime — swap goalies once the 1st half clock hits 00:00. This:

Resets the clock to 00:00 for the 2nd half.
Swaps the two goalies' roles (keeper ↔ outfield anchor).
Reseeds the rotating outfield spots for the new half.

Then tap Start on the clock to run the 2nd half.

End of game

The clock stops accruing playing time and auto-pauses exactly at 00:00 — it won't run past the half length.

Screen staying awake

While the clock is running, the app requests a screen wake lock so your phone won't lock mid-game. If it does get interrupted (e.g. you press the power button), the app recalculates the correct elapsed time the moment you come back, so no time is lost.

Playing-time leaderboard

A live, ranked list of every player (goalies included) by total minutes played, with their current role tag — a quick way to eyeball fairness at any point.

New game vs. End game
New game — resets the clock and everyone's minutes to zero, keeps the same roster and goalies. Use this for your next game with the same squad.
End game — clears the live session entirely and returns you to setup. Roster stays saved either way.
3. The fairness logic, explained
Fair share target

For the rotating players (everyone except the two goalies), the app works out an equal split of the available rotating time across the whole game:

fair share (per player) = (rotating spots × half length × 2) ÷ number of rotating players

Example: 7-a-side (5 rotating spots), 20-minute halves, 8 rotating players → (5 × 20 × 2) ÷ 8 = 25 minutes each over the full game.

Tolerance

Rather than capping a player the instant they hit their exact fair share, the app allows ±5 minutes either side of that target before treating them as "capped". This avoids constant marginal subs over a few seconds' difference.

Sub window

The interval suggested between substitutions is also calculated automatically, based on the whole game (not just one half) — so it doesn't over-suggest subs:

sub window = (half length × 2) ÷ number of rotating players   (min 2 minutes)

Fewer rotating players (small bench) → longer window between subs. More rotating players → shorter window, since there's more churn needed to keep things fair.

Final 5 minutes

In the last 5 minutes of the 2nd half, the app stops suggesting subs (no banner, no buzzer, no highlighted swap) unless someone's actual playing time has drifted outside the ±5 minute tolerance band — in which case it still flags it, since that's a real imbalance worth fixing even late in the game.

Role balance

Every player has a primary and secondary role. The starting lineup shuffle guarantees basic coverage of all four (primary/secondary × defender/attacker) among the outfield starters. During the game, the app doesn't auto-rearrange subs to preserve this — instead it warns you if a suggested swap would leave the pitch without a primary defender or primary attacker, so you make the final call.

4. Installing on your phone

The app is a single self-contained HTML file (including its own icon). Typical setup: host it (e.g. GitHub Pages) and open the link in Safari, then Share → Add to Home Screen. It'll open full-screen with its own icon and no browser bar.
