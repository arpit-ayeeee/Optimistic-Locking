# Optimistic Locking using compare_and_swap

This program demonstrates the use of atomic operations in C to safely update a shared variable (`count`) from multiple threads without causing race conditions. It uses `stdatomic.h` for atomic operations and **pthreads** for multi-threading.

## Overview

The program creates two threads that attempt to increment a shared atomic variable (`count`). Atomic operations ensure that even in a multi-threaded environment, the value of `count` is updated safely.

### Key Concepts:
- **Atomic Operations**: These operations ensure that certain variables (e.g., `count`) are updated in an indivisible way, preventing race conditions.
- **Threads**: The program uses **pthreads** to create and run two concurrent threads, each of which tries to increment the value of `count`.

## Code Structure

- **atomic_int count**: This variable is shared between the threads and uses atomic operations for safe access.
- **incrementCount function**: Each thread calls this function to attempt to increment the `count` variable using an atomic compare-and-swap operation (`atomic_compare_exchange_strong`).
- **pthread_create & pthread_join**: Threads are created and executed concurrently, and the program waits for both threads to finish before terminating.

## How it Works

1. **Thread Creation**: The main function creates two threads, both of which run the `incrementCount` function.
2. **Atomic Increment**: Inside `incrementCount`, the thread tries to atomically increment the `count` by loading its current value, adding 1, and then using `atomic_compare_exchange_strong` to ensure the value hasn't changed between reading and writing.
   - If the operation fails (i.e., another thread modified `count` before the first thread could update it), an error message "inc failed" is printed.

3. **Thread Synchronization**: The main function waits for both threads to complete using `pthread_join`.

## Compilation

To compile the program, use the following command:

```bash
gcc -o atomic_increment atomic_increment.c -lpthread
```

## Usage

After compiling, run the program:

```bash
./atomic_increment
```

The program will attempt to increment the shared `count` variable concurrently from two threads, and it will print "inc failed" if an atomic update fails for a thread.

## Note

In assembly, cmpxchgl (compare exchange lock) is used to do compare_and_swap, and this is an atomic instruction, i.e. cpu will not context switch when this will happen. Check if cpu supports it, else cpu itself uses mutex to do this, basically using pessimistic to achieve optimistic

## Conclusion

This example shows how atomic operations can be used in multi-threaded programs to manage shared state safely without the need for explicit locks, which helps prevent race conditions and potential inconsistencies.
