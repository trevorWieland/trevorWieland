# Pokemon-Playing AI

This project is currently my hobby, as I've been working on some variant of this project off and on for the past two years. 
I've finally made actual progress, once I decided to map everything out from a data-reliability perspective, which enabled me to design around the problems facing similar projects.

Pokemon is a complicated, and at this point, old game. There are over 1000 species, with 18 types, hundreds of moves, abilities, and conditions, etc..
When the data is this categorical, you run into the `p >> n` problem, with matters becoming worse due to the fact that certain combinations may never show up in hundreds of thousands of simulated random games, but still being technically valid combinations.
This means that traditional feature selection techniques are ill-advised, as dropping any columns may lead to the model being unable to handle edge cases that human players can deal with.

What caused my most recent round of inspiration was the decision to forego data-categorization for the machine learning model altogether, using a text-embedding approach instead.
I do still use data-categorization as part of the game-state interpretation and action-availability pipelines, but when it comes to use a model to make a decision, I instead use a model trained to read json-formatted battle-state structures as text.

The project is split across two repos, `poketypes` which provides the data-layer anbd label-consistency, as well as general text-cleaning functions, and `pokesage`, which implements the game-connector logic and machine learning training loops.

## Links

`poketypes`:

- [repo](https://github.com/trevorWieland/poketypes)
- [docs](https://trevorwieland.github.io/poketypes)

`pokesage`:

- [repo](https://github.com/trevorWieland/pokesage)
- [docs](https://trevorwieland.github.io/pokesage)