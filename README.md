# Rusty Robot

This is a project for managing my robot through remote communication written in rust. 

### Goals:
1. Allow for abstracted communication between threads, processes, and even machine through abstraction
2. Make clusters easy to configure with basic .ini files
3. Provide an easy abstraction for event based programming



`Fib_gen` flow:
1. Start up(this could be loading configs ect)
2. Connect to master
3. Subscribe to `fib_requests`
4. Enter wait loop

On request callback:
1. Receive packet from `fib_requests`
2. Parse packet and get the number they want
3. Generate the fib number
4. Write the number to `fib_responses`

`need_fib` flow:
1. Start up
2. Connect to master
3. Subscribe to `fib_responses`
4. Publish request to `fib_requests`
5. Wait for response
6. Receive packet from `fib_responses` and print it out

### Components:

#### Master server:
1. Listen for subscriptions and add subscriptions to a list
2. Listen for published data and forward to all subscribers

#### Client:

Each client consists of two parts:
1. Publishers
2. Subscribers

#### Publishers
1. Connect to master
2. send wrapped data(as in `{"topic": "foo", "data": { passed in data }}` allowing invisible adding of needed protocol information
3. return status code

#### Subscribers
1. Connect to master
2. Send subscribe packet
3. Wait for incoming packets
4. On incoming packet call a callback function in a separate thread 


### Future ideas

1. Add internal buffering to the master server through rabbitmq or redis
2. Add keep alive between subscribers and master
	1. It should be the master that sends most likely so that it doesn't get over ran with requests


### Development plan
1. Implement Basic Master
2. Implement Basic Subscriber
3. Implement Basic Publisher
4. Write a cli client for testing
