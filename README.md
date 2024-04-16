# chess_transformer_mothership

To get datasets:

```
git lfs clone https://huggingface.co/datasets/ezipe/adam-chess-data
git lfs clone https://huggingface.co/datasets/ezipe/lichess_2023_janoct_shards/
```

This holds all the forks of all code here:
https://adamkarvonen.github.io/machine_learning/2024/01/03/chess-world-models.html

`./chess_gpt_eval` holds evaluation scripts for playing gpt3.5-turbo-instruct against stockfish and gpt-4. 
`./chess_llm_interpretability` trains and runs linear probes to predict the board and pieces on it from hidden activations within the transformer.
`./chess-nanoGPT` contains code for training a chess nanoGPT from scratch (25M or 50M params).

dataset: https://huggingface.co/datasets/adamkarvonen/chess_games/tree/main

models: https://huggingface.co/adamkarvonen/chess_llms/tree/main

training logs: https://wandb.ai/adam-karvonen/chess-gpt-batch/reports/Chess-world-models-wandb-runs--Vmlldzo2NDIxNzk4?accessToken=0hp0mi9iruo0328wqwl2dycvegt0rw3jkkyw4aqz56yvaiibaetuxvodb9a22ozp

All SECRETS needed for github actions workflow:
- ${{ secrets.DOCKER_HUB_PASSWORD }} : This is the password to your dockerhub where you want to store yourdocker images.
- ${{ secrets.LICHESS_BOT_TOKEN }}: Once you make a lichess bot account and obtain a the API key (see README.md in /lichess-bot), store it as a secret.
- ${{ secrets.WANDB_AUTH_API_KEY }}: Go to https://wandb.ai/authorize while logged into your WANDB account and copy the key down as a secret.
- ${{ secrets.SSH_PRIVATE_KEY }}: You need to have an ssh key associated with your github account. NOTE: This does not go under the repository's secrets.

summary:

- Adam Karvonen studied the application of language models to playing chess, building on previous ML research, including gpt-3.5-turbo-instruct's ability to play chess at an 1800 Elo level, and Kenneth Li's work on Emergent World Representations.
- Karvonen hypothesized that the poor performance of Othello-GPT trained on human-played games was due to insufficient data. To test this, Karvonen trained a 50 million parameter GPT on 5 million chess games, using both synthetic superhuman bot games and 16 million real games from the Lichess database.
- The resulting model could predict the next move in a game with high accuracy and play chess at approximately 1300 Elo after only a day of training on 4 RTX 3090 GPUs. This model, which was never given explicit board states or rules, learned a variety of chess concepts and could estimate latent variables like player Elo ratings, emphasizing that a large language model can indeed learn chess from a large dataset of games.
- The probes were very accurate (99.2%) in classifying each square as one of 13 classes (blank, or containing a white or black pawn, rook, bishop, knight, king, or queen), suggesting that the model indeed formed an internal representation of the board.
- To visualize how the model was tracking the board state, Karvonen created visual heat maps to show the probe's confidence in the presence of specific pieces at given squares. These heat maps confirmed the model's strong internal representation of the board, clearly showing its confidence level in the placement of pieces, indicated by gradients in the probe's output.
- Moreover, Karvonen investigated whether the model could infer the skill level of the players, a latent variable, by designing probes to predict player Elo based on model activations during mid-game. The linear probe accuracy was competing with that of a randomly initialized model, but when tasked to classify players as below 1550 or above 2050 Elo, the probe performed significantly better, indicating that the model could indeed estimate player skill level to some degree.
- He suggested that his findings indicated that large neural networks, when trained to predict the next tokens in a dataset, could pick up underlying patterns and form internal representations about the data they're predicting, as seen in past
