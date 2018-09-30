# Cpp Project Initialization Scripts
Scripts I use as a starting point for C++ projects.

## Debian-Ubutu
Makefile for C++ projects.
1. Create empty project directory.
2. Ensure you have "Make" installed.
3. Copy Makefile from repository to project folder.
4. If you do not have a C++ compiler + Docker-Ce use the command line to invoke `make sys-config`
5. Initialize the project directory structure using the `make init` command.
6. Write your C++ code in the `/src` directory and unit tests in the `/test` directory.
7. Compile for your local-machine with `make build` and then run it with `make run`
8. Compile and run unit-tests from the `/test` directory using `make test`.
9. To compile to otvher platforms
    1. First check [Dockcross](https://hub.docker.com/u/dockcross/) to see if they have the correct image.
    2. Invoke `make cross-build img=<name>` where "< name >" is the name of the dockcross image (without author).
        - example for windows 64bit: dockcross/windows-x64 is the image and the command is `make cross-build img=windows-x64`
10. Locate compiled binaries in the `/bin` directory. Compiled object files are in the `/obj` directory.
11. Use `make clean` to clean the `/bin` and `/obj` directories if builds are not detecting new code.