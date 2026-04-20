[The Soul of Erlang and Elixir - Sasa Juric - GOTO 2019](https://www.youtube.com/watch?v=JvBT4XBdoUE)

BEAM: Runtime Layer
Languages can be seen as interfaces to BEAM

Process: sequential program (no internal concurrency)

spawn: invokes another completely independent

Processes can send messages to each other. Similar idea to microservices, but way lighter (no HTTP, serialization, etc)

BEAM runs in a single OS process
With some OS threads inside, each one is a scheduler
The scheduler takes a process from the queue, runs it for a while and returns it. Again and again

### First demo

Around 8:00 he shows a very interesting demo about the scalability capabilities of Erlang
One process for each browser connection
The calculation is done in another process

Even if a calculation process fails, the rest of processes are unaffected
Processes can get notified on failure of someone else, avoiding silent errors

About BEAM scheduling:

- Very frequent context-switching
- "It promotes the progress of the system as a whole at the expense on efficiency on any single process"
- One process may be resource bounded or even fail, but it will not affect the performance of the rest of the processes in any way

Debugging:

- IEX: Interactive Elixir
- Reductions: Equivalent to number of instructions executed
- One can interact even with a process running an infinite loop

### Second Demo

- Processes automatically distribute within the nodes
- In case of node failure, its jobs are automatically recreated into other available nodes

- Dependencies (libraries) in BEAM may not only add code, but also running processes
- In BEAM-based languages there is much less need for supportive tooling as it is already included
- This lowers the technical barrier as the same tool is used for what would normally require a bunch of them, simplifies onboarding
- He calls this _Technical Uniformity_, resulting in simpler development, testing, deployment, and maintenance
