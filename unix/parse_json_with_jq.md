# jq

## neat little cli tool to parse json

https://stedolan.github.io/jq/

### very simple use case, pretty print json on the command line

just pipe the json into jq: 

`curl -s "https://mediacenter.s5.njiuko.com/api/tournaments.json?auth_token=<authentication token>"|jq`

### a more interesting usecase, getting all match_ids for hsv/current_seasons

`curl -s "https://mediacenter.s5.njiuko.com/api/clubs/1/current_seasons/schedule.json?auth_token=<authentication token>"|jq '.["data"]|.[]|.id'`

You could then pipe the result into `|wc -l` to count the matches. 
And afterwards uniq them then count them `|sort -u|wc -l` and compare the numbers. Hopefully they match, otherwise we have a bug :)

### collect all match ids for current bundesliga season

`curl -s "https://mediacenter.s5.njiuko.com/api/tournaments/1/seasons/current_season/schedule.json?auth_token=<authentication token"|jq '.["data"]|.["divisions"]|.[]|.rounds|.[]|.matches|.[]|.id'`

### collect match_id: team vs team

`curl -s "https://mediacenter.s5.njiuko.com/api/tournaments/1/seasons/current_season/schedule.json?auth_token=<authentication token>"|jq '.["data"]|.["divisions"]|.[]|.rounds|.[]|.matches|.[]|{match_id: .id, home_team: .teams[0].name, away_team: .teams[1].name}'`

builds something like: [{match_id: xxx, home_team: "...", away_team: "..."}, {...}]

and you can do much more, jus tpipe it into other stuff :)
