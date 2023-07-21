# GNU Make

```
# Generic Makefile for compiling C programs with GCC
#
# Below is a generic Makefile with comments that defines the most common options
# and arguments for GCC (GNU Compiler Collection) in a C programming context:
#
# This Makefile assumes that you have a directory structure where your C source
# files are located in a src/ directory, object files will be generated in a
# build/ directory, and the final executable will be placed in a bin/ directory.
# Adjust the directory names according to your project structure if needed.
# 
# To use this Makefile, place it in the root directory of your project and
# simply run make in the terminal. It will compile all the .c files in the src/
# directory and create the executable in the bin/ directory. You can also run
# make clean to remove all generated files (object files and the executable).
# 
# Please note that this Makefile is meant to be a starting point and may need
# modifications based on the specifics of your project, such as additional
# compiler flags or additional libraries needed for linking.
#
# Resources
# =========
# https://www.gnu.org/software/make/manual/make.html
# https://www.gnu.org/software/make/manual/html_node/index.html
# https://www.gnu.org/software/make/manual/html_node/Quick-Reference.html
# https://www.gnu.org/software/make/manual/html_node/Special-Targets.html
# https://www.gnu.org/software/make/manual/html_node/Catalogue-of-Rules.html

# Compiler and linker options
CC := gcc
CFLAGS := -Wall -Wextra -Wpedantic -std=c11
LDFLAGS :=

# Directories
SRC_DIR := src
BUILD_DIR := build
BIN_DIR := bin

# Source files (add more .c files here if needed)
SRC_FILES := $(wildcard $(SRC_DIR)/*.c)

# Object files derived from source files
OBJECTS := $(patsubst $(SRC_DIR)/%.c,$(BUILD_DIR)/%.o,$(SRC_FILES))

# Executable name
TARGET := $(BIN_DIR)/my_program

# Build rule for the executable
$(TARGET): $(OBJECTS)
	$(CC) $(LDFLAGS) -o $@ $^

# Build rule for object files
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -c -o $@ $<

# Create build directory if it doesn't exist
$(BUILD_DIR):
	mkdir -p $(BUILD_DIR)

# Clean rule to remove all generated files
clean:
	rm -rf $(BUILD_DIR) $(BIN_DIR)

# Meta command (@) to create new project
@new: \
	\
# Begin Recipe
	echo "Hello World"

.PHONY: clean
```
