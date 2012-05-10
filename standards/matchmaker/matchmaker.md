# Matchmaker

This document contains information about all communication between the matchmaker Server and the Clients.

## Basics

The matchmaker Server is used to initiate gaming sessions between the users. All Clients in the system who want to participate in the publich matches have to sign in to the matchmaker.

The Server will store all players waiting for a game in a queue as long as they have no matching players. If two matching players are in the queue, the Server has to start a game between that two gamers.

After the game is done, the Server should store statistical information for the players and should build a "Top 10" list of players.

All XMPP-messages should be send with ```type="normal"```, as we have no direct conversation.

## User management

As the project is using XMPP for its communication, we don't have to worry about user management. Every user has its own XMPP-ID and a ressource for the game itself. The XMPP-ID is stored in the queue so it's easy to assign the players.


### XMPP specific details

All game Clients priority should be ```-128```, the smallest priority available in XMPP, to prevent the game gets messages by accident. The resource should be set to ```battleshipme```, but this is not required.

### Anonymous login

If an user has no own XMPP-Account, an anonymous login should be available. The login should be provided by the XMPP-Server serving the matchmaker-account. The name should look like ```anonymous@battleship.me```. As the username is always the same, the Clients must have different ressources. Thus, a resource could look like ```battleshipme_0a7d213d```.

## Queue management

All players without a game partner will be stored in a queue. When two matching partners are found, the system should assign the two players to each other and start a gaming session.

### Queueing a player

The client should send a message to the server, asking for a place in the queue. The stanza should look like

```xml
<message from="[client]" id="[id]" to="[matchmaker]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <queueing action="request" />
  </battleship>
</message>
```

The server will answer with a success code, if the queueing process was successful. The server also should send the queue id to allow the client additional communication and identification.

```xml
<message from="[matchmaker]" id="[id]" to="[client]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <queueing action="success" />
    <queue id="[the queue id]" />
  </battleship>
</message>
```

### Assigning two players

When two matching players are found, the matchmaker should assign the two players. Thus, the matchmaker has to send messages to both players:

```xml
<message from="[matchmaker]" id="[id]" to="[client]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <queueing action="assign" />
    <partner jid="[partners jid]" />
    <match mid="[match id]" />
  </battleship>
</message>
```

The match id should be a random but unique ID. It will be used to assign the statistical data to a match.

The clients should accept the assigning and sending an answer:

```xml
<message from="[client]" id="[id]" to="[matchmaker]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <queueing action="assigned" />
    <partner jid="[partners jid]" />
    <match mid="[match id]" />
  </battleship>
</message>
```

When the clients accepts the assigning, the matchmaker should remove both clients from the query.

## Statistics collecting

After a game is finished, the matchmaker should collect informational ressources. Thus, the client has to send the informations to the matchmaker:

```xml
<message from="[client]" id="[id]" to="[matchmaker]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <result mid="[match id]" winner="[winners jid]" />
  </battleship>
</message>
```

The matchmaker should store that information, but it should be hidden by default. If both players sent the same result, the statistic-entry should be activated and used for further calculations. The server should response to the incoming result-data:

```xml
<message from="[matchmaker]" id="[id]" to="[client]" type="normal">
  <battleship xmlns="http://battleship.me/xmlns/">
    <result status="saved" mid="[match id]" winner="[winners jid]" />
  </battleship>
</message>
```
