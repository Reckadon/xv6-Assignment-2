# Operating Systems Assignment 2: Boosted Lottery Scheduler for xv6

---

## Overview

This project modifies the xv6 operating system to implement a **Boosted Lottery Scheduler**. The scheduler uses lottery scheduling to proportionally share CPU time among processes and boosts the priority of sleeping processes to ensure fairness.

---

## Assignment 2: Implemented Features

- **Lottery Scheduling:** The scheduler allocates CPU time to processes based on a lottery system. Each process is assigned a certain number of tickets, and whenever scheduling occurs, a random ticket is drawn. The process holding the winning ticket is scheduled to run for the next time slice. This ensures that, over time, the proportion of CPU time a process receives is relatively equal to the proportion of tickets it holds. The first process starts with 1 ticket, and child processes inherit their parent's ticket count. The scheduler logic and ticket management are implemented in `proc.c`.

- **Boosted Tickets:** When a process is blocked (eeither sleeping or waiting for I/O), it cannot participate in the lottery(can't be scheduled to run). To ensure fairness, once the process becomes runnable again, its tickets are temporarily doubled for the same number of ticks it was blocked. This "boost" compensates for the time it could not participate in the lottery, maintaining proportional CPU allocation.

- **Improved Sleep/Wake Mechanism:** Processes are only woken up when their sleep interval has expired. The older XV6 sleep/wake mechanism consisted of waking up each and every process sleeping on the appropriate channel each time an interrupt was called, which was essentially every tick. This led to quite a lot of unnecessary context switching and CPU usage. The new mechanism implemented here basically made it so that processes are only woken up when their sleep interval has actually expired. This was implemented in the `wakeup1` function where the sleep channel is checked against the current time to determine if the process should be woken up. The structure of every process was also changed in `proc.h` to include a sleep time field.

- **New System Calls:**
  - `settickets(int pid, int tickets)`: Allows changing the number of tickets for a specific process.
  - `srand(uint seed)`: Sets the seed for the random number generator used in the lottery scheduler.
  - `getpinfo(struct pstat *)`: Returns information about all processes, including their ticket count, runtime, and boost status, for testing and debugging purposes.

---



## To run XV6, go into your repo and run

```bash
make clean qemu
```

---

## Testing

We have implemented various tests to test the added features in `test.c` file in `usr` directory.  
You can run them all of them from the OS' shell with the command `test`.
