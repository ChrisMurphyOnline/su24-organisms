# Overview

Consider an electronic world consisting of an m by n grid. Virtual "organisms" can exist on this grid, with an organism able to occupy a cell on the grid. Organisms have energy that can be gained or lost
in a variety of ways. When an organism runs out of energy it dies, and vacates the cell it formerly occupied. An organism can have at most M units of energy. An organism may do one of several
things during a virtual time cycle:
* Move one cell horizontally or vertically in any direction. The world wraps, so that an organism traveling off the right edge of the grid appears on the left edge, and similarly for the top and bottom edges. A move uses some energy.
* Stay put and do nothing. This move uses a small amount of energy.
* Reproduce. An organism can split in two, placing a replica of itself on an adjacent square. Each resultant organism has slightly less than half of the initial energy of the original organism since reproduction costs some energy.

An illegal move (such as trying to move or reproduce onto an occupied square) results in a "stay put"
outcome.

There will be food scattered over the grid. One unit of food corresponds to u units of energy. Food
reproduces according to the following rules:
* An empty square has a small probability p of having a single unit of food "blow in".
* For cells already containing food, but no organisms, every food unit on a nonempty square has a small probability q of doubling. This doubling is independent of other food units on the cell. So, a
cell with three units of food may have anywhere between three and six units of food on the next cycle. Nevertheless, no cell may have more than K units of food at any one time, due to space
constraints.
* Cells with organisms never obtain additional food. (The organisms block the light.) In fact, an organism that is on a cell with food, and which has energy no larger than M-u, will consume a unit
of food and add u units of energy to its store. If an organism is hungry, it can eat one unit of food per cycle until either the food runs out, or it achieves energy greater than M-u.

An organism has an external state that is an integer between 0 and 255. This state may be changed by the organism during the course of the simulation. The external state is visible to other organisms, as described below.

An organism can "see" in the four orthogonal directions. An organism gets information about:
* Whether there is food or not on a neighboring square (but not how much food).
* Whether there is another organism on a neighboring square. If there is, then the external state of the neighboring organism is also available.
* The amount of remaining food on the organism's current cell.
* The amount of energy currently possessed by the organism.
* Values of the simulator parameters s, v, u, M, and K (see below), but not p, q, m, or n.

An organism's "brain" is a program that you will write (in Java). Since there will be many organisms on the grid simultaneously, each will run a separate instance of the brain code. Each instance will
have access only to the local environment of the organism. Organisms are placed randomly on the grid, and don't know their coordinates. The brain can keep a history of local events for the organism if
you think that's useful.

Organisms cannot identify their neighbors. Neighbors may be of the same species (i.e., have the same programmed brain), or of a different species (i.e., have a different brain). This will be important
for simulations in which multiple organisms from different groups are placed on the same grid. (How might you use the external state to help in identification? What about impostors?)

Organisms act one-at-a-time from top-left to bottom-right, row by row. We number the top-left cell as (1,1) and the bottom-right cell as (m,n). That means that the state of the virtual world seen by an organism at (x,y) reflects the situation in which all organisms in positions (x',y') lexicographically less than (x,y) have already made their moves, while organisms in positions lexicographically after (x,y) have not yet moved. This convention allows all operations to happen without any need for resolving conflicts between organisms (for example trying to move to the same cell). However, it leads to some
slightly unintuitive effects:
* An organism that is sensed to the west is usually in its final position for the cycle (can you think of an exception?), while an organism sensed to the east may or may not be in its final position.
* An organism moving east can be sensed from the west, while an organism moving west cannot be sensed from the east. (There's an exception to this observation: what is it?)

We'll provide an organism "simulator" that reads in one or more organism brains, places one organism for each such brain randomly on the grid, and lets the organisms behave according to their
brains' instructions.

There are several goals for this project:
1. Your organism should be able to survive and replicate in an environment where it is the only kind of organism present. This may not be as easy as it sounds. Overpopulation may lead to consumption of all the food. If p is sufficiently small, extinction may ensue. The goal is to achieve the highest long-term stable population.
2. Your organism will be tested in environments containing other organisms. The goal here is primarily to survive, and secondarily to survive in higher numbers than competing organisms. Can your organisms populate the grid faster than their competitors? (Is that even the right strategy given the possibility of everybody going extinct?) How might you program your organisms to "recognize" different organisms based on their state and/or behavior? If you can distinguish members of your species, how might you behave differently to members of another species? Your goal here is not necessarily to obliterate other species, but to maximize the fraction of the population containing your brain.
3. In the discussion so far, we haven't limited the size of organisms' brains. How might your programs change if (to simulate an organism's limited brain size) we limited the complexity of the brain code? For example, what if we eliminated all looping constructs (while, for, etc.) from brains?

m/n horizontal/vertical size 5 to simulator screen width
p probability of spontaneous appearance of food 0.001-0.1
q probability of food doubling 0.002-0.2
s energy consumed in staying put 1 (other parameters scale)
v energy consumed in moving or reproducing 2-20
u energy per unit of food 10-500
M maximum energy per organism 100-1000
K maximum food units per cell 10-50
