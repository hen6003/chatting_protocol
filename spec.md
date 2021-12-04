# Specification

## Logging in

Client sends a username and password, seperated by a `ETB`, and ended by a `EOT`.  
Server **must** then respond with either:
* `AWK` if the details were correct and the user has been logged in
* `NAK` if the password was incorrect
* `CAN` if the username didn't exist

### Example transmission

Client: username`ETB`password`EOT`  
Server: `AWK`**or**`NAK`**or**`CAN`  

## Creating account

To create an account send the same data as for logging in but with a preceding `ACK`.  
Server **must** then respond with either:
* `AWK` if the details were accepted and the user has been logged in
* `NAK`reason`EOT` if the details were not accepted, the reason **should** be displayed to the user  

### Example transmission

Client: `ACK`username`ETB`password`EOT`  
Server: `NAK`password insecure`EOT`  
**or**  
Client: `ACK`username`ETB`password`EOT`  
Server: `ACK`  

## Commands

First byte of message must be a command.  
`ETB` ends a command.  
A next command can then be started (will be part of the same message).  
`EOT` ends the message, sending it.  
`CAN` at any point ends a message (not sending it).  

### Commands

|Character|Name|Description|Notes|
|---------|----|-----------|-----|
|>|Quote|Quote another message||
|!|Announce|Announce an important message|May be restricted|
|@|Ping|Notify the user with that name||
|#|Select channel|Send the message in the channel named||
|"|Message|Normal message content||

### Example transmission

\#test`ETB`>Hi`ETB`@arnold`ETB`"Hello`EOT`

Does the following:
1. Selects channel "test"
2. Replys to a "Hi"
3. Mentions someone called "arnold"
3. And says "Hello"

## Notes
All transmissions **must** be in UTF-8.  
