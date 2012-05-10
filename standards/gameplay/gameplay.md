# Gameplay

This document contains information about all communication between players during a match.

## Placing the boats

Placing the boats will not be defined here as that information is not exchanged.

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

#### Both players rolled the same dice

If both players got the same random ID, they should just start another round

## Actions

### Shooting

The shooting client should send a action-stanza to the partner:

```xml
<message from="[player_a]" id="[id]" to="[player_b]" type="normal">
  <battleship>
    <action>fire</action>
    <target>
      <x>8</x>
      <y>3</x>
    </target>
  </battleship>
</message>
```

The partner client should answer wheter the opponent hit a ship or not:

```xml
<message from="[player_b]" id="[id]" to="[player_a]" type="normal">
  <battleship>
    <action>fire</action>
    <target>
      <x>8</x>
      <y>3</x>
    </target>
    <result>water</result>
  </battleship>
</message>
```

The ```result``` can be ```water``` or ```ship```. Both clients should display the shots in a suitable way.
