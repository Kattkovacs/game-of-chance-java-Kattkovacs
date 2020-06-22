# Game of Chance

## Story

In this project your task is to select a game of chance and write a simulation to test out how we could win on it with the best probability.

1. Select a gambling game (**tippmix, formula 1, illegal car race, horse race, cock fight, etc. or something else, but be sure check with the mentors**).
1. **Make sure you understand the _game of chance_ you've selected for yourself.**
    * Don't choose roulette if you don't care about it, or don't know the rules, instead create a "fictive game" (e.g. _jedi master tournament_) where you can set up some fun rules for yourself to enjoy.
1. Define your game's historical data structure (e.g. what will be in a CSV file that you're going to generate).
1. Simulate a "round" of game (e.g. one game of tippmix, one round of horse race) and save the result.
1. Repeat this several times (_lots_ of times) and save each result, this will be your historical dataset.
1. Analyze your historical data and figure out a winning strategy (e.g. which horse to bet to maximize profit).

> **Don't use the words related to _gambling_ in your repository**, because GitHub might ban your account.
> **Do not use these words**: gambling, roulette, bet, lottery, casino, etc.

### Example

As an example of what you need to create here's the output of our reference code, a horse racing simulator.

#### Running _without_ command-line arguments

When run without arguments (`java -classpath <your-class-files> com.codecool.gameofchance.Program`) the program prints out some diagnostic information, tries to load historical data and finally it fails with an error message (`history.csv` doesn't exist).

![Output of running the program without arguments](https://learn.code.cool/media/java/game-of-chance-1.jpg)

#### Running _with_ command-line arguments

Here `10` is passed on to the program as a command-line argument (`java -classpath <your-class-files> com.codecool.gameofchance.Program 10`), which means we ask the program to generate 10 rounds of historical, simulated data for us.

After this is done, the program uses this to evaluate what's the best tactic to win in the game.

![Output of running the program with arguments](https://learn.code.cool/media/java/game-of-chance-2.jpg)

Running the application again with `java -classpath <your-class-files> com.codecool.gameofchance.Program` would repeat this without generate new rounds of simulated games. Running `java -classpath <your-class-files> com.codecool.gameofchance.Program 10` would add 10 new rounds of simulated games and reevaluate the results.

## What are you going to learn?

In this project

* you'll be using command-line arguments to provide config data to your program (as opposed to asking for user input).
* you're going to practice basic control flow structures and use generic collections.
* you'll need to read and write CSV files which will store historical and other kind of data related to your game.
* you'll practice designing classes according to specification and familiarize yourself with modular code.
* you'll learn about what Dependency Inversion is and how it's useful in breaking up your application logic.
* you'll practice how and where to handle exceptions and raising them to create a stable, working program.

## Tasks

1. Requirements imposed on the project structure, the classes it contain, the contents of these classes and the executable's behavior that you're going to produce.
    - All class are defined in the proper package: `gamestatistics`or `logger`
    - These classes are part of the solution: `Main`, `Logger`, `ConsoleLogger`, `DataEvaluator`, `HistoricalDataSet`, `HistoricalDataPoint`, `Result`
    - At least one addtional class (related to custom "game data") is part of the solution.
    - Program runs outside an IDE (VS Code, Visual Studio, etc.)
    - Program runs on other machines where the JRE is installed
    - The codebase doensn't contain static members (except the `Main` method and methods defined as static in the requirements)
    - Only the `ConsoleLogger` and its methods write to the console
    - The program never ask the user for input during its execution

2. Your application's executable must be runnable from the command-line with an argument `rounds` which defines the number of games to simulate.
    - Executing the program without command-line arguments loads historical data from `history.csv`
    - Executing the program without command-line arguments when no `history.csv` exists in the working directory results in an error message
    - Executing the program with a non-negative integer as the first command-line argument generates the required amount of simulated historical data
    - Executing the program with `0` as the first command-line argument doesn't generate new simulated historical data
    - Executing the program when the first command-line argument is *not* a non-negative integer (e.g. `-1`, `123.456`, `nyancat`) results in an error message
    - Executing the program with a valid first command-line argument when `history.csv` exists loads historcal data from the CSV *and* generates the required amount of simulated historcal data, *combining* the two

3. The should be a main entry-point of the application, the `main()` method and its reposponsibilities are creating instances of other classes and coordinating their "work".
    - `Program` contains the static main method: `void main(string[])`
    - `Program` contains the static method: `HistoricalDataSet generateHistoricalDataSet(int)`
    - Calling `generateHistoricalDataSet` with `0` returns a new instance of `HistoricalDataSet` containing data loaded from `history.csv`
    - Calling `generateHistoricalDataSet` with `0` throws an exception when `history.csv` doesn't exist
    - Calling `generateHistoricalDataSet` with a non-negative input generates the required amount of simulated historical data
    - Calling `generateHistoricalDataSet` with negative input throws exception

4. `HistoricalDataSet` is responsible for encapsulating data: for loading existing simulations from CSV and for generating new ones.
    - `HistoricalDataSet` contains the constructor: `HistoricalDataSet(Logger)`
    - Every constructor in `HistoricalDataSet` accepts an `Logger` parameter at least, e.g. `HistoricalDataSet(Logger, string)`
    - `HistoricalDataSet` contains a method to access the number of underlying data points: `int getSize()`
    - `HistoricalDataSet` contains a private field to _store_ the underlying data points: `List<HistoricalDataPoint> dataPoints`
    - `HistoricalDataSet` contains a public method to access the underlying (private) data points: `List<HistoricalDataPoint> DataPoints` Be aware of making it unmodifyable!
    - `HistoricalDataSet` contains the public method: `void generate()`
    - Calling `generate` generates a _single_ new `HistoricalDataPoint` instance, stores it in the private `dataPoints` list *and* appends a new entry to `history.csv` based on it
    - `HistoricalDataSet` contains the public method: `void load()`
    - Calling `load` reads historcal data from `history.csv`, creates a `HistoricalDataPoint` instance for _each_ entry and appends it to the underlying (private) data points

5. Instances of this class describe the input(s) and output(s) for a round of simulated game, e.g. in a horse race simulator it could store the names of the horses that participated in a race and the order that they've finished.
    - `HistoricalDataPoint` defines one or more public properties to expose the state of one round of simulated game

6. Define at least one class that describes the state of a simulated round of game, e.g. in a horse race simulator one such class could be `Horse` which stores every data about a single horse, its name, speed, etc. Store such data in a CSV file, e.g. `horse.csv`.
    - Your "Game Data" class contains the static method: `List<"Game Data"> read()` (use your class's name instead of "Game Data")
    - Calling `read` reads data from a CSV that stores your game objects' data and returns a list of objects
    - Calling `read` when no CSV exist to read data from throws an exception

7. The `DataEvaluator`'s reposponsibility is clearly separated from managing (loading, generating) simulated data, instead it only _reads_ already available data stored in memory and analyzes the results to figure out the best bet possible based on historical events.
    - `DataEvaluator` contains the constructor: `DataEvaluator(HistoricalDataSet, Logger)`
    - Every constructor in `HistoricalDataSet` accepts a `HistoricalDataSet` and an `Logger` parameter at least, e.g. `HistoricalDataSet(HistoricalDataSet, ILogger, string)`
    - `DataEvaluator` contains the public method: `Result run()`
    - Calling `run()` evaluates historical data points in order to generate and return a new `Result` instance
    - The `run()` method uses the data evaluator's state (the data points it was supplied during its construction) during evaluation

8. `Result` class is simply a "data" class which contains no behavior, its sole purpose is to aggregate different outputs into a single object so that the `DavaEvaluator` could return a single return value from the `run()` method.
    - `Result` contains the constructor: `Result(int, string, float)`
    - `Result` doesn't contain any other constructor definitions
    - `Result` contains integer field and getter method to access the number of data points the result is based on: `int numberOfSimulations`
    - `Result` contains a string field and a getter method to access what's the best choice to win in this game: `String bestChoice`
    - `Result` contains float field and getter method to expose what's the chance of winning when betting on the best choice in this game: `float bestChoiceChance`
    - The `bestChoiceChance` field's value is normalized between `0.0` and `1.0`, e.g. something like `0.4f`, which translates to `40%`

9. Instances of the `ConsoleLogger` encapsulate logic to display informational and error messages to the user during the application execution. It displays its output on the terminal. There could be other similar classes which implement the `Logger` interface. Since other classes rely only on the interface they're **loosly coupled** with the logging behavior.
    - `ConsoleLogger` implements the `Logger` interface
    - `ConsoleLogger` contains the constructor: `ConsoleLogger()`
    - `ConsoleLogger` doesn't contain any other constructor definitions
    - `ConsoleLogger` contains the public method: `void info(string)`
    - Calling the `info()` method logs messages in the following format: `INFO  2020-03-09T05:19:03: an informational message`
    - Calling the `info()` method logs _blue_ informational messages to the console
    - `ConsoleLogger` contains the public method: `void error(string)`
    - Calling the `error()` method logs messages in the following format: `ERROR 2020-03-09T07:23:09: an error message`
    - Calling the `error()` method logs _red_ informational messages to the console

10. Using the proper access modifiers is a key aspect of object-oriented programming. Design your classes, methods, fields and properties taking into account to use the most restrictive modifiers possible.
    - Every _class_ is given the strictest access modifier possible
    - Every _property_ is given the strictest access modifier possible
    - Every _field_ is given the strictest access modifier possible
    - Every _constructor_ is given the strictest access modifier possible
    - Every _method_ is given the strictest access modifier possible

11. Knowing when and how many times classes need to be instantiated is a key concept to master. During the implementation of required classes and methods keep in mind the following requirements and use class design techniques to overcome the limitations they impose (e.g. by using constructor and method parameters to pass around references).
    - `Program` is never instantiated during the program's execution
    - `HistoricalDataSet` is instantiated exactly once during the program's execution
    - `DataEvaluator` is instantiated exactly once during the program's execution
    - `ConsoleLogger` is instantiated exactly once during the program's execution
    - `Result` is instantiated exactly once during the program's execution
    - `HistoricalDataPoint` is instantiated zero or more times during the program's execution
    - Your "Game Data" class is instantiated zero or more times during the program's execution

## General requirements

- Do not change method signatures defined in the requirements
- Exceptions are only handled in the `Program` class and by its methods (exceptions can be thrown anywhere else)
- Variables are declared with the narrowest scope possible

## Hints

- If you need to store a file (e.g. `your.csv`) location independently which are resources to your project,
  put it into the `src/main/resources` folder. [Here is how to manage it](https://docs.oracle.com/javase/8/docs/technotes/guides/lang/resources.html)
- You can easily colorize the console output [with some ANSI magic](https://stackoverflow.com/questions/5762491/how-to-print-color-in-console-using-system-out-println)

## Starting your project

To start your project click [this link](https://journey.code.cool/v2/project/solo/blueprint/game-of-chance/java).

## Background materials

- :exclamation: [How to design classes](https://learn.code.cool/full-stack/#/../pages/java/how-to-design-classes.md)
- :exclamation: [Enums](https://learn.code.cool/full-stack/#/../pages/java/enums)
- :exclamation: [Collections](https://learn.code.cool/full-stack/#/../pages/java/collections)
- :exclamation: [A guide to Java HashMap](https://www.baeldung.com/java-hashmap)
- :exclamation: [How to compile and run Java](https://learn.code.cool/full-stack/#/../pages/java/how-to-compile-and-run-java)
- :exclamation: [Reading a file in Java](https://www.baeldung.com/reading-file-in-java)
- :open_book: [A guide to Java packages](https://www.baeldung.com/java-packages)
- :lollipop: [Dependency Inversion principle](https://www.baeldung.com/java-dependency-inversion-principle)
