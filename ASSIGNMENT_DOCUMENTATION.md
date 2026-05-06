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
1 → simulate single CPU
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

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

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
