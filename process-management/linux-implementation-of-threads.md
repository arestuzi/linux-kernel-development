# The Linux Implementation of Threads

Threads are a popular modern programming abstraction. They provide multiple threads of execution within the same program in a shared memory address space. They can also share open files and other resources. Threads enable *concurrent programming* and, on multiple processor systems, true *parallelism*.

Linux has a unique implementation of threads. To the Linux kernel, there is no concept of a thread. Linux implements all threads as standard processes. The Linux kernel does not provide any special scheduling semantics or data structures to represent threads. Instead, a thread is merely a process that shares certain resources with other processes. Each thread has a unique `task_struct` and appears to the kernel as a normal process --- threads just happen to share resources, such as an address space, with other processes.

This approach to threads contrasts greatly with operating systems such as Microsoft Windows or Sun Solaris, which have *explicit*(明确的) kernel support for threads (and sometime call threads *lightweight processes*). The name "lightweight process" sums up the difference in philosophies between Linux and other systems. To these other operating systems, threads are an abstraction to provide a lighter, quicker execution unit than the heavy process. To Linux, threads are simply a manner of sharing resources between processes(which are already quite lightweight). For example, assume you have a process that consists of four threads. On systems with explicit thread support, one process descriptor might exist that, in turn, points to the four different threads. The process descriptor describes the shared resources, such as an address space or open files. The threads then describe the resources they alone possess. Conversely(相反地), in Linux, there are simply four processes and thus four normal `task_struct` structures. The four processes are set up to share certain resources. The result is quite elegant.

## Creating Threads
Threads are created the same as normal tasks, with the exception that the `clone()` system call is passed flags corresponding to the specific resources to be shared:
```C
clone(CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND, 0);
```
The previous code results in behavior identical to a normal `fork()`, except that the address space, filesystem resources, file descriptors, and signal handlers are shared. In other words, the new task and its parent are what are popularly called *threads*.

In contrast, a normal `fork()` can be implemented as
```C
clone(SIGCHLD, 0)
```
And `vfork()` is implemented as
```C
clone(CLONE_VFORK | CLONE_VM | SIGCHLD, 0);
```
The flags provided to `clone()` help specify the behavior of the new process and detail what resources the parent and child will share. Table 3.1 lists the clone flags, which are defined in `<linux/sched.h>`, and their effect.

| **Flag** | **Meaning** |
| ---- | ---- |
| CLONE_FILES | Parent and child share open files |
| CLONE_FS | Parent and child share filesystem information |
| CLONE_IDLETASK | Set PID to zero (used only by the idle tasks) |
| CLONE_NEWNS | Create a new namespace for the child. |
| CLONE_PARENT | Child is to have same parent as its parent |
| CLONE_PTRACE | Continue tracing child. |
| CLONE_SETTID | Write the TID back to user-space. |
| CLONE_SETTLS | Create a new TLS for the child. |
| CLONE_SIGHAND | Parent and child share signal handlers and blocked signals. |
| CLONE_SYSVSEM | Parent and child share System V `SEM_UNDO` semantics. |
| CLONE_THREAD | Parent and child are in the same thread group. |
| CLONE_VFORK | `vfork()` was used and the parent will sleep until the child wakes it. |
| CLONE_UNTRACED | Do not let the tracing process force `CLONE_PTRACE` on the child. |
| CLONE_STOP | Start process in the `TASK_STOPPED` state. |
| CLONE_SETTLS | Create a new TLS(thread-local storage) for the child. |
| CLONE_CHILD_CLEARTID | Clear the TID in the child. |
| CLONE_CHILD_SETTID | Set the TID in the child. |
| CLONE_PARENT_SETTID | Set the TID in the parent. |
| CLONE_VM | Parent and child share address space. |

## Kernel Threads
It is often useful for the kernel to perform some operations in the background. The kernel accomplished this via *kernel threads* --- standard processes that exist solely in kernel-space. The significant difference between kernel threads an normal processes is that kernel threads do not have an address space. (Their mm pointer, which points at their address space, is NULL.) They operate only in kernel-space and do not context switch into user-space. Kernel threads, however, are schedulable and preemptable, the same as normal processes.

Linux delegates(委托) several tasks to kernel threads, most notably the *flush* tasks and the *ksoftirqd* task. You can see the kernel threads on your Linux system by running the command `ps -ef`. There are a lot of them! Kernel threads are created on system boot by other kernel threads. Indeed, a kernel thread can be created only by another kernel thread. The kernel handles this automatically by forking all new kernel threads off of the *kthreadd* kernel process. The interface, declared in `<linux/kthread.h>`, for spawning a new kernel thread from an existing one is
```C
struct task_struct *kthread_create(int (*threadfn)(void *data),
                                    void *data,
                                    const char namefmt[],
                                    ...)
```

The new task is created via the `clone()` system call by the *kthread* kernel process. The new process will run the `threadfn` function, which is passed the `data` argument. The process will be named `namefmt`, which takes *printf-style* formatting arguments in the variable argument list. The process is created in an unrunnable state; it will not start running until explicitly woken up via `wake_up_process()`. A process can be created and made runnable with a single function, `kthread_run()`:
```C
struct task_struct *kthread_run(int (*threadfn)(void *data),
                                    void *data,
                                    const char namefmt[],
                                    ...)
```

This routine, implemented as a macro, simply calls both `kthread_create()` and `wake_up_process()`:
```C
#define kthread_run(threadfn, data, namefmt, ...)               \
({                                                              \
    struct task_struct *k;                                      \
                                                                \
    k = kthread_create(threadfn, data, namefmt, ## __VA_ARGS__; \
    if (!IS_ERR(k))                                             \
    wake_up_process(k);                                         \
    k;                                                          \
})
```
When started, a kernel thread continues to exist until it calls `do_exit()` or another part of the kernel calls `kthread_stop()`, passing in the address of the `task_struct` structure returned by `kthread_create()`:
```C
int kthread_stop(struct task_struct *k)
```
We discuss specific kernel threads in more details in later chapters.