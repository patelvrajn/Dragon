# Recursive wildcard make function; recursively searches for files matching 
# given wildcard pattern.
rwildcard=$(foreach d,$(wildcard $(1:=/*)),$(call rwildcard,$d,$2) $(filter $(subst *,%,$2),$d))

# Source files (*.cpp)
srcs = $(call rwildcard,./,*.cpp)

# Object files (*.o)
objs = $(patsubst %.cpp,%.o,$(srcs))

# Dependency information files (*.d)
depends = $(patsubst %.cpp,%.d,$(srcs))

# Compiler and compiler flags.
CXX      = g++
CXXFLAGS = \
		   -g \
		   -std=c++17 \
	       -O3 \
	       -MMD \
	       -MP \
	       -Wall \
	       -Wextra \
	       -Wpedantic \
		   -fpie \
	       -pie \
		   -I ../include

.PHONY: all clean

all: dragon

# Clean compile outputs from last make.
clean:
	find .. -name "*.d" -type f -delete
	find .. -name "*.o" -type f -delete
	find .. -name "dragon" -type f -delete

# The final product depends on the object files.
dragon : $(objs)
	$(CXX) $(CXXFLAGS) -o $@ $^

# An object file depends on the respective cpp file.
%.o: %.cpp
	$(CXX) $(CXXFLAGS) $< -o $@ -c

# Include dependency rules output from previous make.
-include $(depends)
