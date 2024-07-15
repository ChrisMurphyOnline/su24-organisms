# Overview

Consider an electronic world consisting of an _m_ by _n_ grid. Virtual "organisms" can exist on this grid, with an organism able to occupy a cell on the grid. Organisms have energy that can be gained or lost
in a variety of ways. When an organism runs out of energy it dies, and vacates the cell it formerly occupied. An organism can have at most _M_ units of energy. An organism may do one of several
things during a virtual time cycle:
* Move one cell horizontally or vertically in any direction. The world wraps, so that an organism traveling off the right edge of the grid appears on the left edge, and similarly for the top and bottom edges. A move uses some energy.
* Stay put and do nothing. This move uses a small amount of energy.
* Reproduce. An organism can split in two, placing a replica of itself on an adjacent square. Each resultant organism has slightly less than half of the initial energy of the original organism since reproduction costs some energy.

An illegal move (such as trying to move or reproduce onto an occupied square) results in a "stay put" outcome.

There will be food scattered over the grid. One unit of food corresponds to u units of energy. Food reproduces according to the following rules:
* An empty square has a small probability _p_ of having a single unit of food "blow in".
* For cells already containing food, but no organisms, every food unit on a nonempty square has a small probability _q_ of doubling. This doubling is independent of other food units on the cell. So, a
cell with three units of food may have anywhere between three and six units of food on the next cycle. Nevertheless, no cell may have more than _K_ units of food at any one time, due to space
constraints.
* Cells with organisms never obtain additional food. In fact, an organism that is on a cell with food, and which has energy no larger than _M-u_, will consume a unit of food and add _u_ units of energy to its store. If an organism is hungry, it can eat one unit of food per cycle until either the food runs out, or it achieves energy greater than _M-u_.

An organism has an external state that is an integer between 0 and 255. This state may be changed by the organism during the course of the simulation. The external state is visible to other organisms, as described below.

An organism can "see" in the four orthogonal directions. An organism gets information about:
* Whether there is food or not on a neighboring square (but not how much food).
* Whether there is another organism on a neighboring square. If there is, then the external state of the neighboring organism is also available.
* The amount of remaining food on the organism's current cell.
* The amount of energy currently possessed by the organism.
* Values of the simulator parameters _s_, _v_, _u_, _M_, and _K_ (see below), but not _p_, _q_, _m_, or _n_.

In this project, you will implemement the behavior of a species of organism. Since there will be many organisms on the grid simultaneously, each will run a separate instance of the code, i.e. each organism is a separate Java object. Each instance will have access only to the local environment of the organism. Organisms are placed randomly on the grid, and don't know their coordinates. The organism can keep a history of local events for the organism if you think that's useful.

Organisms cannot identify their neighbors. Neighbors may be of the same species (i.e., have the same programmed behavior), or of a different species. This will be important for simulations in which multiple organisms from different groups are placed on the same grid. You may want to use the external state to help in identifying organisms of the same species.

Organisms act one-at-a-time from top-left to bottom-right, row by row. We number the top-left cell as (1,1) and the bottom-right cell as (m,n). That means that the state of the virtual world seen by an organism at (x,y) reflects the situation in which all organisms in positions (x',y') lexicographically less than (x,y) have already made their moves, while organisms in positions lexicographically after (x,y) have not yet moved. This convention allows all operations to happen without any need for resolving conflicts between organisms (for example trying to move to the same cell). However, it leads to some
slightly unintuitive effects:
* An organism that is sensed to the west is usually in its final position for the cycle, while an organism sensed to the east may or may not be in its final position.
* An organism moving east can be sensed from the west, while an organism moving west typically cannot be sensed from the east.

There are several goals for this project:
1. Your organism should be able to survive and replicate in an environment where it is the only kind of organism present. This may not be as easy as it sounds. Overpopulation may lead to consumption of all the food. If _p_ is sufficiently small, extinction may ensue. The goal is to achieve the highest long-term stable population.
2. Your organism will be tested in environments containing other organisms. The goal here is primarily to survive, and secondarily to survive in higher numbers than competing organisms. Can your organisms populate the grid faster than their competitors? (Is that even the right strategy given the possibility of everybody going extinct?) How might you program your organisms to "recognize" different organisms based on their state and/or behavior? If you can distinguish members of your species, how might you behave differently to members of another species? Your goal here is not necessarily to obliterate other species, but to maximize the fraction of the population containing your species.

We will provide a Java program that simulates the environment in which the organisms live. The following summarizes the configuration parameters and their typical values:
* _m/n_: horizontal/vertical size (5 to simulator screen width)
* _p_: probability of spontaneous appearance of food (0.001-0.1)
* _q_: probability of food doubling (0.002-0.2)
* _s_: energy consumed in staying put (1; other parameters scale to this)
* _v_: energy consumed in moving or reproducing (2-20)
* _u_: energy per unit of food (10-500)
* _M_: maximum energy per organism (100-1000)
* _K_: maximum food units per cell (10-50)
