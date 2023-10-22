# fantasy-football-schedule-analysis

You know how sometimes in fantasy football you have a good week but happen to be matched up against the week's highest scorer, so you lose? Or how sometimes you have a terrible week but happen to be facing someone who had an even worse week, so you still win? Of course you do! Is that fun? I guess depends on which side of that you're on, but what's _always_ fun is quantifying everyone's schedule luck.

If we assume everyone tries to maximize points and isn't making specific roster decisions based on who they're playing that week, we can see what the standings would look like with a different schedule. Of course, this also means we can look at _all_ the possible schedules (or just a sample of possible schedules if the number of teams is small enough).

## Schedule Luck Theory
Whether it's set manually or determined by the order in which team owners sign up, any fantasy football schedule can be thought of as different slots that match up against each other. With four teams, it might look like this:

|Week 1|Week 2|Week 3|Week 4|Week 5|
| :---: | :---: | :---: | :---: | :---: |
|A vs. B|A vs. C|A vs. D|A vs. B|A vs. D|
|C vs. D|B vs. D|B vs. C|C vs. D|B vs. C|

Standings are heavily dependent on who ends up in each team slot. Maybe if you're Team A, you're 4-1 through week 5, but if you were Team B with different opponents each week, maybe you'd be 1-4. Or maybe you'd be 5-0! Or maybe 0-5!

A league with `n` teams has `n!` different possible schedules. With 8 teams, there are roughly 40k different schedule arrangements and it's not impractical to look at them all. For more teams than that, it's better to take a sample of schedule scenarios to estimate. For example, with 12 teams, there are roughly 479 million different schedule arrangements, but I've found that a sample of 100k random arrangements provides a reasonable tradeoff between variability and the amount of time it takes for the process to run.

By iterating through the possible schedule arrangements, we can calculate an expected average number of wins a team should have given a random schedule arrangement. Comparing that number to a team's actual number of wins gives a sense of how (un)fortunate they've been in a season because of the particular actual schedule in use. Because it's fun, we can keep track of the maximum and minimum wins for each team too. If you haven't ever been the highest or lowest scoring team in your league, it's likely that there are some schedule arrangements where you'd be undefeated and others where you'd be unfeated and completely winless. See? You're not _that_ unlucky! (Probably.)

## Usage
Things you'll need:
- Cookie info: When logged in to your ESPN fantasy account, you'll need to grab values for your `swid` and `espn_s2` browser cookies. Doing this in the same browser where you're running the notebook will ensure the code doesn't have to do any kind of further authentication. This is important because the code doesn't currently do that anyway. Store these cookie IDs in `credentials.yaml`. (There's a `credentials.yaml.sample` file you can rename and edit.)
- League ID: This is the numeric value after `leagueId=` in any URL related to your league. Update the `LEAGUE_ID` constant with your league's ID.

After you've made those updates, running all the cells in the notebook will do the following:
- Grab your league info from ESPN
- Generate the schedule (the real version of the sample outlined above)
- Iterate through different schedule arrangements. The default number of schedule arrangements is 100000, but you can adjust this to whatever you'd like.
- Print a table of the results
- Create a luck chart showing the difference between actual and expected wins
- Create a season summary chart showing actual wins, expected wins, maximum wins, and minimum wins.

Enjoy!

## Caveats
- This repository currently only works with data from ESPN leagues.
- I've only ever tried this analysis with leagues that have an even number of teams. I don't know what happens with odd numbers. I'm sure the methodology is still relevant but I don't know what the odd-teams data looks like and thus don't know how it'll work with the code.
- It'd probably be interesting to look at the distribution of a team's expected wins to see how common or extreme the actual schedule arrangement has turned out to be, beyond just comparing actual wins to average expected wins. I haven't done this yet though.
