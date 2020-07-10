## This is TIL (Today I Learned) for logging what I learned
- 2020.07.10
  - in linux, fd (file descriptor) limit is applied per process ([link](https://unix.stackexchange.com/questions/55319/are-limits-conf-values-applied-on-a-per-process-basis))
  - you can check limits of running process with `cat /proc/<pid>/limits`
  - you can check currently open fds for process at `ls -al /proc/<pid>/fd`
