# Matchmaker

This document contains information about all communication between the matchmaker server and the clients.

## Basics

The matchmaker server is used to initiate gaming sessions between the users. All clients in the system who want to participate in the publich matches have to sign in to the matchmaker.

The server will store all players waiting for a game in a queue as long as they have no matching players. If two matching players are in the queue, the server has to start a game between that two gamers.

After the game is done, the server should store statistical information for the players and should build a "Top 10" list of players.

## User management

As the project is using XMPP for its communication, we don't have to worry about user management. Every user has its own XMPP-ID and a ressource for the game itself. The XMPP-ID is stored in the queue so it's easy to assign the players.


### XMPP specific details

All game clients priority should be ```-128```, the smallest priority available in XMPP, to prevent the game gets messages by accident. The resource should be set to ```battleshipme```, but this is not required.

### Anonymous login

If an user has no own XMPP-Account, an anonymous login should be available. The login should be provided by the XMPP-Server serving the matchmaker-account. The name should look like ```anonymous@battleship.me```. As the username is always the same, the clients must have different ressources. Thus, a resource could look like ```battleshipme_0a7d213d```.
