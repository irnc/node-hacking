# Prerequisites

Ability to answer following questions:

- What is a file descriptor?
- How operating system represents and manages a _pipe_ resource?

## References

- http://stackoverflow.com/questions/5256599/what-are-file-descriptors-explained-in-simple-terms
- Advanced Programming in the Unix Environment, Book by W. Richard Stevens
- http://www.cs.stevens.edu/~jschauma/810/
- https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/3_Processes.html
- http://poincare.matf.bg.ac.rs/~djenic/apue.pdf

# `epoll (7)`

- https://banu.com/blog/2/how-to-use-epoll-a-complete-example-in-c/
  - epoll does not perform any linear scans
  - `epoll_create`, `epoll_ctl`, `epoll_wait` all (2)
- edge-triggered interface
  - http://stackoverflow.com/questions/9162712/what-is-the-purpose-of-epolls-edge-triggered-option
  - see description at `man 7 epoll`

## `select` and `poll`

- `O(n)` complexity on number of controlled descriptors

> The  poll()  system call was introduced in Linux 2.1.23. On older kernels that
lack this system call, the glibc (and the old Linux libc) poll() wrapper
function provides emulation using select(2).
>
> _cite from `man 2 poll`_

# No libuv

- http://blog.kazuhooku.com/2014/09/the-reasons-why-i-stopped-using-libuv.html
- https://github.com/h2o/h2o
