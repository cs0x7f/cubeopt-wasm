# cubeopt-wasm
WebAssembly based optimal rubiks cube solver, available on [https://cstimer.net/cubeopt/](https://cstimer.net/cubeopt/)

You can directly open the link above to use this solver, or you can deploy this project separately. This project contains all the resources needed for the solver, but you may encounter requirements for HTTPS and HTTP headers (mime, cors, etc.) during deployment.

cubeopt is a series of fast optimal solvers for Rubik's Cube in half-turn metric (HTM). This project is the deployment files compiled from cube48opt series into webassembly modules for use in browsers. Due to runtime limitations, its performance is approximately half that of the native version ([benchmark of numerious solvers](https://github.com/cs0x7f/cube_solver_test/wiki)). We haven't tested all the browsers yet, but generally recommend using the latest Chrome browser.

# Variations and usage

## Variations

The cube48opt series uses IDA* algorithm to solve Rubik's Cube. The difference between them lies in the size of pruning table (pattern database). Quantitative analysis shows that solving efficiency is approximately proportional to the size of pruning table; that is, doubling the size of pruning table halves the solving time.

This project includes 9 solvers, and their pruning table sizes and solving efficiencies are shown in the table below. (Please note that time-related data is heavily dependent on the environment.)

| Variations | Table Size | Seconds per cube |
|:----------:|:----------:|:----------------:|
| cube48opt1 |  30.4 MB   |       ~80        |
| cube48opt2 |   121 MB   |       ~20        |
| cube48opt3 |   243 MB   |       9.6        |
| cube48opt4 |   486 MB   |       4.8        |
| cube48opt5 |   972 MB   |       2.4        |
| cube48opt6 |  1945 MB   |       1.2        |
| cube48opt7 |  3891 MB   |       0.6        |
| cube48opt8 |  7782 MB   |       0.4        |
| cube48opt9 | 15565 MB   |       0.2        |

For comparison, the table below shows the efficiency of other solvers. Some solvers do not support multi-threading for solving a single cube, in this case, the second number in the table represents the throughput-converted efficiency (i.e., the equivalent solving time per cube after concurrently solving multiple cubes).

|     Solvers      | Table Size |    Seconds per cube    |
|:----------------:|:----------:|:----------------------:|
|    h48h7         |  3793 MB   |             1.1        |
| vcube coord=112  |  2603 MB   |       7.5 / 1.3        |
|    nxopt31       |  2603 MB   |      11.4 / 1.5        |
|  Cube Explorer   |  1972 MB   |      28.2 / 3.5        |
|    3ColorCube    |  2959 MB   |             4.2        |

## usage

Before solving, you need to select a suitable solver and initialize the pruning table. There are two ways to initialize the pruning table: let the solver do it automatically, or use the data file downloaded after initialization.

The larger the pruning table, the longer the initialization time usually is. However, due to the structure of the pruning table, the initialization times for cube48opt2 to 3 are similar (less than 1 minute), and the initialization times for cube48opt4 to 7 are similar (several minutes).

Once the solver has finished initialization, you can download the pruning table to your local machine for faster initialization next time.

After initialization, you can input the cube to be solved in the scramble area (currently only inputting the scramble algorithm is supported), or you can randomly generate several random-move scrambles for testing.

Before solving, you can choose the number of threads to use and the number of cubes to solve simultaneously. By default, the solver uses all available threads and solves only one cube at a time; all threads are used to solve that one cube until it is solved before moving on to the next. If you set the number of cubes to be solved simultaneously to a value greater than 1 (which must be a divisor of the number of threads), the solver will solve multiple cubes simultaneously, but the number of threads per cube will decrease accordingly. In other words, if you set the number of cubes to be solved simultaneously to the number of threads, each cube will be assigned to only one thread (this usually has the highest throughput but the longest latency, because communication between multiple threads affects solving efficiency).

After clicking the "Solve" button, each cube in the scrambled area (each row of scrambles) will be solved. The log area will print detailed solution information, including the solution for each cube and search information used for analysis (number of nodes, traversal efficiency, etc.).

