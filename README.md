# Integrate `dmd as a library` in `D-Scanenr`

## Current Status
A proper and reliable compiler library would encourage the development of tools such as
**auto completion**, **auto refactoring**, **style checkers**, tools that would benefit
the ecosystem. For a few years, there has been a compiler library exposing `dmd`
internals, namely `dmd as a library`. However, the interface is still lacking.
To address this issue my approach is trying to integrate `dmd-as-a-library` in an
already successful project `D-Scanner`.

## Goals
This project has 2 main goals:
- Developing a proper interface for the compiler library by leveraging
existing use cases
- Updating existing tools to use the compiler library which makes it easier to
update to the latest version of the compiler

This project will add the following benefits to the D ecosystem:
- Improve the compiler library so that it can be used in other projects as well
- Get rid of the effort necessary to update 3rd party tools used in D-Scanner
as the compiler evolves

## Method
D-Scanner's architecture is based on the visitor design pattern. For most features
that D-Scanner offers, the implementation consists in a custom visitor which can
traverse an abstract syntax tree and also do a specific check for particular nodes.

These visitors are relying on a 3rd party tool called `libdparse`.
The desired approach is to replace `libdparse` with `dmd as a library` in all visitors
and fixing the lacks discovered in `dmd as a libary` in the process.

The extensive list with all the visitors is too long to be presented here, but can be
seen [here](https://github.com/dlang-community/D-Scanner).

A few examples of how replacing `libdparse` with `dmd-as-a-library` looks like can
be seen [here](https://github.com/Dlang-UPB/D-scanner).

## Planning

During SAoC, a subset of the visitors available in D-Scanner will be reworked to
use `dmd as a library` instead of `libdparse`, according to the following schedule:
|          Week           |                                 Activity / Functions                               |      Details     |
|:-----------------------:|:----------------------------------------------------------------------------------:|:----------------:|
| 15.09.2021 - 22.09.2021 | `final_attribute`                                                                |
| 22.09.2021 - 29.09.2021 | `fish`                                                                  |
| 29.09.2021 - 06.10.2021 | `function_attributes`                                           |
| 06.10.2021 - 13.10.2021 | `has_public_example`           |
| 13.10.2021 - 20.10.2021 | `if_statements`                                                 | First Milestone  |
| 20.10.2021 - 27.10.2021 | `ifelsesame`                                         |
| 27.10.2021 - 03.11.2021 | `imports_sortedness`                                   |
| 03.11.2021 - 10.11.2021 | `incorrect_infinite_range` |
| 10.11.2021 - 17.11.2021 | `label_var_same_name_check`                                           | Second Milestone |
| 17.11.2021 - 24.11.2021 | `lambda_return_check`                                            |
| 24.11.2021 - 01.12.2021 | `length_subtraction`                                                 |
| 01.12.2021 - 08.12.2021 | `line_length`                                            |
| 08.12.2021 - 15.12.2021 | `local_imports`                                               | Third Milestone  |
| 15.12.2021 - 22.12.2021 | `logic_precedence`                                                 |
| 22.12.2021 - 29.12.2021 | `mismatched_args`                                                   |
| 29.12.2021 - 05.01.2022 | `numbers`                                        |
| 05.01.2022 - 15.01.2022 | `package`                                                                         | Final Milestone  |

Note: The names of the visitors are as they can be found in [D-Scanner](https://github.com/dlang-community/D-Scanner), and also this planning
contains only the implementation of visitors, but in reality there will also be work
involved to update `dmd as a library`, but this is a bit more difficult to schedule,
because these updates will be implemented the moment they prove themselves necessary.

