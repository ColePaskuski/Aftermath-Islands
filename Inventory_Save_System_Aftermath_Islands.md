# Aftermath Islands Metaverse Inventory Save System

This document will go through the save system for an inventory system for the Aftermath Islands Metaverse project. 

## Save System Overview:
I designed and created a save system that utilizes a WebSocket Server and a backend database to save player data. The WebSocket Server serves as the communication channel, facilitating the sending and receiving of messages triggered by player actions in the game. For instance, when a player moves an item in their inventory, a message containing the relevant item variables is sent to the WebSocket. This message is then sent to the backend database, where the variables are stored for each player. This approach was specifically chosen for an MMO game with a large player base, as it allows developers to make necessary updates to the game without the need for rebuilding. This method also provides added security measures by reducing the risk of item duplication and cheating, as the database consistently maintains an accurate record of each player's inventory.

The WebSocket is implemented as an Actor Component that can be easily attached to any Blueprint requiring a connection to the WebSocket. The decision to use an actor component was based on the advantages it offers. Firstly, it promotes modular coding by enabling the reuse of the same component across different Blueprints. This enhances code organization and maintenance. 

The process and Blueprints for the WebSocket are below, this is all inside of one blueprint.

### Websocket Created Blueprint

The Clubhouse level in Aftermath Islands uses three connected WebSocket’s to use the inventory system (these include Vendors, Containers, and Players) Each of these WebSocket’s is connected to its own WebSocket Server on the same Port. These custom events are triggered upon the creation/spawning of the respective Actor.

![Alt text](WebSocket_Created.png)

### Websocket Connection Blueprint

Upon creation, each WebSocket is immediately connected and can send and receive messages. Sent messages are triggered when a particular action is executed, such as when a player moves an item. 

![Alt text](WebSocket_Connected.png)

### Websocket Events Blueprint

Received messages are based on the JSON message received from the backend. For example, if the JSON string reads "inventory,” the WebSocket creates the relevant items in the player's inventory.

![Alt text](WebSocket_Events.png)

### Websocket Disconnected Blueprint

In the event that the WebSocket server crashes, all players will be disconnected from the game to prevent cheating. Additionally, the player will not be able to use their inventory until the WebSocket server is restored.

![Alt text](WebSocket_CrashedorError.png)