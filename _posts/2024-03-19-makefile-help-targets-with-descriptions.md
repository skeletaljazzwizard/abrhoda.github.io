---
layout: post
title:  "Makefile Help Target With Descriptions"
date:   2024-03-19 00:00:00 -0400
categories: [tips, automation]
---

`make` and Makefiles are a utility tool to automate tasks by running what are called targets which are made up of prerequisites and individual steps. A quick example is a `build` target invoked by calling `make build` which might have a prerequisite to bring the project to a clean state by running a target of `clean` and then steps to build the project locally. You can read more about make and Makefile [here](https://www.gnu.org/software/make/manual/make.html).

Below is an example for a self documenting Makefile where every target is printed to the console along with a help message. Invoking `build help` will print out each target’s help message after the “##” string.

```make
build: clean main.o ## calls the `clean` recipe and then builds `proj.o`
  gcc main.o -o proj

main.o: main.cpp ## builds main.o
  gcc main.cpp

.PHONY: clean
clean: ## remove built files
  rm -f  *o proj

.PHONY: help
help: ## print this help message
  @awk -F ':|##' '/^[^\t].+?:.*?##/ {printf "\033[36m%-20s\033[0m %s\n", $$1, $$NF}' $(MAKEFILE_LIST)
```

Example output can be seen below.
```
build                 calls the `clean` recipe and then builds `proj.o` 
main.o                builds main.o
clean                 remove built files
help                  print this help message
```
