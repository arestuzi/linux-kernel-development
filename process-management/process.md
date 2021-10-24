# Process Management
This chapter introduces the concept of the `process`, one of the fundamental abstractions in Unix operating systems. It defines the process, as well as related concepts such as threads, and then discuss how the Linux kernel manages each process: how they are enumerated within the kernel, how they are created, and how they ultimately die. Because running user applications is the reason we have operating system, the process management is a crucial part of any operating system kernel, including Linux.

## The Process
A `process` is a program(object code stored on some media) in the midst of execution. Processes are, however, more than just the executing program code (often called the `text section` in Unix). They also include a set of resources such as open files and pending signals, internal kernel data, processor state, a memory address space with one or more memory mappings, one or more `threads of execution`, and a `data section` containing global variables. Processes, in effect, are the living result of running program code. The kernel needs to manage all these details efficiently and transparently. 

Threads of execution, often shortened to `threads`, are the objects of activity within the process. Each thread includes a unique program counter, process stack, and set of processor registers. The kernel schedules individual threads, not processes. In traditional Unix systems, each process consists of one thread. In modern systems, however, multi-threaded programs --- those that consist of more than one thread --- are common. As you will see later, Linux has a unique implementation of threads: It does not differentiate between threads and processes. To Linux, a thread is just a special kind of process.

On modern operating systems, processes provide two virtualizations: a virtualized processor and virtual memory. The virtual processor gives the process the illusion that it alone monopolizes the system, despite possibly sharing the processor among hundreds of other processes. Chapter 4, "Process Scheduling", discusses this virtualization. Virtual memory lets the process allocate and manage memory as if it alone owned all the memory in the system. Virtual memory is covered in Chapter 12, "Memory Management". Interestingly, note that threads share the virtual memory abstraction, whereas each receives its own virtualized processor.

A program itself is not a process; a process is an **active** program and related resources. Indeed, two or more processes can exist that are executing the same program. In fact, two or more processes can exist that share various resources, such as open files or an address space.

A process begins its life when, not surprisingly, it is created. In Linux, this occurs by means of the `fork()` system call, which creates a new process by duplicating an existing one. The process that calls `fork()` is the `parent`, whereas the new process is the `child`. The parent resumes execution and the child starts execution at the same place: where the call to `fork()` returns. The `fork()` system call returns from the kernel twice: once in the parent process and again in the newborn child.

Often, immediately after a fork it is desirable to execute a new, different program. The `exec()` family of function calls creates a new address space and loads a new program into it. In contemporary Linux kernels, `fork()` is actually implemented via the `clone()` system call, which is discussed in a following section.

Finally, a program exists via the exit() system call. This function terminates the process and frees all its resources. A parent process can inquire about the status of a terminated child via the `wait4()` system call, which enables a process to wait for the termination of a specific process. When a process exists, it is placed in to a special zombie state that represents terminated processes until the parent calls `wait()` or `waitpid()`.

> Note
> 
> Another name for a process is a task. The Linux kernel internally refers to processes as tasks. In this book, I use the terms interchangeably, although when I say `task` I am generally referring to a process from the kernel's point of view.

[`Previous Page`](../introduction-to-the-linux-kernel/linux-kernel-version.md) [`Next Page`](process-descriptor-and-task-structure.md) [`Table`](../README.md)