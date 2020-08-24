## This is TIL (Today I Learned) for logging what I learned

- 2020.08.24
  - python mutable vs immutable
    - In python, if an object is changable, we call it `mutable`. On the other hand, if an object is not changable, we call it `immutable`
      - E.g, list is mutable because we can change list after it's creation
      - E.g, tuple is immutable because we cannot change tuple once it's created
    - Why python provides `mutable` and `immutable`?
      - Comparison is very cheap for immutable objects because we only compare address of obejct. Immutable is proper for primitive types.
      - Mutable is more efficient for some scenarios
        - E.g, when we have huge array and we frequently change this array, mutable is much more effective than immutable. If array is immutable, we need to copy huge array whenever we change this array.
    - **Tricky case**
      -
      ```
      # temp.py
      
      a = [1,2,3]
      b = [4,5,6]
      c = (a, b)
      print(c)  # ([1,2,3], [4,5,6])
      a[0] = 100
      print(c)  # ([100,2,3], [4,5,6])
      ```
      - Tuple c contains `address` of a and b. So, tuple cannot resist changes on a.
- 2020.08.23
  - linux redirection
    - In linux, redirection means changing a stream to another stream, e.g, changing stdout stream to file stream.
    - Each process in linux usually has 3 stream by default, stdin(usually keyboard), stdout(usually screen), stderr(usually screen).
    - `/dev/null` is special file which discards all data written to it.
    - For example, let's review this command: `python test.py > output.txt 2> error.txt`
      - `2` means stderr and `>` means redirection stream and overwrite it. So all error messages generated from test.py will be written to error.txt
      - `1` stdout and you can skip writting `1` to command. stdout test.py will be over-written to output.txt
      - For further information, `>>` also means redirection but append to stream. 
    - For example, lets' review this command: `ls 2>/dev/null`
      - Above commands run ls command and does not print any errors at screen because `2>/dev/null` means sending all error messages to `/dev/null` which discards received logs
      - `2>/dev/null` is used when we don't want to print error messages to screen
      - For further information, `python test.py > output.txt 2>&1` means writting both stdout and stderr to output.txt
- 2020.08.11
  - PIPE
    - one possible way of IPC, i.e, communication between 2 processes
    - when we call `A | B`, stdout of A will be stdin of B
    - PIPE buffer resides in kernel space and it's size varies with OS. Typically 65536 bytes
    - stdin buffer is differ from PIPE buffer. Size of stdin buffer is usually size of page.
    - useful references
      - https://mug896.github.io/bash-shell/buffering.html
      - http://www.pixelbeat.org/programming/stdio_buffering/
- 2020.08.09
  - socket
    - network consists of many layers (e.g, OSI 7 layers, TCP/IP 4 layers)
    - socket provides abstraction on network on behalf of developer. Developer can use socket without considering many layers, e.g, network access / internet layer in TCP/IP 4 layers
    - TCP socket
      - uses (IP, PORT) as namespace
      - used for network communication
    - UNIX domain socket
      - uses filesystem as namespace
      - used for IPC (inter process communication)
    - in python, we can use built in `socket` module to communicate using socket
      - [Unix domain socket example](https://pymotw.com/3/socket/uds.html)
      - [TCP socket example](https://wiki.python.org/moin/TcpCommunication)
- 2020.07.30
  - In python, to run multiple bash commands with subprocess, e.g, `echo "hi"; echo "hello"`, you must pass argument `shell=True`.
  - subprocess cannot parse `;` therefore cannot run multiple commands
- 2020.07.29
  - To write `testable` code, avoid global variables. Instead, inject them into each function's parameter list
- 2020.07.27
  - __init__.py file in python language
    - **specify current working directory is part of package** which can be imported
    - it this file does not exist, there must be an import error

- 2020.07.22
  - linux uniq command
    - http://2min2code.com/articles/uniq_command_intro
    - example) `ls -al /proc/24686/fd | awk '{print $11}' | uniq` will show unique lines
    - `uniq -u` only prints lines that are unique, i.e, if duplicates exists, this does not print any line
    - `uniq -d` only prints lines that has duplicate lines
    - `uniq -c` prints count for each line
- 2020.07.20
  - Counter in python
  - Counter is standard library of python, so you can use after import
    - `from collections import Counter`
  - Counter counts number of elements in iterable objects and return dict-like object
    ```
    # with Counter
    from collections import Counter
    arr = [1,2,3,1,2,3,1,2,3]
    counter = Counter(arr)
    counter.items() # dict_items([(1, 3), (2, 3), (3, 3)])
    counter[1]      # 3

    # without Counter
    counter = dict()
    arr = [1,2,3,1,2,3,1,2,3]
    for num in arr:
      if num in counter:
        counter[num] += 1
      else:
        counter[num] = 1

    counter # {1: 3, 2: 3, 3: 3}
    ```
- 2020.07.19
  - topological sort
    - ordering of vertices in graph such that edge (u, v) leads to order of u -> v
    - example code [link](https://github.com/BaeInpyo/ProgrammingStudy/blob/master/leetcode/july_challenge/course_schedule_2/sjw.py)
    - there are 2 soltuions
      - BFS
        ```
        indegree <= count indegree
        queue = [vertex with 0 indegrees]
        answer = []
        while queue:
          curr = queue.pop()
          answer.append(curr)
          for next_vertex in curr.neighbors:
            indegree[next_vertex] -= 1
            if indegree[next_vertex] == 0:
              queue.append(next_vertex)

        if len(answer) < number of vertices:
          return -1 # this means cycle exists

        return answer
        ```
      - DFS
        ```
        keep 2 lists, visited and finished to detect cycle
        visited = [False for all vertices]
        finished = [False for all vertices]
        answer = []

        for vertex in all_vertices:
          if indegree[vertex] == 0:
            dfs(vertex)

        dfs(src, visited, finished, answer):
          visited[src] = True

          for next_vertex in src.neighbors:
            if visited[next_vertex] but not finished[next_vertex]:
              return # this means cycle

            if not visited[next_vertex]:
              dfs(next_vertex)

          finished[src] = True
          answer.append(src)

        # check if cycle exists
        if len(answer) < number of vertices:
          return -1

        return answer[::-1] # reverse order of bfs is answer of tpsort
        ```
- 2020.07.10
  - in linux, fd (file descriptor) limit is applied per process ([link](https://unix.stackexchange.com/questions/55319/are-limits-conf-values-applied-on-a-per-process-basis))
  - you can check limits of running process with `cat /proc/<pid>/limits`
  - you can check currently open fds for process at `ls -al /proc/<pid>/fd`
  - yum (package manager) uses yum repos (typically http url) located at `/etc/yum.repos.d/*.repo` to find packages. So if you want to install some packages with certain version unshown in `yum list --showduplicates` command, you can download .repo file which contains the version you want directly from internet and copy it to /etc/yum.repos.d. Then you can find/install the version you want
