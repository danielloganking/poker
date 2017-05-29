# Introduction

This is a C# [.NET Core](https://github.com/dotnet/core) solution to judge 3 Card poker. It _should_ be cross-platform but was developed and tested on Win10-x64 so your mileage may vary.

## Design Goals

This project was designed to solve the problems of 3 Card poker while being extensible and flexible enough to be quickly adapted to solve other pokers. Below are the major namespaces and classes and what they mean to understand how this is achieved.

* `Program.cs`: Console program responsible for I/O
* `Poker.cs`: Defines the game of poker (a set of players with "hands") which is scored in tiers and then re-scored in "tie-breaker" fashion
* `RuleSets` (namespace): implementation of rule builder pattern, should allow the service to be quickly modified to support other pokers. Each "rule set" is a variation of poker rules to judge with.
* `RuleProcessors` (namespace): allow multiple rules to share logic and _cached results_
* `Rules` (namespace): define individual methods for evaluating hands in a round of poker; include initial evaluation and tie-breaking logic.
* `Models` (namespace): concepts fundamental to implementing poker.

Note, there is one significant kludge that may need to be refactored if the architecture were to be improved. Specifically, `Hand` is responsible for determining and knowing if it contains n-of-a-kind cards and what rank those cards are. Ideally this would live in something like a `NumberOfAKindProcessor` but getting that to work with Pair tiebreaking was too difficult to be worth it for initial round of dev.

Another potential refactor would be to create `PlayerHands.cs` to carry `PlayerId` + `Hand` instead of putting the Id onto `Hand` itself.

# Requirements
1. [.NET Core SDK](https://www.microsoft.com/net/download/core)
2. [Python 2.7](https://www.python.org/downloads/) - required to run tests

# Build/Compile

## CLI Publish
1. Set current directory to `.\src` from project root
2. Run `dotnet publish -o ..\publish -c release`

# Run
1. Set current directory to published code location
   * if following above from project root that is `.\publish`
2. Run `dotnet PokerSolver.dll`  

# Test
1. `python .\run_tests "dotnet .\publish\PokerSolver.dll"`
	* **Note**: the path the the published dll is relative to the project root. The above only works if publish directions above are followed.

Note, Tests in the test folder _should_ automatically be run on build.

