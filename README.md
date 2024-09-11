#!/bin/bash
#                  @@@@
#             @@@@@@@@@@@@@@@@    @@@@@@@@@
#            @@@@@@@@@@@@@@@@@@   @@@@@@@@@
#           ,@@@@@@@@  @@@@@@@@,  @@@@@@@@@
#           ,@@@@@@@@@.           @@@@@@@@@
#            @@@@@@@@@@@@@%       @@@@@@@@@
#              (@@@@@@@@@@@@@@    @@@@@@@@@
#                  /@@@@@@@@@@@@  ---------
#            @@@@@@@@  @@@@@@@@@  @@@@@@@@@
#            @@@@@@@@   @@@@@@@@  @@@@@@@@@@@@@@@@@@@
#            @@@@@@@@@@@@@@@@@@,  @@@@@@@@@@@@@@@@@@@@
#              @@@@@@@@@@@@@@@    @@@@@@@@@  @@@@@@@@@
#                                 @@@@@@@@@  %@@@@@@@@
# @@@@@@#             @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@
# @@@@@@#             @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@
# @@@@@@#.@@@@@@@&                @@@@@@@@@  %@@@@@@@@
# @@@@@@@@@@@@@@@@@/  @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@  *########  #########
# @@@@@@& &@@@@@@@@%  @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@  (@@@@@@@@  @@@@@@@@@
# @@@@@@#  @@@@@@@@%  @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@  (@@@@@@@@  @@@@@@@@@
# @@@@@@#  @@@@@@@@%  @@@@@@@@@,  @@@@@@@@@  %@@@@@@@@  (@@@@@@@@  @@@@@@@@@
# @@@@@@#  @@@@@@@@%  @@@@@@@@@,
# @@@@@@#  @@@@@@@@%  @@@@@@@@@,
# @@@@@@#  @@@@@@@@%  @@@@@@@@@,
# @@@@@@#  @@@@@@@@%  @@@@@@@@@, BIN.sh General Tester...
# @@@@@@# %@@@@@@@@%  @@@@@@@@@,
# @@@@@@@@@@@@@@@@@   @@@@@@@@@,
# @@@@@@ /@@@@@@@(    @@@@@@@@@,
#
# whoAmI(campus=42barcelona, login=mporras-, mail=manuel.porras.ojeda@gmail.com)
# linkedin: https://www.linkedin.com/in/manuelporrasojeda
# Github: https://github.com/manon42bcn
# Feel free to write!
# Original version: https://github.com/manon42bcn/BinTester

# Bin General Tester

This is a general tester to any bin that will be executed by console. How it works can sound a little bit over-complicated, but my idea was make it flexible enough to be used with any module, that's why test cases files can have "too much" or repeated data.
I developed this tester in bash to keep it open to any user. You can adapt it to make it really useful for you.

## Basic Usage and flags:
```bash
./BinTester.sh [-d] [-l] [-h] [-b] <path_to_cases> [-s] [-f] <path_to_file.txt>
```
- "-d" detailed output.
- "-l" full logs. Default mode will log only errors.
- "-h" help, basic usage help.
- "-b" base directory to cases files.
- "-s" execute only smoke tests
- "-f" execute only an specific file.

## General info.

The idea is work with test cases, defined at one or more files. The files structure:
```  
.
├── exNN
│   ├── BinTester.sh
│   ├── Makefile
│   ├── myCompiledBin
│   ├── Tests
│   │   ├── basic_cases.txt
│   │   ├── big_cases.txt
│   │   ├── other_cases.txt
│   │   ├── basic_smoke.txt
│   │   ├── bin
│   │   │   ├── bin_to_create_input.sh
│   │   │   └── bin_to_create.sh
│   │   └── docs
│   │       └── README.md
│   ├── inc
│   │   └── ...
│   └── srcs
│       └── ...
├── ...
└── make_all.sh
```  
Beside normal files for your project, you can find:
- BinTester.sh (Bash Script with the core of the tester).
- Tests directory. Directory to include bins, test cases, docs, etc. You can personalize where the core should look for test cases using -d flag
- _cases.* files: Files to describe each test case.
- _smoke.* files: Files to describe each smoke test case (\*).
- bin directory. Directory to include any binary or script used in test cases (\*\*). The Core tester will look for binaries using test cases route, but is important to keep everything in order :D.
- docs: README.md and any doc that you need to include. 

- (\*) "Smoke test" is not the proper name, but I use it to separate basic control, errors, Makefile checks, etc. from "regular" tests.
    The execution and structure of "_cases.*" and "_smoke.*" files is the same, are separated to keep everything nice and tidy. _smoke its used to test quite fundamental behaviour, if any smoke tests fails, tester will stop.
- (\*\*) You can use a binary to create an input or to compare output in your test cases.

## Test cases files

- Each test case file should name should be [any informative name]_cases.txt. 
- Each test case file should start with a descriptive title to all cases included in the file.
- Each test case should include, as minimal data: case, bin, input and expected.
- All test cases should be described without space between them.
- eof line is not mandatory.
- Numeral \# to comment line.

As reference. For cpp-06 ex00 I include:
```
│   ├── Tests
│   │   ├── basic_smoke.txt
│   │   ├── double_cases.txt
│   │   ├── extra_cases.txt
│   │   ├── float_cases.txt
│   │   └── integer_cases.txt
```

Each file should use this structure:

- Minimal Data: Only expected allow multiline data.
```
FEATURE>>>General description
case>>>Case Name One
bin>>>./BinaryToTest
fd>>>1
input>>>all
expected>>>
THIS IS AN OUTPUT
<<<expected
<<<case
case>>>Case Name Two
bin>>>./BinaryToTest
fd>>>2
input>>>hello
expected>>>
THIS IS AN OUTPUT
A multiline Output
<<<expected
<<<case
...
<<<eof
```

- You can use special input and strings to get extra functionality:
  - [BIN]: to execute a binary as input or expected. Binary as expected will receive same args from input that bin field. You can also use a command like input>>>[BIN]echo input.
  - [EMPTY]: to leave an empty space.
  - [REGEX]: to compare expected using regex capability.
  - fd>>>output to test. 1 = stdout, 2 = stderr, 3 both.

## Examples.

- random_span.sh: generates a random list of numbers. Output [total_elements] [elem1] [elem2].. [elemN]
- Span_bash.sh: return shortest and longest span. Bash version of cpp08/ex01 exercise.
- Files to test Span (cpp modules 08, ex01).
