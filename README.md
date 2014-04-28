eecs678_rrscheduler
===================

Overview of additions to stub functions:

enqueue_task_other_rr:
	Since the scheduler is using round robin, adding a task is as simple as adding
	it to the tail. So, Add the process that is passed to the other_rr queue in rq
	using list_add_tail.  nr_running must also be incremented so that because the
	length of the list was increased by 1.

dequeue_task_other_rr:
	First update the runtime statistics of the process that is about to be taken off
	the queue.  Then delete the task from the list and decrement the number of items
	in the queue.  

yield_task_other_rr:
	Just call requeue_task_other_rr to put the task at the end of the queue.

pick_next_task_other_rr:
	Picking the next tasking is simple in the round robin scheduler, just pick the
	item that is at the head of the queue.  So, first check if the queue is empty,
	if it is, then return NULL, otherwise return the item.  To maintain runtime
	statistics it is necessary to set the se.exec_start value to rq->clock before
	returning the process.

task_tick_other_rr:
	First, check if the time quantum is set to 0 becuase, if it is the scheduler
	should use First-Come-First-Serve, so it should just return automatically.
	If the time quantum is not 0, then decrement the time quantum of the process.
	Check if the process' time slice is now 0, and if it is then it need to
	be preempted, so reset its time slice, requeue it, then tell the scheduler
	to reschedule it with set_tsk_need_resched().


Difficulties:
	The only major difficulty I had was figuring out which state to use since several
	fields are copied in from rq to rq->other_rr (like nr_running).  Then I was very
	confused when thread_runner caused a segmentation fault only when I was used the
	"-q" option, but after investigating the seg. fault in gdb, I realized that the
	options string passed to get_opt() was incorrect (so the problem was in
	thread_runner.c, not my implementation of the scheduler).  I test my scheduler
	by comparing the output of thread_runner when the time quantum was 0, 25, and
	100.  I also checked the dmesg log to see whether the print statements were showing
	that the time quantum was being changed and the round robin scheduling was
	selected.
