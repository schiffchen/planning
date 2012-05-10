# Gameplay

This document contains information about all communication between players during a match.

## Starting a game

### The Dice Roll

Both clients should roll a dice from 1 to 6 and send the result to the partner. The player with the bigger number is allowed to start the game.

#### Example

Player B has a 4:

```xml
<message from="[player_a]" id="[id]" to="[player_b]" type="normal">
  <battleship>
    <action>starter</action>
    <dice>4</dice>
  </battleship>
</message>
```

Player B has a 5:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <action>starter</action>
    <dice>5</dice>
  </battleship>
</message>
```

In that example, player B is allowed to start the game

#### Both players have the same dice

If both players got the same random ID, they should just start another round
