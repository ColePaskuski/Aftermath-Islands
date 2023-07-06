# Aftermath Islands Metaverse Inventory Save System

This document will go through the Save System for the inventory for the Aftermath Islands Metaverse project. Aftermath Islands is owned by Liquid Avatar Technologies and is an open-world multiplayer online game (MMO). Aftermath Islands is playable through a pixel stream which allows the game to be run through a web browser on PC and mobile phones and does not require any download from the player. The game is currently being developed in Unreal Engine 5. The first island (Clubhouse Island) was released to the public as an open beta in June 2023. This project is ongoing, so this document will be updated accordingly.

I was responsible for coding the Save System for the inventory through Blueprints. The WebSocket Server and Backend Database were developed by Aftermath Islands Backend Developer. The inventory system was premade, and I had to rework it to work with a Backend Database and save system. 

## Save System Overview:

The Save System utilizes a WebSocket Server and a Backend Database to save player data. The WebSocket Server will send and receive messages when a certain action is done by the player in the game. For example, when a player moves an item in their inventory, a message containing the relevant item variables (slot ID, name, quantity, etc) is sent to the WebSocket Server. The variables are then formatted into a JSON string which is the message. This message is then sent to the Backend Database. Once the Backend receives the message, the JSON string is broken back down into individual variables and stored in the Database. The variables are stored based on the inventory components Universally Unique Identifier (UUID). 

The WebSocket code is implemented as an Actor Component that can be easily attached to any Blueprint requiring a connection to the WebSocket Server. An Actor Component was chosen since it enables modular coding by the reuse of the same component across several Blueprints. 

## Inventory Component Overview:

Each actor that needs an inventory will have an inventory Actor Component. The Clubhouse Island level has three Blueprints that have an inventory component (Vendors, Crafting Tables and each Player). The inventory for each Player, Vendor and Crafting Table is unique through a UUID. This allows the Backend to easily track the inventory contents for each inventory component in the level. The UUID is set for the player when they log into their Liquid Avatar account when starting the game. The UUID for Vendors and Crafting Tables is set in Unreal Engine. 
##

This approach was chosen because Aftermath Islands will be a large MMO game that will have a large amount of daily active users. Having a Backend Database handle the inventory item data and actions will allow future developers to make necessary updates to the game without the need for rebuilding. For example, if a player needs an item added to their inventory, this can be done through the Backend without needing the item to be placed in the level. This approach also provides added security measures by reducing the risk of item duplication and cheating, as the Database consistently maintains an accurate record of each inventory.

## Websocket's:

The Clubhouse level in Aftermath Islands has three Blueprints that have a WebSocket and an inventory component (these include Vendors, Crafting Tables, and each Player). Each of these WebSocket’s is connected to its own WebSocket Server on the same Port. 

### WebSocket Initialization
 
The custom events are triggered upon the code being executed in the respective Blueprint, which happens at begin play. The events will initialize the creation of each WebSocket for Vendors, Crafting Tables, and each Player.

### Websocket Creation

Upon creation, each WebSocket is immediately connected and can send and receive messages. The custom events are triggered upon the creation/spawning of the respective Blueprint..

![Alt text](WebSocket_Created.png)

### WebSocket Connection 

Upon creation, each WebSocket is immediately connected and can send and receive messages. 

![Alt text](WebSocket_Connected.png)

### WebSocket Sent Messages 

Sent messages are triggered when a particular action is executed, such as when a player moves an item. The code is the same for each the Vendors, Containers, and Players.

![Alt text](WebSocket_Message_Sent.png)

### WebSocket Recieved Messages

Received messages are based on the JSON message received from the backend. For example, if the JSON string reads "inventory" the WebSocket creates the relevant items in the player's inventory. The code is the same for each the Vendors, Containers, and Players.

![Alt text](WebSocket_Events.png)

### WebSocket Message Events

The received messages are executed in the Inventory Component, each message is run through a custom event running on the owning client. 

![Alt text](WebSocket_ReceivedMessages.png)

### WebSocket Disconnection 

In the event that the WebSocket server crashes, all players will be disconnected from the game to prevent cheating. Additionally, the player will not be able to use their inventory until the WebSocket server is restored.

![Alt text](WebSocket_CrashedorError.png)

### See Full WebSocket Blueprint Here: 

https://blueprintue.com/blueprint/egestpq_/ 

## Inventory Componenet:

The inventory component is where code for the sent and received WebSocket messages are executed.  There is a separate message for each action that can be made by any of the Vendors, Containers, and Players. 

### Websocket Sent Message Code:

To ensure synchronization between the game and the database, whenever a player makes changes to their inventory, they receive a message (get inventory message) that removes and recreates all the items in their inventory. See how this process works for the Player below.

![Alt text](GetInventoryMessage.png)

 When the items are created, they are added to an item array, this array contains all the player’s inventory items. See how items are created for the Player below. The code is the same for the Vendors and Containers.

 ![Alt text](CreateItemMessage.png)
 
 Each item will also have a unique identifier (Item ID) that will tell it apart from the other items any of the inventories. This process is the same for Containers and Vendors, each having their own respective arrays. 

### Websocket Sent Message Process:

WebSocket Messages are designed with a common base code to retrieve specific item variables related to the altered item. These item variables include quantity, slot ID, name, etc. The item variables are compared with the variables in the player item array mentioned above to find the correct Item ID for the item. Once the item ID is found, the item variables are converted into individual variables (integer, string, float, etc), which are then transformed into a JSON string containing the item information. This WebSocket message containing the item information is then sent to the backend.

### See WebSocket Sent Messages Blueprint Here: 

https://blueprintue.com/blueprint/3izdf7i5/

### See Items broken Down and Formatted to a JSON string here: 

https://blueprintue.com/blueprint/glq997qx/ 

Once the items are formatted into JSON they are sent to the WebSocket