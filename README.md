# cubeopt-wasm
WebAssembly based optimal rubiks cube solver, available on [https://cstimer.net/cubeopt/](https://cstimer.net/cubeopt/)

This project contains all the resources needed for the solver, but you may encounter requirements for HTTPS and HTTP headers (mime, cors, etc.) during deployment.

cubeopt is a series of fast optimal solvers for Rubik's Cube in half-turn metric (HTM). This project compiles cube48opt series into WebAssembly modules for use in browsers. Due to runtime limitations, its performance is approximately half that of the native version ([benchmark of numerious solvers](https://github.com/cs0x7f/cube_solver_test/wiki).)