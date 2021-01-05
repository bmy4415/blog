## This is TIL (Today I Learned) for logging what I learned

##### 2021.01.05 (3)
- https://serverfault.com/questions/843243/ssh-jumping-with-aliases-and-f
  - proxyhost에 관한 ssh config
  - ssh -F <config file>을 이용하면 ProxyCommand directive에서 현재 config를 사용하지 않아서 문제가 생김 => `ProxyJump` directive 이용하거나 Proxycommand에 -F 추가해줘야함

##### 2021.01.03 (2)
- https://dev.to/oxodesign/series/8187
  - typescript + jest + express + eslint + prettier 셋업
- https://www.ironin.it/blog/a-simple-kubernetes-cluster-on-aws.html
  - aws EC2와 kubespray를 이용한 k8s cluster 구축
  - https://snowdeer.github.io/kubernetes/2018/02/13/kubernetes-can-not-use-kubectl/
    - kubectl 명령어 이용했을 때 `The connection to the server localhost:8080 was refused - did you specify the right host or port?` 에러 발생
      - `/etc/kubernetes/admin.conf`를 `$HOME/.kube/config`에 옮겨주면됨

##### 2021.01.02 (1)
- https://subicura.com/k8s/ (쿠버네티스 튜토리얼)
  - service / ingress / volume / configmap 실습  

---

##### 2020.11.06
- mongoose + typescript + subdocument
  - typescript + mongoose를 이용할 때, 보통 하나의 collection을 표현하기위해 다음과 같이 `Document`를 상속하는 interface를 만든다.
    ```
    import { Document } from 'mongoose';
    export interface User exnteds Document {
      username: string;
      age: number;
    }
    ```
  - 그런데 subdocument를 이용할 때는, subdocument에 해당하는 interface가 `Document`를 상속하지 않아야 된다. (상속하면 type error가 뜬다.)
    ```
    import { Document } from 'mongoose';

    # Correct
    export interface Score {
      subject: string;
      score: number;
    }

    # Incorrect
    export interface Score extends Document {
      subject: string;
      score: number;
    }

    export interface User extends Document {
      username: string;
      age: number;
      scores: Score[];
    }
    ```
  - reference
    - https://medium.com/@agentwhs/complete-guide-for-typescript-for-mongoose-for-node-js-8cc0a7e470c1

---

##### 2020.11.01
- lint and formatter
  - lint
    - 직접 source code를 보고 버그, 컨벤션 등에 문제가 있는지 확인하는 tool
  - formatter
    - lint가 버그를 탐지할 수 있는데에 비하여 formatter은 주로 코드의 형식, 컨벤션 등을 확인한다.
  - 예시
    - javascript
      - eslint (lint) + prettier (formatter)
      - 두 tool을 서로 연동하여 사용할 수 있음
    - python
      - pylint (lint) + black (formatter)을 각각 사용

---

##### 2020.10.22
- DNS (Domain Name System)
  - 컴퓨터를 나타내는 주소는 크게 2가지가있는데, 하나는 IP주소, 다른 하나는 domain name이다. 예를들어 google.com 이라는 주소창에 입력하는 문자열은 domain name이라고 하고 172.217.26.14라고 입력하는 숫자는 IP주소라고 한다.
  - IP주소는 숫자로 이루어져있어 상대적으로 사람이 외우기 어렵기 때문에 보통은 domain name을 이용한다. 이때 domain name과 IP주소를 연결해주는 system을 DNS라고 한다. 즉 google.com이라는 주소는 사실은 172.217.26.14라는 IP주소로 어딘가에 매핑이되어있다.
  - **즉 DNS는 domain name과 IP주소의 mapping을 저장하고있는 database라고 이해할 수 있다.** (실제로는 (domain name, ip주소) 뿐만아니라 (domain name, domain name) 등 다양한 종류의 mapping을 저장하고있다.)
  - 각각의 maaping을 DNS Record라고 부르는데, 이 record에는 A, MX, CNAME, SOA등 여러가지 종류가 있다.
    - https://win100.tistory.com/360 참고
    - MX (Email Exchange) record는 아주 간단하게 말하면 email에 대한 DNS record이다. 예를들어 내가 `mydomain.com`이라는 domain name을 구입하고 `asdf@mydomain.com`과 같은 메일 서버를 구축하고 싶을 때, 메일서버를 직접 구축할 수도 있지만 naver, google 등에서 제공하는 메일서버 서비스를 이용할 수도 있다. 이때 MX record로 mydomain.com과 naver/google 등 이메일 서버 제공 업체의 domain을 연결하면, `asdf@mydomain.com`과 같은 이메일을 사용할 수 있다.
    - A record는 (domain name, IP주소)의 mapping을 저장하는 record이다.
    - CNAME record는 (domain name, domain name)의 mapping을 저장하는 record이다.


---

##### 2020.10.20
- .dockerignore
  - Every file/directory exists in same level with `Dockerfile` is called `context`.
  - `Context` is included when you create image from `Dockerfile`.
  - Similar with `.gitignore`, `.dockerignore` excludes files when creating image from `Dockerfile`
  - For example, you can ignore `.vscode`, `.git`, `node_modules` directories which is not necessary for docker image.

---

##### 2020.10.06
- npx
  - npx is, in short, node package `runner`.
    - npx runs node pakcages.
  - npm is node package `manager`.
    - npm manages dependencies of node packages.
  - Use case
    - `create-react-app` is CLI (command line interface) which creates bootstraped react app.
    - `cretae-react-app` comnand is not used frequently. You only run this command at the start of creating react project.
    - Since most javascript pakcages are changed frequently, installing and managing `create-react-app` globally is tricky.
    - You can use `npx` in this case. npx just runs (actually, install -> run -> delete) pakcages instead of installing it

---

- 2020.09.29
  - .editorconfig
    - I found this file first at https://github.com/asdf-vm/asdf
    - This file contains general config of editor setting
    - This file is not dependent on specific IDE. I.e, many IDE supports this.
      - E.g, in case of VSCode, https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig
    - More reference
      - https://blog.cookapps.io/guide/editorconfig/#%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95
  - asdf
    - asdf is powerful CLI which manages runtime version of many many languages
    - https://github.com/asdf-vm/asdf
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
