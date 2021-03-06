REPL = repl
PROG = lsp
TEST = test_$(PROG)

REPL_SRC = repl.c lsp.c
REPL_OBJ = $(patsubst %.c,%.o,$(REPL_SRC))

PROG_SRC = main.c
PROG_OBJ = $(patsubst %.c,%.o,$(PROG_SRC))

TEST_SRC = test.c lsp.c
TEST_OBJ = $(patsubst %.c,%.o,$(TEST_SRC))

SRC_DIR = ../src
INC_DIR = ../include

vpath %.c $(SRC_DIR)
vpath %.h $(INC_DIR)

CPPFLAGS = -I$(INC_DIR)
CFLAGS = -ggdb -std=c99

.PHONY: all
all: $(REPL) $(PROG) $(TEST) TAGS

$(REPL): $(REPL_OBJ)
	gcc -o $@ $^ -lreadline

$(PROG): $(PROG_OBJ)
	gcc -o $@ $^

$(TEST): $(TEST_OBJ)
	gcc -o $@ $^

%.o: %.c
	gcc $(CFLAGS) $(CPPFLAGS) -c -o $@ $<

TAGS: $(TEST_SRC) $(PROG_SRC)
	ctags -e -R $(SRC_DIR) $(INC_DIR)

.PHONY: run_tests
run_tests: $(TEST)
	@echo
	@echo Running tests...
	@echo
	./$(TEST)

.PHONY: check
check: run_tests
	@echo
	@echo Checking for memory leaks...
	@echo
	valgrind --tool=memcheck --leak-check=full ./$(TEST)

.PHONY: clean
clean:
	rm -f $(REPL) $(PROG) $(TEST) $(REPL_OBJ) $(PROG_OBJ) $(TEST_OBJ) TAGS
