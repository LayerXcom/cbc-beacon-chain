**This repository is based on https://github.com/ethereum/cbc-casper**

# Casper CBC

A python implementation of a class of ["correct-by-construction" consensus protocols](https://github.com/ethereum/research/tree/master/papers/cbc-consensus). Currently, this includes Casper the Friendly Ghost (a blockchain consensus protocol) and Casper the Friendly Binary Consensus Protocol.

Specifications for these protocols can be found here [here](https://github.com/ethereum/research/tree/master/papers/CasperTFG), but the implementation and the spec may deviate from the spec, as they are still moving targets.

## Requirements
* Python 3
* ubuntu/debian: `sudo apt-get install python3-dev python3-venv python3-tk`
* OSX via Homebrew: `brew install python3`


## Developer Setup
If you would like to hack on cbc-casper or run the simulations yourself, setup your dev environment with:

```
make install
```

NOTE: If you prefer to manage venv explicitly, run a standard venv setup and
pip install using `requirements.txt`

## Run Simulations
### Standard
Standard simulations are marked up for use as follows:

```
make run-[binary | blockchain | concurrent | integer | order | sharding]
```

Each protocol represents consensus on a different data structure.

Optionally, you can add a message passing mode to each protocol. For example,
```
make run-binary MODE=rrob
```
There are currently three message passing modes:

`rand:` each round, some randomly selected validators propagate their most recent message to other randomly selected validators, who then create new messages.

`rrob:` each round, the creator of the last round's block sends it to the next receiver, who then creates a block.

`full:` each round, all validators receive all other validators previous messages, and then all create messages.

By default, a gif and associated images of the simulation will be saved in `graphs/graph_num_0/`. These settings can be modified, along with the number of validators, the number of messages that propagate per round, and the report interval in the `config.ini`.

### Advanced
Advanced simulations can be run with a little command line wizardy.
- First ensure that you are using the virtual environment via: `. venv/bin/activate`
- Simulations can then be run via `casper.py`. The following are example usages:
```
# get help and all options for casper.py
python casper.py --help

# run a simulation with 100 validators and random message propagation
python casper.py --protocol blockchain --msg-mode rand --validators 100

# run a simulation without displaying the viewgraphs, but instead save them and create a GIF
python casper.py --protocol blockchain --msg-mode rand --display false --save true

# run a simulation with 20 validators and 1000 rounds of round robin message propagation,
# reporting every 100 rounds
python casper.py --protocol blockchain --msg-mode rrob --validators 6 --rounds 300 --report-interval 100


```

## Write Simulations

COMING SOON...

## Run Tests
To run all tests:

```
make test
```

To run a specific test, use (or the equivalent for whatever test you wish to run)

```
make test TEST=tests/test_safety_oracle.py
```

To run tests with visualizations:
```
make test-with-reports
```
Note: each view graph must be closed for the tests to continue running.
