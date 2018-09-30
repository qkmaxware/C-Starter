# -------------------
# Project Structure
# -------------------
BIN_DIR	= bin
LIB_DIR = lib
OBJ_DIR = obj
SRC_DIR = src
DOC_DIR = doc
TEST_DIR = test

EXE_NAME = bin

COMPILER = $(CXX)
COMPILE_FLAGS = -g -std=c++17 -static

.DEFAULT_GOAL := build

# -------------------
# Source File Search
# -------------------
CPP_SRC = $(wildcard $(SRC_DIR)/*.cpp)
C_SRC = $(wildcard $(SRC_DIR)/*.c)

CPP_TEST = $(wildcard $(TEST_DIR)/*.cpp)
C_TEST = $(wildcard $(TEST_DIR)/*.c)

SOURCES = $(CPP_SRC) $(C_SRC)
TEST_SOURCES = $(CPP_TEST) $(C_TEST)

#-------------------
# Compile Rules
#-------------------
COMPILE_SRC = $(SOURCES:.cpp=.o) $(SOURCES:.c=.o)
BASENAME_SRC = $(subst /,., $(SOURCES))
LINKLIST_SRC = $(patsubst %.cpp, $(OBJ_DIR)/%.o, $(BASENAME_SRC))

COMPILE_TEST = $(SOURCES_TEST:.cpp=.o) $(SOURCES_TEST:.c=.o)
BASENAME_TEST = $(subst /,., $(SOURCES_TEST))
LINKLIST_TEST = $(patsubst %.cpp, $(OBJ_DIR)/%.o, $(BASENAME_TEST))

BIN_NAME = $(EXE_NAME)-$(notdir $(DEFAULT_DOCKCROSS_IMAGE))

.cpp.o:
	@$(COMPILER) $(COMPILE_FLAGS) -c $< -o $(OBJ_DIR)/$(subst /,.,$@) 

.c.o:
	@$(COMPILER) $(COMPILE_FLAGS) -c $< -o $(OBJ_DIR)/$(subst /,.,$@) 


#-------------------
# Targets
#-------------------
.PHONY: sys-config
sys-config:
	@echo "Installing dependancies"
	@sudo apt-get install -y build-essential

	@sudo apt-get remove -y docker docker-engine docker.io
	@sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
	@curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	@sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
	@sudo apt-get install -y docker-ce

.PHONY: init
init:
	@echo "Initializing project directory"
	@mkdir $(BIN_DIR) -p || true
	@mkdir $(LIB_DIR) -p || true
	@mkdir $(OBJ_DIR) -p || true
	@mkdir $(SRC_DIR) -p || true
	@mkdir $(DOC_DIR) -p || true
	@mkdir $(TEST_DIR) -p || true

	@touch .gitignore
	@echo "bin/" >> .gitignore
	@echo "doc/" >> .gitignore
	@echo "lib/" >> .gitignore 

.PHONY: cross-build
cross-build:
	@docker pull dockcross/$(img)
	@docker run --rm dockcross/$(img) > ./helper-dockcross-$(img)
	@chmod +x ./helper-dockcross-$(img)
	@./helper-dockcross-$(img) make build
	@rm -f ./helper-dockcross-$(img) || true

.PHONY: clean
clean:
	@rm -f $(LINKLIST_SRC) || true
	@rm -f $(LINKLIST_TEST) || true
	@rm -rf $(BIN_DIR)/* || true

.PHONY: compile
compile: $(COMPILE_SRC)

.PHONY: build
build: compile
	@$(COMPILER) $(LINKLIST_SRC) -o $(BIN_DIR)/$(BIN_NAME) $(COMPILE_FLAGS)

.PHONY: run
run:
	@./$(BIN_DIR)/$(BIN_NAME)

.PHONY: test
test:$(COMPILE_TEST)
	@$(COMPILER) $(LINKLIST_TEST) $(LINKLIST_SRC) -o $(BIN_DIR)/$(BIN_NAME)-test $(COMPILE_FLAGS)
	@./$(BIN_DIR)/$(BIN_NAME)-test