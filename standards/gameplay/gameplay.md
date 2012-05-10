# Gameplay

This document contains information about all communication between players during a match.

## Placing the boats

Placing the boats will not be defined here as that information is not exchanged.

## Starting a game

### The Dice Roll

After they have placed their boats, both clients should roll a dice from 1 to 6 and send the result to the partner. If both players have submitted their number, both players should have placed their boats already. The player with the bigger number is allowed to start the game.

#### Example

Player B has a 4:

```xml
<message from="[player_a]" id="[id]" to="[player_b]" type="normal">
  <battleship>
    <diceroll dice="4" />
  </battleship>
</message>
```

Player B has a 5:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <diceroll dice="5" />
  </battleship>
</message>
```

In that example, player B is allowed to start the game

#### Both players rolled the same dice

If both players got the same random ID, they should just start another round

## Actions

### Shooting

The shooting client should send a action-stanza to the partner:

```xml
<message from="[player_a]" id="[id]" to="[player_b]" type="normal">
  <battleship>
    <shoot x="8" y="3" />
  </battleship>
</message>
```

The partner client should answer wheter the opponent hit a ship or not:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <shoot x="8" y="3" result="water" />
  </battleship>
</message>
```

The ```result``` can be ```water``` or ```ship```. Both clients should display the shots in a suitable way. If a ship is destroyed, the partner client should provide information about that ship:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <shoot x="8" y="3" result="ship" />
    <ship startx="4" starty="3" endx="8" endy="3" destroyed="true" />
  </battleship>
</message>
```

## Ending a game

When a player has no more ships in the sea, the client has to end the game:

```xml
<message from="[player_a]" id="[id]" to="[player_b]" type="normal">
  <battleship>
    <gamestate state="end" looser="[player_a_jid]" />
  </battleship>
</message>
```

The other client should confirm the ended game with sending the same stanza back again:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <gamestate state="end" looser="[player_a_jid]" />
  </battleship>
</message>
```

The clients should now submit the statistical data to the matchmaker-server, see the matchmaker documentation for details.
