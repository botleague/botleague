# Bot League

## Open league for AI competition

This repo coordinates an open league where bots are ranked on a set of community 
contributed problems. We provide an ongoing leaderboard of the best bots in the 
world on the problems and challenges submitted. Anyone can submit
a problem and and anyone can submit a bot.

### Conceived for self-driving
The league was conceived specifically for self-driving where industry feedback to academia is limited, despite academia being the major contributor of new algorithms to self-driving.
With Bot League, a self-driving car company can present it's toughest problems directly to the public and 
to academia to compete on. 

### Generality
Generality across problems is a key component of the way the league is structured, 
where bots list a set of problems they can solve. Crucially, these problems
can be within completely different simulators on different machines architectures, etc... 
So if  a self-driving car company wants to test a neural net on its ASIC hardware,
it can pull the docker container containing the weights, compile the net
and evaluate it. The platform is agnostic to both bot and problem implementation.
In this way it's more of an API for comparision and competition of AI.
Bots are ranked and displayed on the  
[leaderboards](https://botleague.io) which are [open source and open data](https://github.com/voyage/leaderboard-generator/data).

We provide a reference implementation of bots and problems via Deepdrive, and [TBA].
 
### How it works

High level: Submit a docker container with your bot and problems it solves, 
and get ranked on botleague.io.

Low level:

Pull requests to [bots](bots) in this repo trigger evaluation on the set of [problems](problems) designated in its bot.json. Pull requests are then evaluated, ranked on the leaderboards, and merged so long as they are in the correct format. Bots include a docker container and optionally writeups and source code that are pulled in and tested by problem evaluators. Anyone can contribute a problem, so long as they support the minimal API. Example problem evaluators are currently Deepdrive and [TBA]. To create a problem within one the problem providers, refer to the docs of that provider.

_Side note_: Reproducibility is core to making progress in research, but we put the onus of that on the problem implementations, and choose to avoid enforcing it here for now in the name of simplicity.

## Bots

### Submission

Submit a pull request to this repo with a bot.json under [bots](bots)/{github-name}/{repo-name} - with a [bot.json](crizcraig/forward-bot/bot.json) to have your agent 
evaluated and ranked on the leaderboards.
 
You must push the docker tag referred to in your bot.json before submitting the pull request.
 

### Problems

A list of problems to test your agent against can be found
in [problems](problems).

# TODO: Link to problems page on leaderboards site to give an idea of what they involve

A bot-league compatible endpoint mush be included in a new problem.

#### Problem Endpoints

Problem endpoints implement a simple API, accepting one request and sending a confirmation then results to liaison.botleague.io.

##### 1. Accept `/eval` POST

`https://your-endpoint/eval/[problem_name]/[seed_value]`, i.e. `/eval/domain_randomization_sanity/9999999999?eval-key=asdf`


Problem endpoints will receive pull request notifications when a bot is submitted along with a secret problem evaluation key (`eval-key`).

One random seed value is also generated that is broadcast to all of the problem evaluators for the bot.

##### 2. Send `/confirm` POST

Problem evaluators must then send a confirmation request with the `eval-key` to `https://liaison.botleague.io/confirm` to verify that botleague indeed initiated the evaluation.

##### 3. Send `results.json` POST

Finally evaluators POST `results.json` to `https://liaison.botleague.io/results` with the `eval-key` to complete the evaluation and to be included on the Bot League leaderboards. An example `results.json` can be found [here](problems/example_results/results.json).


### Challenges

Challenges are sets of problems a bot is tasked with generalizing across. 