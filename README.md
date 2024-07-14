# Project 2 - Organisms

## Project Setup

- `Organisms` should be the project root
- `Organisms/src` should be the sources root

In Eclipse, this should be the case automatically if you open the proper 
project root.

### IntelliJ setup

Similar to the last project, IntelliJ might silently fail to open the project
due to `Organisms/.classpath`. Delete this file from your local copy if this
is the case, but please _do not commit or push that change._

You will also have to do some additional setup steps:

- Make sure you open `Organisms` as the project root
- Go to `File > Project Structure` (or whatever the equivalent is on Mac)
  - Under the "Project" tab, make sure "SDK" has something valid selected
    and that the "Language level" matches that SDK
  - Under the "Modules" tab, mark `Organisms/src` as the sources root -
    and only that folder. `Organisms` should not be a sources root, just `src`.

## Running the project

- As usual, `gamemodel.properties` contains a list of classes that will 
  appear in the GUI.
- `ui/GUI` will run the GUI.
- `ui/TournamentRunner` will run a tournament based on the `classNames` in
  that file and `tournament.csv`.

## Other Notes

- Do _not_ use any static variables. (Static methods are fine.)
- Do _not_ use `Action.values()` to turn a number into an `Action` - this is a
  costly operation in terms of performance (it makes a new copy every time).
  Please use `Action.fromInt` instead.
- If you need to use randomization, please use `ThreadLocalRandom.current()`
  instead of `new Random()` to avoid significant performance penalties under
  multithreaded workloads.

