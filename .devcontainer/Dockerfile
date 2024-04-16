FROM mcr.microsoft.com/devcontainers/anaconda:0-3

# Set the working directory in the container
WORKDIR /chess_transformer_mothership

# Install chess-nanoGPT Python dependencies
COPY chess-nanoGPT/requirements.txt .
RUN pip install -r requirements.txt

# Copy lichess-bot Python dependencies
COPY lichess-bot/requirements.txt .
RUN pip install -r requirements.txt
