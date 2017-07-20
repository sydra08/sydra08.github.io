---
layout: post
title:  "Tic Tac Toe with AI"
date:   2017-07-20 15:28:39 +0000
---


I spent the past week working through the [Tic Tac Toe with AI lab](https://learn.co/tracks/full-stack-web-dev-with-react/object-oriented-ruby/final-projects/tic-tac-toe-with-ai). The tests helped with getting the board, players and game play setup, but after that I was on my own. The goal was to have three modes of play: 0 players (computer vs computer), 1 player (human vs computer) and 2 player (human vs human). The 2 player mode was the least of my worries - both players were able to see the board so as long as the game functioned properly, which was pretty much complete. The other two modes were not so simple and the main challenge I faced with this was: As a user, I was able to physically see the board, but how was the computer going to do the same? 

The program is consists of 3 classes: `Board`, `Player` and `Game`, none of which are directly related to each other by nature. While each class has it’s own responsibility, they collaborate to formulate a working Tic Tac Toe game. The `Board` class is responsible for storing the data and logic of the game board itself -  what it looks like at any given point in the game, where the pieces are, if a player’s move is valid, etc. The `Player` class handles what the player’s token (“X” or “O”) is and what moves are made. The `Game` class is the heart of the program, it uses the `Board` and `Player` classes for the game play, in addition to containing the basic game logic - whose turn is it?, is the game over?, who won?, etc. 

When a `Game` instance (`game`)  is created, it creates a `Board` instance (`board`)  and two `Player` instances (`player_1` and `player_2`). 

```
class Game
  def initialize(player_1=Players::Human.new("X"), player_2=Players::Human.new("O"), board=Board.new)
	 @player_1 = player_1
	 @player_2 = player_2
	 @board = board
  end
end
```

At this point in time, the `game` is like a one-way mirror: it can see everything inside of the `board`, `player_1` and `player_2` instances that it has created, but no one can see into the `game` instance. 

![](https://docs.google.com/drawings/d/sBkZwf2WGodc0w5Z2fKnFbg/image?w=392&h=266&rev=1&ac=1)

This takes us back to my original challenge: How can the computer “see” the board and understand what is going on in the game? The short answer is that the computer needs to access the specific `Board` instance within the current `Game` instance. To do this, I gave the `Player` instance a `game attr_accessor` that stores an instance of the `Game` class. 

```
class Player
  attr_reader :token
  attr_accessor :game

  def initialize(token)
    @token = token
  end
end
```

When a `Game` instance is created, it adds itself to each of the `Player` instances via the `#game` method. In doing so, the `Player` is then connected to the `Board` via the `Game` and can access the data contained within that instance and use it to employ different strategies.  

```
class Game
  def initialize(player_1=Players::Human.new("X"), player_2=Players::Human.new("O"), board=Board.new)
	 @player_1 = player_1
	 @player_2 = player_2
	 @board = board
	 player_1.game = self
	 player_2.game = self	
  end
end
```







