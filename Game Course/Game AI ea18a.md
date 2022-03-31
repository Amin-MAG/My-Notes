# Game AI

Game AI comes in 2 flavors.

The first and the most common it the agent, which is a virtual character in the game world. It could be enemy, non-playing character, sidekicks, or...

The seconds type is abstract controllers, which can provides the tactical strategies for a strategy game.

1. Sensory data
2. Think
3. Controller 
4. Act

## FSM

Finite State Machines or FSM is one of the techniques used in Game AIs. It’s not scalable and it’s a little complex. It’s better to have many small automata against of having one big automata.

One way to create Synchronized FMS is shared memory (Storing some booleans). Another way is message passing.

## Rule System

Can handle more complex situations than FSM. They can describe group of behaviors for a game (provides global model of behavior).