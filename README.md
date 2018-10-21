# Architecture

## Server – Two Threads (Server)
        ◦ Thread A – Server-Client (Post-Fork):
            ▪ Creates a socket structure, binds it to a port and starts listening.
            ▪ Gets a connection from a client, forks it and hands the socket to the child and closes the socket in itself.
            ▪ Populates a global list with each client’s IP address, Port Number, Pipes (2 pipes), Process ID.
        ◦ Thread B – Interactivity:
            ▪ Takes input from the server user’s screen in an infinite loop.
            ▪ Allows user to see all the clients (with Active status) in the list.
            ▪ Allows user to kill any of the clients.
            ▪ Allows user to get a list of the process of any of the active clients using the client’s IP address. (uses pipes to send and receive info from client)
            ▪ Allows user to get a list of the processes of all the active clients. (uses pipes to send and receive info from client)
            ▪ Allows user to send a message to any one(or all) of the clients. (uses pipes to send and receive info from client)

## Client Handler (Server Child) – Two Threads(Server)
        ◦ Thread A
            ▪ Talks to the client process using socket.
            ▪ Receives instructions from client using socket.
            ▪ Executes instructions (add,mult,div,sub,run,list,kill,help,etc) and sends the result back to the client process using the socket.
        ◦ Thread B
            ▪ Talks to the server.
            ▪ Is blocked in a read pipe reading from server until server sends a command.
            ▪ Receives command from server – executes it(print message or extract list) and sends the result back to the server using another pipe.



Client Process(Client)
        ◦ Thread A
            ▪ Connects to the server using the Server’s IP address and Port through a socket.
            ▪ Starts an infinite loop and reads from the user screen.
            ▪ Writes it to the socket and goes back to reading.
        ◦ Thread B
            ▪ Reads from the socket.
            ▪ Prints it on the screen.




# How-To Use Commands

## Server
    • list conn		-> get a list of all the connections with serial number, IP address, port, Process ID, Activity Status
    • list process		-> print process-list of all the active clients
    • list process <ip>	-> prints process-list of the client whose ip is given (only if it is active)
    • kill all			-> kills all the currently active clients and update their activity status in the lists
    • exit			-> send SIGTERM to all the clients to kill them (and their processes) and then shut down the server

## Client
    • add x x x		-> adds a given series of numbers
    • sub x x x		-> subtracts a given series of numbers
    • mul x x x		-> multiplies a given series of numbers
    • div x x x		-> divides a given series of numbers
    • run <name>		-> runs the program with the given name
    • kill name <name> 	-> kills the process given by the name (only if the client started it)
    • kill pid <pid>		-> kills the process given given by the pid (only if the client started it)
    • kill all			-> kills all the processes started by the client
    • help			-> lists down all the available commands
    • exit			-> shuts down the client process
    • list			-> lists down only the active processes started by the client with PID, start time, end time, elapsed time, active status
    • list active		-> lists down only the active processes started by the client with PID, start time, elapsed time, active status
