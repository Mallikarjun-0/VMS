# Virtual Memory Simulation using Demand Paging

### 1. Overview
In this project, we simulate a demand-paged virtual memory. This involves processes generating virtual addresses, which the Memory Management Unit (MMU) translates into physical addresses. If a page is not in memory, a page-fault occurs, and the MMU brings the page from disk(Not practically), possibly replacing an existing page using the Least Recently Used (LRU) algorithm. The simulation consists of four modules:

- **Master:** Creates other modules and initializes data structures.
- **Scheduler:** Schedules processes using the First-Come, First-Served (FCFS) algorithm.
- **MMU:** Translates page numbers to frame numbers and handles page faults.
- **Process:** Generates page numbers from a reference string.

### 2. Input
- **Total number of processes (k)**
- **Virtual address space:** Maximum number of pages required per process (m)
- **Physical address space:** Total number of frames (f)

### 3. Master Process
**Tasks:**
- **Data Structures:**
  - **Page Table:** One per process, size equal to virtual address space.
  - **Free Frame List:** Shared memory list used by MMU.
  - **Ready Queue:** Message queue used by the scheduler.
  - **Communication Queues:** Two message queues for scheduler-MMU and process-MMU communication.

**Process Creation:**
- Creates the scheduler, MMU, and processes at fixed intervals.
- Generates page reference strings for processes.

**Implementation:**
- File: `Master.c`
- Initializes data structures and creates modules.
- Waits for scheduler notification to terminate modules.

**Reference String Generation:**
- Random length, ensuring some invalid addresses.
- Potential for illegal addresses causing segmentation faults.

### 4. Scheduler
**Tasks:**
- Schedules processes from the ready queue using FCFS.
- Handles messages from MMU:
  - **PAGE FAULT HANDLED:** Enqueues the process.
  - **TERMINATED:** Schedules the next process.

**Implementation:**
- File: `sched.c`
- Executed by Master, handles process execution and termination.

### 5. Processes
**Tasks:**
- Generates page numbers from reference string and interacts with MMU.
- Handles valid frame numbers, page faults, and invalid page references.
- Notifies MMU upon completion.

**Implementation:**
- File: `process.c`
- Created by Master, waits in the ready queue until scheduled.

### 6. Memory Management Unit (MMU)
**Tasks:**
- Translates page numbers to frame numbers.
- Handles page faults by invoking PageFaultHandler (PFH).
- Sends messages to the scheduler based on page fault handling and process completion.

**Implementation:**
- File: `MMU.c`
- Executed by Master, maintains global timestamp for page references.

### 7. Data Structures
- **Page Table:** Shared memory, one per process, includes frame number and validity bit.
- **Free Frame List:** Shared memory list managed by MMU.
- **Process to Page Number Mapping:** Shared memory, contains number of pages per process.

### 8. Output
- **MMU:** Prints and logs page fault information, invalid page references, and global ordering.
- **Output File:** `result.txt` for all logs.

---

This simulation mimics the behavior of a demand-paged virtual memory system, handling processes, scheduling, and page management to understand the intricacies of operating system memory management.
