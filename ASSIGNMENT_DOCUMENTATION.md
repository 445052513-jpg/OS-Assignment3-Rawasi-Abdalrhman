# Assignment 3 - Complete Documentation

**Student Name**: rawasi abdalrhman alotaibi 
**Student ID**:445052513
**Date Submitted**: 7/ may /2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 4, 2026 (7:00 PM)]
**What I implemented**: 
I found shared resources like contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog by examining the supplied code.
**Challenges encountered**: 
Recognizing potential instances of race situations in the code
**How I solved it**: 
I went over Operating System Concepts' Chapter 3,5 and noted important passages.
**Testing approach**: 
Code execution tracing by hand
**Time spent**: 
2 hours
---

### Entry 2 - [May 5, 2026 (6:30 PM)]
**What I implemented**: 
To safeguard shared variables, I used ReentrantLock.
**Challenges encountered**: 
Making certain that every crucial area was adequately secured
**How I solved it**: 
Try-finally blocks were used to ensure lock release.
**Testing approach**: 
To ensure uniformity, run the software several times.
**Time spent**: 
2 hours and 30 minute
---

### Entry 3 - [May 6, 2026 (5:00 PM)]
**What I implemented**: 
I implemented Semaphore to control CPU access
**Challenges encountered**: 
Understanding where to place acquire() and release()
**How I solved it**: 
Placed acquire at start of run() and release in finally block
**Testing approach**: 
Observed process execution order
**Time spent**: 
2 hours
---

### Entry 4 - [May 6, 2026 (8:00 PM)]
**What I implemented**: 
Tested the full program and verified output correctness
**Challenges encountered**: 
Ensuring no deadlocks occur
**How I solved it**: 
Used proper lock handling and avoided nested locks
**Testing approach**: 
Multiple executions
**Time spent**: 
1 hours and 30 minute
---

### Entry 5 - [May 6, 2026 (10:00 PM)]
**What I implemented**: 
Prepared documentation and final review
**Challenges encountered**: 
Connecting theory with implementation
**How I solved it**: 
Referenced textbook concepts
**Testing approach**: 
Final validation run
**Time spent**: 
1 hour
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
The shared variable contextSwitchCount is where the first race situation takes place. This variable is incremented concurrently by several threads without synchronization, which may result in lost updates and inaccurate counts.

The second race condition is seen in the ArrayList executionLog. Concurrent changes to ArrayList may result in incorrect data or runtime problems because it is not thread-safe.

Unpredictable outcomes happen from threads accessing shared resources without mutual exclusion, which causes several problems.


---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
A ReentrantLock provides mutual exclusion by allowing only one thread to access a critical section at a time. In this implementation, it is used to protect shared variables such as counters and logs.

A Semaphore controls access to a limited number of resources. In this code, a semaphore with one permit is used to simulate CPU access, ensuring only one process executes at a time.

Locks ensure data consistency, while semaphores control resource allocation.


---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock occurs when multiple threads are waiting indefinitely for resources held by each other.
Two prevention techniques are:
1-Using try-finally blocks to ensure locks are always released.
2-Avoiding nested locks and maintaining a consistent locking order.
In this code, deadlocks are prevented by releasing locks in finally blocks and using a single lock.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used a single lock (coarse-grained locking) to protect all shared counters.

This simplifies the design and reduces the risk of deadlocks. However, it limits concurrency because only one thread can access any counter at a time.

Fine-grained locking would allow higher concurrency but increases complexity and risk of errors.

Since the counters are independent, fine-grained locking could provide better performance, but for simplicity and safety, coarse-grained locking was chosen.
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
Because multiple threads update them concurrently
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
    'lock.lock();
    try {
        contextSwitchCount++;
    } finally {
        lock.unlock();
    }'

**Justification**: 
Ensures atomic updates and prevents race conditions
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
ArrayList is not thread-safe
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
    `lock.lock();
    try {
        executionLog.add(message);
    } finally {
        lock.unlock();
    }`

**Justification**: 
Prevents concurrent modification issues
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control CPU access
**Number of permits and why**: 
1 (simulate single CPU)
**Where implemented**: 
Inside run() method
**Code snippet**:
`SharedResources.cpuSemaphore.acquire();
...
SharedResources.cpuSemaphore.release();`

