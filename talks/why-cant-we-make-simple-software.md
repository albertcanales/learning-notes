[Why Can't We Make Simple Software? - Peter van Hardenberg](https://youtu.be/czzAVuVz7u4)

The talk shows the concept of *Complexity Homeostasis*. The idea that complexity of a program tends to an equilibrium point:

 - If program is below equilibrium, more features are added.
 - When it surpasses equilibrium, refactors are made.

This equilibrium point might depend on a lot of factors: programmer's preferences, skills... Adding more programmers allows for division of complexity. That way, each division has it's own equilibrium and, when considering the system as a whole, the complexity is much higher than was before possible.

This efect of homeostasis has been seen before (although much debated):

  - Deaths on car accidents after the belt introduction. The idea is it introduced more risky behaviour in passengers, thus compensating the benefits.

 > As someone who has been in countries where a seatbelt is rarely used, I strongly disagree that sealtbeltless passengers are more precautious.

  - Coal usage after the invention of more efficient engines. The increase on efficiency also meant an increase in production.

 > The idea of this concept is nice, but I don't think it can be applied to all software. In the one hand, some software may decide to stay purposefully non-complex (see [suckless.org](https://suckless.org/)). On the other, pressure from third parties may delay the refactors for so long that the *equilibrium point* is shifted permanently.

The talk also gives a few recommendations:

  - [Arquitecture of Open Source Software Applications](https://aosabook.org/en/)
    - *Architects look at thousands of buildings during their training. In contrast, most software developers only ever get to know a handful of large programs well and never study the great programs of history. As a result, they repeat one another's mistakes rather than building on one another's successes.*
    - Interviews to OSS mantainers explaining their architectural decisions.
    - Talk recommends especially Lesson 3 (bash)

  - [*Legend of Zelda: Breath of the Wild* Implementation Talk](https://www.youtube.com/watch?v=QyMsF31NdNc)
    - The devs explain how they implemented, among others, the chemistry engine for the game.

  - [Ink and Switch](https://www.inkandswitch.com/)
    - Their research lab focusing.
    - They have very interesing work focusing on VCS for general and structured data.
