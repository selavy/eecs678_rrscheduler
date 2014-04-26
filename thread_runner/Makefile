#CFLAGS=-O2
CFLAGS = -g

NUM_THREADS=4
BUF_SIZE=20m
OPTIONS= -s other_rr

thread_runner: thread_runner.c
	gcc $(CFLAGS) thread_runner.c -lpthread -o thread_runner

clean:
	rm -f thread_runner

test:
	@echo "\n\nRunning with Quantum = 25"
	./thread_runner $(NUM_THREADS) $(BUF_SIZE) $(OPTIONS) -q 25
	@echo "\n\nRunning with Quantum = 0"
	./thread_runner $(NUM_THREADS) $(BUF_SIZE) $(OPTIONS) -q 0
	@echo "\n\nRunning with Quantum = 100"
	./thread_runner $(NUM_THREADS) $(BUF_SIZE) $(OPTIONS) -q 100