**Effect on program behavior**: 
Ensures only one process runs at a time
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results
I tested the program by executing it multiple times (at least five runs) to verify that the results are consistent across executions, especially the shared counters such as contextSwitchCount, completedProcessCount, and totalWaitingTime.

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```
**Results**: 
(Show that running multiple times produces consistent, correct results)
═══ Synchronization Statistics ═══
Total Context Switches: 19
Total Completed Processes: 10
Total Waiting Time: 320712ms
Average Waiting Time: 32071ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         ٤            ٤٤٢٩         ٣٢٣٧١       
P2         ٢            ٣٤٨٤         ٤٠٣٣        
P3         ٥            ٤٩٥٧         ٣٢٨١٩       
P4         ٢            ٨٥٩٤         ٤٥٥٠٨       
P5         ٤            ٨٨٣٥         ٤٦١٢٩       
P6         ٤            ٢٣٦١         ١٩٥٨٩       
P7         ٤            ٣٤١٦         ٢١٩٦٢       
P8         ٤            ٧٥٢٢         ٤١٨٩٤       
P9         ٥            ٢٩٣٨         ٢٩٤٠٢       
P10        ٥            ٨٦٦٦         ٤٧٠٠٥       

═══ Execution Log Summary ═══
Total log entries: 38

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
When multiple threads change common resources like contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog without synchronization, race situations may arise. For instance, when two threads increment the same number at the same time, their changes may be overwritten, producing inaccurate results. Concurrent updating of the ArrayList executionLog may also lead to runtime problems like ConcurrentModificationException or incorrect data. In order to prevent threads from interfering with one another when accessing shared data, synchronization guarantees mutual exclusion.
**Conclusion**: 
The use of ReentrantLock and Semaphore ensures consistent and correct results across multiple executions, demonstrating that the synchronization mechanisms are properly implemented.
---

### Test 2: Exception Testing 
**What I tested**:
I tested whether concurrent modifications to the shared executionLog could cause runtime exceptions such as ConcurrentModificationException. 
**Testing procedure**:
The program was executed multiple times with different random inputs, while closely monitoring the execution log behavior. I also increased the number of processes and reduced the time quantum to increase thread interleaving and stress test the logging mechanism.
**Results**: 
No ConcurrentModificationException or any runtime exception occurred during execution.
The execution log remained consistent and complete in all runs.
**What this proves**: 
This proves that the executionLog is properly synchronized using ReentrantLock. The critical section that modifies the ArrayList is protected, ensuring thread-safe operations and preventing concurrent modification issues.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
I verified the correctness of the final output values, including total number of completed processes, total context switches, and total waiting time.
**Expected values**: 
-The number of completed processes should equal the total number of processes created.
-The context switch count should reflect the number of times processes were scheduled.
-Total waiting time should be a non-negative value and logically consistent with process execution.
**Actual values**: 
═══ Synchronization Statistics ═══
Total Context Switches: 19
Total Completed Processes: 10
Total Waiting Time: 320712ms
Average Waiting Time: 32071ms
**Analysis**: 
The results confirm that the synchronization mechanisms ensure data integrity and correctness. No incorrect or inconsistent values were observed, indicating that race conditions have been successfully eliminated.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Different time quantum values
**Purpose**: To evaluate how the scheduling algorithm behaves under different configurations and to ensure that synchronization remains effective regardless of workload variations.
**Results**: 
The system behaved correctly under all tested scenarios. With smaller time quantum values, the number of context switches increased, as expected in Round Robin scheduling. With larger quantum values, processes completed faster with fewer context switches. In all cases, the synchronization mechanisms maintained correct and consistent results.
**What I learned**: 
I learned that the performance and behavior of scheduling algorithms are highly influenced by the time quantum. However, synchronization remains essential regardless of the scheduling configuration to ensure correctness and stability.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]
Through this assignment, I gained a deeper understanding of synchronization in multi-threaded systems. I learned that race conditions occur when multiple threads access shared resources without proper coordination, leading to unpredictable results. Using ReentrantLock allowed me to enforce mutual exclusion and protect critical sections effectively. I also learned how semaphores can be used to control access to limited resources, such as simulating a single CPU. One important insight was the necessity of using try-finally blocks to ensure locks are always released, preventing deadlocks. Additionally, I understood the trade-offs between simplicity and performance when choosing synchronization strategies. Overall, this assignment helped me connect theoretical concepts from operating systems with practical implementation in Java.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems where multiple users access and update account balances simultaneously. Synchronization ensures that transactions are processed correctly without data corruption.

**Example 2**: 
Operating system process scheduling, where multiple processes compete for CPU time. Synchronization ensures fair and correct allocation of resources.
---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

I would explain synchronization as a way to organize access to shared resources when multiple threads are running at the same time. Imagine several people trying to write in the same notebook at once—without coordination, the content would become messy and incorrect. Synchronization acts like a rule that allows only one person to write at a time or limits how many people can access the notebook. In programming, we use tools like locks and semaphores to enforce these rules and ensure that data remains correct and consistent.
---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
