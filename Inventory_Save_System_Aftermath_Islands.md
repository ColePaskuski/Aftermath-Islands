# Aftermath Islands Metaverse Inventory Save System

This document will go through the save system for an inventory system for the Aftermath Islands Metaverse project. 

## Save System Overview:
I designed and created a save system that utilizes a WebSocket Server and a backend database to save player data. The WebSocket Server serves as the communication channel, facilitating the sending and receiving of messages triggered by player actions in the game. For instance, when a player moves an item in their inventory, a message containing the relevant item variables is sent to the WebSocket. This message is then sent to the backend database, where the variables are stored for each player. This approach was specifically chosen for an MMO game with a large player base, as it allows developers to make necessary updates to the game without the need for rebuilding. This method also provides added security measures by reducing the risk of item duplication and cheating, as the database consistently maintains an accurate record of each player's inventory.

The WebSocket is implemented as an Actor Component that can be easily attached to any Blueprint requiring a connection to the WebSocket. The decision to use an actor component was based on the advantages it offers. Firstly, it promotes modular coding by enabling the reuse of the same component across different Blueprints. This enhances code organization and maintenance. 

The process and Blueprints for the WebSocket are below

### Websocket Connected Blueprint

![Alt text](WebSocket_Created.png)

### Websocket Created Blueprint

![Alt text](WebSocket_Connected.png)

### Websocket Events Blueprint

![Alt text](WebSocket_Events.png)

### Websocket Disconnected Blueprint

![Alt text](WebSocket_CrashedorError.png)