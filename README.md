import chess.engine
import berserk
import random

# Define the bot's behavior
async def bot(client, challenge):
    # Accept the challenge
    await client.bots.accept_challenge(challenge['id'])
    
    # Create a board representation
    board = chess.Board()
    
    # Play random moves until the game is over
    while not board.is_game_over():
        # Generate a list of legal moves
        legal_moves = list(board.legal_moves)
        
        # Choose a random move
        move = random.choice(legal_moves)
        
        # Make the move
        board.push(move)
        
        # Send the move to Lichess
        await client.bots.make_move(challenge['id'], move)

# Main function
async def main():
    # Create a Lichess API client
    session = berserk.TokenSession("YOUR_API_TOKEN")
    client = berserk.Client(session)
    
    # Start listening for challenges
    async with client.bots.stream_incoming_events() as events:
        async for event in events:
            if event['type'] == 'challenge':
                # Handle incoming challenge
                await bot(client, event['challenge'])

# Run the main function
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())

<!---
DeveloperLichessbot/DeveloperLichessbot is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
