## This is TIL (Today I Learned) for logging what I learned

##### 2021.07.19 (45)
- datagrip sslhandshake 에러
    - `javax.net.ssl.SSLHandshakeException: No appropriate protocol (protocol is disabled or cipher suites are inappropriate).`
    - TLS version과 관련된 문제로 `Data source and drivers` > `Advanced` > `Enabled TLS protocols`에서 값을 `TSLv1.2`로 바꿔주면됨
    - reference
        - https://intellij-support.jetbrains.com/hc/en-us/community/posts/360010798199-DataGrip-upgraded-to-2021-1-2-error-occurs-when-connecting-to-database-SSLHandshakeException-
- kotlin object 키워드
    - singleton 객체 만들기
    - static 멤버변수/멤버함수 선언에 이용
    - 익명 객체에 이용
    - reference
        - https://0391kjy.tistory.com/29#:~:text=%EC%BD%94%ED%8B%80%EB%A6%B0%EC%97%90%EC%84%9C%EB%8A%94%20%22object%22%20%ED%82%A4%EC%9B%8C%EB%93%9C,%EB%A5%BC%20%EC%83%9D%EC%84%B1%ED%95%9C%EB%8B%A4%EB%8A%94%20%EC%A0%90%EC%9E%85%EB%8B%88%EB%8B%A4.

---

##### 2021.07.08 (44)
- NUMA (Non Uniform Memory Access)
    - RAM(Random Access Memory)에서는 메모리의 상의 어느위치나 자유롭게 접근 가능
    - NUMA에서는 각 cpu core마다 local memory 영역을 할당하여 ( 예를들어 4CPU 512GM RAM인 경우 1개의 CPU에 128G씩 local memory 영역 할당 ) local memory에 대해서는 훨씬 더 빠른 접근속도를 보장해줌
    - 기존의 SMP(Symmetric Multi Processing)에서는 하나의 IO bus를 통해 여러 CPU와 RAM이 소통하지만 NUMA에서는 마치 multi bus인것처럼 작동하여 성능상의 이점을 가져갈 수 있음

    - reference
        - https://namu.wiki/w/NUMA
        - https://wogh8732.tistory.com/399

---

##### 2021.07.06 (43)
- mac multiple java versions setup
    - multiple java versions라 함은 곧 multiple jdk versions라는 말이다.
    - 여러개의 jdk를 설치한 후 [jenv](https://github.com/jenv/jenv)를 통해 global 또는 local(directory마다 다르게)로 관리할 수 있다.
    - [openjdk](https://formulae.brew.sh/formula/openjdk) 또는 [AdoptOpenJDK](https://github.com/AdoptOpenJDK/homebrew-openjdk) 중에 이용하면 된다.
- jdk
    - java가 유료라고 할 떄 java language가 유료라는 의미가 아니라 jdk가 유료라는 말이다.
    - jdk는 java compiler를 포함하는데 이를 통해 java source code를 java bytecode로 변환할 수 있다.
    - jdk는 여러 구현체가 있다. (마치 python 구현체가 CPython과 pypy등이 있는것처럼)
        - Oracle JDK
        - open JDK
            - AdoptOpenJDK
            - Amazon Corretto
            - etc
    - reference
        - https://stackoverflow.com/questions/52431764/difference-between-openjdk-and-adoptium-adoptopenjdk
        - https://broccolies.com/2021/04/oracle-jdk%EC%99%80-open-jdk%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EB%B9%84%EA%B5%90/#:~:text=%EB%8F%99%EC%9D%BC%ED%95%9C%20%EA%B8%B0%EB%B0%98%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%AF%80%EB%A1%9C,%EB%A5%BC%20%EB%8D%94%20%EC%9E%90%EC%A3%BC%20%EC%A0%9C%EA%B3%B5%ED%95%A9%EB%8B%88%EB%8B%A4.
- jvm / jre / jdk
    - java로 작성된 application의 실행 순서는 java source code => compiled java code (java byte code) => run on jvm => jvm created machine code ( os/machine specific ) 이다.
    - 즉 **javac**를 이용해서 **java byte code**를 만들고 이를 **JVM**에서 실행한다. **JVM**은 platform(os)에 맞게 **bytecode**를 생성하여 실행한다.
    - jvm (java virtual machine)
        - java bytecode를 input으로 machine code를 output으로 낸다. 즉 각 os(linux, windows, mac 등)마다 JVM이 생성하는 machine code는 다르지만 input으로 받는 java bytecode는 동일하다. 그로인해 똑같은 java source code를 JVM을 통해 다양한 os에서 실행할 수 있다.
    - jre (java runtime environment)
        - JVM + class files (standard library 등)
        - JVM은 혼자 설치할 수 없고 JRE와 같이 설치된다.
    - jdk (java development kit)
        - jre에 compiler, debugger 등 개발에 필요한 추가도구를 포함한다. 
        - `java를 실행만 할 때는 JRE면 충분하고 개발을 할 때는 JDK가 필요하다.`
    - JVM < JRE < JDK 의 포함순서가 있다.
    - 보통은 개발용으로 java를 설치하기때문에 jdk를 설치하고 server에서 돌리는 등의 특별한 경우 jre를 설치하면 된다.
    - reference
        - https://devpouch.tistory.com/9
        - https://www.guru99.com/difference-between-jdk-jre-jvm.html

---

##### 2021.06.23 (42)
- ORM (Object Relation Mapping)
    - db에 접근할 때 raw sql로 접근하는대신 Relation (db의 table)을 Object (프로그래밍언어에서의 객체)로 mapping하여 접근하는 방식
    - pros
        - db 변경했을 때 소스코드 변경이 필요없음
        - sql query를 잘 못짜는 사람에게 도움이 됨 (orm이 알아서 짜주니까)
        - prepared statement를 이용하기때문에 비교적 sql injection에 안전함
    - cons
        - raw sql을 직접 관리하는것에 비해 성능하락 가능성이 있음 (orm, db의 내부동작에 대해 정확히 이해하지 못하고 사용할 가능성이 높기때문에)
    - 각 언어별 대표적인 ORM framework는 다음과 같음
        - ruby: ActiveRecord
        - python: SQLAlchemy
        - javascript: Sequelize
        - java: Hibernate
    - 참고자료
        - https://medium.com/@violetmoon/understanding-orm-frameworks-with-ruby-and-activerecord-83a9e9d8490e
        - https://centralblue.co.uk/blog/2019/01/the-pros-and-cons-of-object-relational-mapping-orm
- uuid
    - unique한 id를 만들기위한 규약
    - 128bit이자 36개 문자 (32개 alpha-numeric + 4개 hypen)
    - v1 ~ v5까지있으며 주로 사용하는것은 v1(timestamp 기반), v4(랜덤 기반)
    - 참고자료
        - https://www.huskyhoochu.com/what-is-uuid/
        - https://ko.wikipedia.org/wiki/%EB%B2%94%EC%9A%A9_%EA%B3%A0%EC%9C%A0_%EC%8B%9D%EB%B3%84%EC%9E%90

- 리그레션 테스트 (regression test)
    - 소프트웨어를 변경했을 때 기존 기능이 잘 돌아가는지 확인하는 테스트
    - e.g) python으로 구축된 서버를 go로 전환했을 때 기존 서버에서 작동하던 기능들이 go로 전환한 서버에서도 잘 작동하는지 확인하는 테스트
    - 참고자료
        - http://jidum.com/jidums/view.do?jidumId=581 

---

##### 2021.06.21 (41)
- rustup / rustc / cargo / toolchain
    - rust를 설치하면 rustup / rustc / cargo 등의 executable이 PATH에 추가됨
    - toolchain
        - https://butter-shower.tistory.com/24
        - intelliJ-rust 등을 사용하다보면 `toolchain location`등 toolchain이라는 용어가 나오는데, 이는 실제 rust와 관련된 binary들이 저장된 장소이다
        - e.g) $HOME/.rustup/toolchains/nightly-x86_64-apple-darwin/bin
    - rustup
        - https://github.com/rust-lang/rustup/tree/cef627130947d2e7a20f88030c357b11d1c1bb1f#how-rustup-works
        - https://stackoverflow.com/questions/41947210/rustup-vs-cargo-binaries
        - rustup은 `toolchain multiplexer`, 즉 여러개의 rust version을 설치하고 관리하게 해줌
        - ~/.cargo에 있는 executable은 실제로는 ~/.rustup를 가르키는 proxy임 (symlink와 비슷하지만 file size를 봤을 때 symlink는 아닌듯)
    - rustc
        - https://doc.rust-lang.org/rustc/what-is-rustc.html#what-is-rustc
        - **rustc는 rust compiler이며 대부분의 경우 `rustc`를 직접 call하기보다 cargo를 통해 간접으로 call함**
    - cargo
        - https://doc.rust-lang.org/cargo/index.html
        - rust의 package manager
        - nodejs의 npm, python의 pip와 같이 dependent package를 설치해주기도 하고 rust는 compile 언어이므로 compile도 해줌
        - 내가 만든 package를 distribute할 때도 이용함
        - **rust로 개발할 때 command line에서 이루어지는 일의 대부분을 이 명령어가 담당함**
- database transaction
    - 여러개의 database query를 묶어서 하나의 atomic한 operation으로 만들어주는 기능
    - all or nothing을 보장하는것이지 txn(transaction)이 실패할 경우 auto-retry를 해주는 개념은 아니다
    - auto retry는 application logic에서 구현해야함 (database level에서 제공하지않음)
- aws ami (amazon machine image)
    - ec2 등의 instance를 생성할 때 사용할 image
    - docker image와 비슷한 개념으로 생각하면 됨
    - 
- kms (key management service)
    - data를 암호화할 때 사용하는 key를 관리해주는 서비스, e.g) [aws kms](https://aws.amazon.com/ko/kms/)
    - Oracle에서 cloud service를 개발할 때 경험했듯이 주로 특정 보안 요구사항을 충족시키기위해 사용
    - 참고자료
        - https://bluese05.tistory.com/71
- webview (웹뷰)
    - mobile app을 만들 때 모바일 기기에 내장된 web browser를 이용하여 view(화면)을 구성하는 것
    - 즉 HTML을 이용하여 mobile app 화면을 구성하는 것
    - 이와 같은 app을 hybrid app한다
    - 참고자료
        - https://hanamon.kr/%EC%9B%B9%EB%B7%B0-webview/
- hot wallet vs cold wallet
    - crypto currency는 wallet에 저장하고 wallet은 개인키(private key)가 있으면 제어할 수 있다
    - hot wallet은 인터넷에 연결된 wallet, 즉 private key가 인터넷상에 있는 wallet이며 대표적으로는 중앙화거래소 (binance 등) wallet이 있다
    - cold wallet은 인터넷에 연결되지 않은, 즉 offline wallet이고 USB 등의 hardware장치에 private key를 보관한다
    - private key의 보관이 online보다 offline이 (일반적으로) 안전하므로 cold wallet이 더 안전하다고 할 수 있다. 하지만 사용상의 편의는 hot wallet이 더 좋다고 할 수 있다.
- vscode vs android studio
    - 기존에 vscode를 쓰고있기때문에 안드로이드 개발을 vscode로할지 android studio로 할지 고민하다 jetbrain 계열 제품(android studio는 intelliJ 기반)을 직접 써보고싶어서 android studio로 결정
    - vscode에서도 android개발이 가능한것 같음(emulator도 제공)
    - android studio는 vscode와 keymap이 다른데 (mac기준 `cmd ⬇` 등의 동작에서 다름) 이때 keymap을 sublimetext로 바꾸면 vscode와 유사한 keymap이 됨 (**vscode로 바꾸면 vscode와 유사하지 않음**)
    - https://blog.codemagic.io/android-studio-vs-visual-studio-code/
- m1 mac android emulator
    - 2021.06.21 현재 apple m1 processor로는 android emulator를 직접 돌리기 불가능한 상황
    - 아래의 blog post를 보고 `android-emulator-m1-preview`를 이용하거나 android 휴대폰을 직접 mac에 연결하여 emulation 가능
        - `android-emulator-m1-preview`는 현재 v3이 latest release인데 새로운 release가 나올때마다 작업해주기는 번거로울것 같아서 일단은 직접 android 휴대폰을 연결하는 방법을 이용하기로 결정함
    - https://dev-repository.tistory.com/97 

---

##### 2021.06.13 (40)
- deep learning production with celery as task queue
    - https://towardsdatascience.com/serving-deep-learning-algorithms-as-a-service-6aa610368fde
        - long running task에는 celery같은 asynchronous task queue를 쓰면 좋은데 위 자료에 그 예시가 잘 설명되어있음
- rabbitMQ tutorial with python
    - https://blog.storyg.co/rabbitmqs/tutorials/python/01-hellowolrd

---

##### 2021.06.12 (39)
- mysql redo log
    - aws RDB에서 PI (Performance Insight)를 보면 db load를 확인할 수 있는데 그 중에서 `io redo log flush`와 같은 요소가 있었다
    - (일반적으로) database는 query를 수행하고 그 결과를 바로 disk에 write하지 않는다. 보통은 memory에만 수정내용을 반영하고 여러 수정내용을 묶어서 batch로 disk file에 write한다. 이렇게함으로써 disk I/O를 줄여 성능향상을 꾀할수 있다. 조금 더 구체적으로 말하자면 inno db buffer pool -> inno db log buffer -> os buffer -> disk 순으로 데이터의 이동이 발생한다. `Innodb_flush_log_at_trx_commit` 파라미터를 이용해서 그 level을 설정할 수 있다.
    - reod log는 그 이름에서 알 수 있듯이 그 내용을 실행함으로써 db의 state를 다시 재현할 수 있는 log이다. 위의 과정에서 inno db log buffer에는 redo log가 채워진다.
    - 참고자료
        - https://sarc.io/index.php/mariadb/1146-innodb-redo-logging-process
        - https://dus815.tistory.com/entry/Mysql-Redo-Log-%EB%9E%80
- mysql bin log
    - redo log는 mysql의 engine인 innodb level에서 생성되는 log이고 bin log는 mysql 자체, 즉 mysql server 수준에서 생성되는 log이다.
    - 참고자료
        - http://cloudrain21.com/mariadb-binary-log

- postgresql vacuum
    - 직접 postgresql을 쓰지 않기때문에 상세하게 다루기보다는 어떤 개념인지만 정리하자면, 다른 RDBMS와 다르게 postgresql(이하 pgsql)에서는 row에 대한 update나 delete가 일어날 때 해당 row를 disk에서 update/delete하지않고 새로운 영역에 write한다. 즉 시간이 지날수록 사용되지않는 영역이 늘어나고 clear해주는것이 vacuum이다.
    - 참고자료
        - http://hochul.net/blog/postgresql-vacuum-optimization/

---

##### 2021.06.08 (38)
- db size
    - database 크기를 확인할 수 있는 query
    ```sql
    SELECT 
        table_name,
        table_rows,
        round(data_length/(1024*1024*1024),2) as 'DATA_SIZE(GB)',
        round(index_length/(1024*1024*1024),2) as 'INDEX_SIZE(GB)'
    FROM information_schema.TABLES
    where table_schema = '<Your database name>'
    GROUP BY table_name 
    ORDER BY data_length DESC 
    LIMIT 10;
    ```
- db partitioning
    - table의 크기가 너무 커서 query의 성능이 떨어질 때 table을 분리하여 성능을 개선시키는 행위
    - table의 row를 분산시키는 horizontal partitioning과 column을 분산시키는 vertical partitioning이 있음
    - horizontal partitioning은 sharding이라고도 부름
    - partitioning을 할 경우 table의 크기가 작아지기때문에 단일 table에 대한 query의 성능은 증가하지만 join의 성능은 떨어짐

    ```sql
    # Partition 예시 query
    CREATE TABLE tb_gillog (

    	id INT NOT NULL,
    	write_date DATETIME NOT NULL,
    	...
    	PRIMARY KEY(id)
    	
    )

    PARTITION BY RANGE ( YEAR(write_date) ) (
    	
    	PARTITION p2018 VALUES LESS THAN (2019),
    	PARTITION p2019 VALUES LESS THAN (2020),
    	PARTITION p2020 VALUES LESS THAN MAXVALUE
    	
    );
    ```

    - 참고자료
        - [https://velog.io/@gillog/MySQL-Partition](https://velog.io/@gillog/MySQL-Partition)
        - [https://gmlwjd9405.github.io/2018/09/24/db-partitioning.html](https://gmlwjd9405.github.io/2018/09/24/db-partitioning.html)
- db buffer cache
  - buffer pool과 비슷한 의미로 사용됨
  - 일반적으로 table의 모든 data를 memory에 적재할 수 없으므로 일부 data만 memory에 적재하는데 적재할 때 사용하는 memory를 buffer cache 또는 buffer pool이라고 부름
  - 당연하게도 buffer cache (= buffer fool)의 size가 클수록 db의 성능이 좋아짐
  - 보통 db만 떠있는 instance의 경우 buffer cache의 크기를 memory의 80%정도로 하면 좋다고함
  - 참고자료
    - https://dus815.tistory.com/entry/Inoodb-buffer-pool-%EC%9D%B4%EB%9E%80
    - http://cloudrain21.com/mysql-innodb-basic-performance-tunning
    - https://owlyr.tistory.com/23

---

##### 2021.04.20 (37)
- celery
  - https://daeguowl.tistory.com/157
    - queue에 있는 작업이 실행됨을 보장하기위해 `late_ack`와 같은 옵션을 사용할 수 있다. 이 옵션은 worker가 작업을 시작할 때 queue에서 task를 제거하는게 아니라 작업을 완료하고 ack가 날아오면 그제서야 task를 제거한다. task가 반드시 실행된다는 보장은 있지만 `한번만`실행된다는 보장은 없으므로 `late_ack를 사용하기 위해서는 task가 idempotent(여러번 작동해도 같은 결과를 냄) 해야한다.`
      - `visibility_timeout` 이내에 ack가 안오면 해당 task를 새로운 worker에 다시 할당
    - prefetch / prefork를 이용하여 performance tuning을 할 수 있다.
      - cpu bound / IO bound task마다 설정 방법이 

---

##### 2021.04.16 (36)
- eks에서 user에게 kubectl 사용할 수 있는 권한 추가
  - https://aws.amazon.com/ko/premiumsupport/knowledge-center/amazon-eks-cluster-access/
  - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
  - 2번째 link에서 `k get cm/aws-auth -n kube-system -o yaml > temp.yaml`와 같은 방법으로 config map을 파일로 추출할 수 있음
  - 1번째 link에서 보듯이 user의 arn을 추가하면 해당 user에게 kubectl 사용 권한을 줄 수 있음

---

##### 2021.04.10 (35)
- ubuntu에서 cuda 삭제 후 재설치
  - https://forums.developer.nvidia.com/t/cuda-install-unmet-dependencies-cuda-depends-cuda-10-0-10-0-130-but-it-is-not-going-to-be-installed/66488/5

---

##### 2021.04.09 (34)
- Use GPU inside docker container
  - https://marmelab.com/blog/2018/03/21/using-nvidia-gpu-within-docker-container.html
  - https://stackoverflow.com/questions/47465696/how-do-i-specify-nvidia-runtime-from-docker-compose-yml
  - Docker container에서 GPU를 사용하려면 Nvidia-docker를 이용해야함

---

##### 2021.03.31 (33)
- ELB / nginx timeout
  - https://aws.amazon.com/ko/premiumsupport/knowledge-center/apache-backend-elb/
  - http://nginx.org/en/docs/http/ngx_http_core_module.html#client_header_timeout
  - nginx 설정에 보면 여러종류의 timeout이 있는데 이 중에서 `client_header_timeout`, `client_body_timeout`은 client가 보낸 요청의 header/body를 읽는데 걸리는 시간을 제한하는 parameter이고 `keepalive_timeout`은 한번 맺은 TCP connection을 재사용하는 option으로 client와 server사이의 connection을 유지하는 parameter이다.
  - aws ELB와 연관지어 생각해보면 ELB idle timeout과 nginx의 keepalive timeout의 값을 설정할 때, EC2 instance의 nginx가 keepalive timeout에의해 connection을 close했을 때 이를 ELB쪽에 알릴 수 있는 방법이 없으므로 일반적으로 `ELB idle timeout < nginx keepalive timeout`으로 설정하여야 ELB쪽에서 `504 Timeout Error`를 방지할 수 있다.

---

##### 2021.03.30 (32)
- elb 502 error while ec2 nginx returns 200
  - aws eb를 이용하여 server는 관리하는 환경임
  - eb는 자체적으로 elb / auto scaling group을 설정해줌
  - cpu 사용량이 많고 response time이 상당히 긴 (upto 300s) http request를 받는 서버가 있는데 서버 ec2의 nginx log에서는 200을 보내주는데 client는 자꾸 502를 받음
  - 확인해보니 elb가 ec2에 보내는 healthcheck의 return이 원활하게 오지 않았고 그 결과 elb가 해당 ec2를 `unhealthy`로 간주하였음. `**정확한 이유는 모르겠지만 unhealthy ec2에서 오는 응답은 elb가 자체적으로 5XX 에러코드를 설정하는것같음**`
- 

##### 2021.03.29 (31)
- k8s autoscaling
  - https://www.replex.io/blog/kubernetes-in-production-best-practices-for-cluster-autoscaler-hpa-and-vpa

---

##### 2021.03.26 (30)
- python regex unicode
  - 특정 string이 japanese character을 포함하는지 확인해야하는 일이 있었음
  - https://stackoverflow.com/questions/43418812/check-whether-a-string-contains-japanese-chinese-characters
  ```
  # 아래와 같이 사용하면 unicode range를 이용하여 regex 사용 가능 
  pattern = re.compile(r'[\u3040-\u30ff\u3400-\u4dbf\u4e00-\u9fff\uf900-\ufaff\uff66-\uff9f]', re.UNICODE)
  ```

---

##### 2021.03.25 (29)
- ts-node 사용중 `Cannot find module 'typescript'`
  - https://github.com/TypeStrong/ts-node/issues/707
    - `typescript`를 global하게 설치해야함

---

##### 2021.03.22 (28)
- aws ec2 cpu
  - aws ec2는 vm기반이다. 즉 aws 데이터센터에 실제 머신 (physical machine)을 들여놓고 그 내부에 아주 높은 수준의 가상화를 통해서 vm을 만든다. 여기서 높은 수준이란 pm과 동일한 수준의 vm을 의미한다. 그래서 ec2 instance type을 보면 CPU가 아니라 `vCPU`라는 용어가 있다. 이는 pm의 CPU (core와 thread)를 나누어 vm에 할당하고 vm내부에서는 다시 CPU 가상화를 통해 vCPU를 만든다. 아래의 예시는 c5.xlarge instace type의 cpuinfo이다.
```
[ec2-user@ip-192-168-88-94 ~]$ cat /proc/cpuinfo
processor	: 0
model name	: Intel(R) Xeon(R) Platinum 8275CL CPU @ 3.00GHz
siblings	: 4
cpu cores	: 2
...

processor	: 1
model name	: Intel(R) Xeon(R) Platinum 8275CL CPU @ 3.00GHz
siblings	: 4
cpu cores	: 2

processor	: 2
model name	: Intel(R) Xeon(R) Platinum 8275CL CPU @ 3.00GHz
siblings	: 4
cpu cores	: 2

processor	: 3
model name	: Intel(R) Xeon(R) Platinum 8275CL CPU @ 3.00GHz
siblings	: 4
cpu cores	: 2
...
```
  - `processor`
    - vCPU의 cpu번호를 의미한다.
  - `siblings`
    - pm의 cpu에서 이 vm에 할당된 실제 thread의 갯수를 의미한다. model name에 나와있는 `Intel(R) Xeon(R) Platinum 8275CL CPU @ 3.00GHz` cpu는 24core/48thread인데 이 중에서 현재 vm은 4thread를 사용중인 것이다.
  - `cpu cores`
    - pm의 cpu에서 이 vm에 할당된 실제 core의 갯수를 의미한다. 즉 24core중 현재 vm은 2core를 할당받은 것이다.
  - `processor`에서 약간 헷갈릴 수 있는데, 이는 vCPU의 core이므로 실제 2core/4thread를 가상의 4cpu로 분할한 것이다.

---

##### 2021.03.19 (27)
- kubeconfig
  - https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
    - kubectl을 이용해 kubernetes cluster와 통신하기위해서는 kubeconfig가 필요하다. kubeconfig는 `clusters`, `users`, `contexts`의 3가지 부분으로 구성되어있다.
    - 여러개의 k8s cluster에 접근할때는 각 cluster별로 kubeconfig파일을 따로 가지고 있는것이 관리의 측면에서 더 좋다.
  - https://ahmet.im/blog/mastering-kubeconfig/
    - kubeconfig에 관한 이해에 도움이 되는 글
    - 하나의 kubeconfig file에 정의된 여러개의 context를 분리(split)할 수도 있고, 여러개의 kubeconfig file에 정의된 여러개의 context를 하나의 kubeconfig file로 merge할 수도 있음
---

##### 2021.03.17 (26)
- k8s resource request vs limit
  - https://bcho.tistory.com/1291
    - request는 최소량, limit은 최대량
    - [blog](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-resource-requests-and-limits)에 따르면 request와 limit을 같게하고 cpu의 경우 데이터베이스 등의 무거운 app이 아니면 100m이하로 하는게 좋다고함
  - https://itchain.wordpress.com/2018/05/16/kubernetes-resource-request-limit/
    - limit은 한계선으로 설정하고 request는 평소사용량 +10% 정도로 설정하면 좋음 그러나 정답은 없는듯함
  - https://bcho.tistory.com/1291
    - cpu는 1이 1개의 vCPU 또는 cpu core를 나타낸다. 100m은 0.1과 같다.
    - memory는 Mi, Gi를 이용하여 byte 단위로 나타낸다.
- aws alb ingress
  - 현재는 ingress의 apiVersion을 `extensions/v1beta1`를 사용해야함 (network.k8s.io/v1를 이용하려다가 path rewrite가 안되는 문제로 삽질하였음)

---

##### 2021.03.15 (25)
- ssh to eks work node
  - https://github.com/awslabs/amazon-eks-ami/issues/161
  - eksctl로 eks cluster를 만들었을 경우 worker node로 ssh를 통해 접근할 수 있고 이때 user는 `ec2-user`를 이용하면 됨
- 읽어보기
  - https://phoenixnap.com/kb/understanding-kubernetes-architecture-diagrams
  - https://phoenixnap.com/blog/kubernetes-monitoring-best-practices
  - https://blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=221535575875
- k8s DaemonSet
  - k8s resource type의 일종으로서 `주로 모든 node에 Pod을 배포해야할 경우 사용된다`. 예를들어 node monitoring 등의 경우 모든 node에 logging agent가 배포되어야하는데 이러한 경우 DaemonSet을 이용하면 된다. 
- cadvisor vs node exporter
  - cadvisor는 running container의 정보를 제공한다. cadvisor가 수집한 정보는 kubelet으로 전달이되므로 prometheus config에서는 pull target을 적절히 설정해줘야한다.
    ```
    - job_name: 'kubernetes-cadvisor'
      scheme: https
      tls_config:
        ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token

      kubernetes_sd_configs:
      - role: node
      relabel_configs:

      # cAdvisor information can be fetched using kube-apiserver
      - source_labels: [__address__]
        target_label: __address__
        replacement: kubernetes.default.svc:443
      - source_labels: [__meta_kubernetes_node_name]
        regex: (.+)
        target_label: __metrics_path__
        replacement: /api/v1/nodes/${1}/proxy/metrics/cadvisor
    ```
  - node exporter는 node의 hardware information을 수집한다.
  - 즉 cadvisor가 수집하는 cpu 사용량은 container의 cpu 사용량이고 node exporter가 수집하는 cpu 사용량은 node의 cpu 사용량이다.

- `prometheus.io/scrape`, `prometheus.io/port` annotation의 의미
  - prometheus config에서 위의 annotation을 이용하여 해당 pod에서 metric을 pull 할지 말지를 결정할 수 있다.
    ```
    - job_name: 'kubernetes-pods'

      kubernetes_sd_configs:
      - role: pod

      relabel_configs:

      # Fetch metrics from only pods where scrape is enabled
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
    ```
- prometheus default retention day
  - https://prometheus.io/docs/prometheus/latest/storage/#operational-aspects
    - prometheus는 기본적으로 15d간의 데이터를 저장함

---

##### 2021.03.10 (24)
- k8s resource monitoring tools
  - kube dashboard / prometheus 등이 있는데, prometheus를 가장 많이 사용하는듯함
  - **[PromCat](https://promcat.io/)을 이용하면 상당히 다양한 종류의 metric을 수집할 수 있다. 그래서 prometheus를 사용하기로 했다.**
- k8s resource monitoring targets
  - https://www.samsungsds.com/kr/insights/kubernetes_monitoring.html
- k8s monitoring with prometheus
  - https://gruuuuu.github.io/cloud/monitoring-02/#
    - helm을 이용하지 않고 prometheus와 grafana 구성
  - https://sysdig.com/blog/kubernetes-monitoring-prometheus/
  - prometheus는 http/https를 이용하여 metric을 수집함. config에 target을 추가하면 target_ip:target_port/metrics 경로를 일정 interval마다 call하여 데이터를 수집함
  - [traefik](https://github.com/traefik/traefik) 처럼 prometheus-friendly한 service의 경우 /metrics 경로를 통해 이미 metrics을 제공함
  - nginx 등의 traditional한 service의 경우 자체 metric을 생성하는데, 이를 prometheus에서 쉽게 가져다 사용하기위해 **exporter**를 이용함
  > An exporter is a “translator” or “adapter” program that is able to collect the server native metrics (or generate its own data observing the server behavior) and re-publish them using the Prometheus metrics format and HTTP protocol transports.
- helm 도입예시
  - https://spoqa.github.io/2020/03/30/k8s-with-helm-chart.html

---

##### 2021.03.09 (23)
- serialize vs deserialize
  - js나 python에서는 잘 사용하지 않지만 cs 전반에서 종종 보이는 용어라 궁금해서 정리해봄
  - https://en.wikipedia.org/wiki/Serialization
  > In computing, serialization (US spelling) or serialisation (UK spelling) is the process of translating a data structure or object state into a format that can be stored (for example, in a file or memory data buffer) or transmitted (for example, across a computer network) and reconstructed later (possibly in a different computer environment)
  - 즉 프로세스 내부에서 사용하는 자료구조나 object 등을 저장될 수 있는 형태 ( 대표적으로 byte 등등 )로 변환하는것을 serialize라고 하고, serialize된 데이터를 나중에 deserialize를 통해 다시 자료구조나 object로 복원이 가능함
  - js의 경우 JSON.stringify() / JSON.parse(), python의 경우 json.dumps() / json.loads()가 각각 serialize, deserialize의 예시 함수라고 할 수 있다.

- json vs yaml
  - https://levelup.gitconnected.com/json-vs-yaml-6aa0243aefc6
  - 둘 다 매우 유명한 serialize 방법 중 하나인데, json이 더 정형화된 형식을 가져서 parsing 성능이 좋고 yaml이 더 human readable한 형식을 가짐. 특히 yaml은 `comment를 지원함`. 그래서 config file에는 json보다 yaml이 더 유리함

---

##### 2021.03.08 (22)
- 아무리 사소한 글이라도 일단 쓰고 보자.
- https://stackoverflow.com/questions/66110223/which-branch-should-workflow-files-be-located-on-for-github-actions-to-execute-t
  - github action은 push/PR된 branch의 workflow file을 이용함. 즉 branch마다 서로 다른 workflow file을 배치하면 각 push/PR된 branch의 workflow file만 동작함
- k8s rename context name
  - https://stackoverflow.com/questions/59606449/kubectl-how-to-rename-a-context

---

##### 2021.03.03 (21)
- git merge 전략 3가지
  - https://evan-moon.github.io/2019/08/30/commit-history-merge-strategy/
  - create: 분리된 branch의 commit history를 그대로 합치는 방법
  - squash: 분리된 branch의 여러 commit을 하나의 commit으로 만들어 합치는 방법, feature -> dev 간의 merge에서 유용하다. 다만 commit history를 잃을 수있다.
  - rebase: 분리된 branch의 commit이 마치 분리되기 전 branch에서 작업한 것처럼 합치는 방법. squash와 다르게 commit history를 유지할 수 있지만 conflict 폭탄을 맞을 수 있다.
  - 세가지 전략을 잘 골라서 쓰는것이 좋은데, create에 비해 squash/rebase가 commit history가 깔끔해진다는 장점이 있다.ㅇ

---

##### 2021.03.02 (20)
- k8s Deployment strategy
  -  https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/
    -  `Recreate`와 `RollingUpdate`가 있는데 default는 `RollingUpdate`이다
    -  `Recreate`의 경우 update를 위해 현재 실행중인 모든 pod을 shutdown한 후 새로운 pod을 띄운다
    -  `RollingUpdate`의 경우 현재 실행중인 pod중 일부를 shutdown하고 일부는 유지함으로써 **무중단배포**를 가능하게 한다. 즉 배포시점에는 구버젼과 신버젼의 pod이 동시에 실행중인 상태가 된다.
- k8s cluster 내부에서 nginx을 써야할까?
  - nginx를 사용하는 여러가지 이유가 있지만, 대표적인 이유로 다음의 용도가 있다.
    -  reverse proxy server로서의 역할, 즉 외부 요청을 nginx를 통해 web app에 전송
    -  static file serving의 역할
    -  caching / rate limiting 등 부가 기능을 이용하기위한 용도
  - 현재 회사의 상황은 static file (image, js, css)을 s3로 serving하고 web app은 api server의 역할만 있으며 reverse proxy의 기능은 k8s service가 해주고 있다. 이런 상황에서 굳이 nginx pod이 필요할까? => `특정 기능을 사용하기위해서라면 필요하지만 그렇지 않다면 없애는것도 가능할것같다. 또한 L7 load balancing을 이용하고싶다면 nginx pod 대신 nginx ingress controller를 이용할 수 있다.`
    - https://velog.io/@nawnoes/Nodejs-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%84%B1%EC%8B%9C-Nginx%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EA%B0%80
    -  https://mygumi.tistory.com/146
    -  https://uhou.tistory.com/96

---

##### 2021.02.26 (19)
- k8s aws alb
  - https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/
  - https://github.com/kubernetes-sigs/aws-load-balancer-controller
  - https://velog.io/@ausg/eks-k8s-elb#%EC%9D%B8%EC%A6%9D%EC%84%9C-%EC%84%A4%EC%A0%95%EC%9D%84-%ED%86%B5%ED%95%B4-https-%EA%B9%8C%EC%A7%80 
  - https://medium.com/coinone/aws-eks%EC%97%90%EC%84%9C-alb-ingress-controller-%ED%99%9C%EC%9A%A9%EA%B8%B0-6a29aa2a1144

---

##### 2021.02.19 (18)
- eks loadbalaner https
  - https://aws.amazon.com/ko/premiumsupport/knowledge-center/terminate-https-traffic-eks-acm/
- k8s node 분류
  - https://waspro.tistory.com/582
- aws load balancer
  - ALB(application load balancer), CLB(elb, classic load balancer), NLB(network load balancer)가 있는데 CLB에서는 websocket을 지원하지 않음
  - k8s ingress도 그렇고 여러 blog글들도 그렇고 요새는 L7 level load balaner, 즉 ALB를 많이 사용하는것 같음
  - https://haloworld.tistory.com/137

---

##### 2021.02.18 (17)
- k8s loadbalancing
  - https://blog.getambassador.io/load-balancing-strategies-in-kubernetes-l4-round-robin-l7-round-robin-ring-hash-and-more-6a5b81595d6c
    - 여러개의 pod을 묶기위해 하나의 service를 이용하는데 service에서 pod으로 가는 routing은 `kube-proxy`가 수행해주고 kube-proxy는 round-robin 방식을 이용한다.
  - https://itnext.io/kubernetes-service-load-balancing-kube-proxy-and-iptables-da3ebf1c802a
    - user proxy mode에서는 rounb-robin, iptables proxy mode에서는 randomly,
    - iptables proxy mode에서는 ReadinessProbe를 잘 설정해야한다.

---

##### 2021.02.15 (16)
- vscode enable pylint
  - https://github.com/JHyeok/TIL/blob/master/TIL/Python/python-black-pylint.md
  ```
  # settings.json
  
  # enable pylint
  "python.linting.pylintEnabled": true,
  "python.linting.enabled": true,
  "python.linting.lintOnSave": true
  
  # enable black for python files (instead of prettier)
  "python.formatting.provider": "black",
  "[python]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "ms-python.python"
  },
  ```

---

##### 2021.02.14 (15)
- aws 기본 설정
  - https://aws-diary.tistory.com/7?category=753069d
- eks 셋업
  - https://aws-diary.tistory.com/44?category=753089

---

##### 2021.02.13 (14)
- study on eks
  - https://www.youtube.com/watch?v=O3znWPUdt18
    - kubelet(runs on worker), kubeproxy(runs on worker), controller manager(runs on master), scheduler(runs on master) watches apiserver
    - apiserver watches etcd
    - kubelet, kubeproxy, controller manager, scheduler는 서로 통신을 하지 않고 container로 떠있기 때문에 잠깐 장애가 생겨도 금방 복구할 수 있지만 **apiserver, etcd**는 장애가 생기면 cluster level의 장애로 이어진다. 그러므로 apiserver, etcd는 HA(High Availabilty), 즉 여러개를 띄워놔야한다.
      - etcd는 홀수개로 유지되어야한다. (https://blog.naver.com/alice_k106/221310093541)
    - eks는 master node에 대한 관리를 해준다.
    - eks는 worker node를 위해
      - ec2 instance를 직접 관리하거나
      - eks node groups를 이용해서 자동관리 해준다.
    - k8s RBAC (Role Based Access Control)
      - role: namespace 단위 resource 권한
      - role binding: role을 user/group에 할당 (pv, pvc의 관계와 비슷한듯)
      - cluster role: cluster 전역의 resource 권한
    - eks에서 사용하는 AWS CNI는 resource에 VPC 내부의 real IP를 할당한다. 즉 해당 resource에 직접 접근하기가 용이하다. 또한 많은 resource(많은 수의 pod 등)를 이용한다면 충분히 넓은 ip range를 설정해야한다. 그런데 ec2 type에 따라 사용할 수 있는 network interface의 갯수가 다르므로 ip range와 ec2 type 모두를 잘 고려해야한다.

---

##### 2021.02.02 (13)
- k8s ingress controller
  - https://blog.naver.com/alice_k106/221502890249
  - dial tcp timeout뜨면 `kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission`

---

##### 2021.01.26 (12)
- python issubclass()
  - minIO python sdk를 보면 MinIO.put_object()의 argument 중 `data`에 `io.RawIOBase` type에 해당하는 변수를 넘겨야한다. 그런데 `io`에서는 io.BytesIO, io.StringIO 등등 다양한 IO type을 제공하는데 이때 이 다양한 IO type들과 RawIOBase와 어떤 관계인지를 확인해야 정확한 코딩을 할 수 있었다.
  - python3.7 기준으로 https://github.com/python/cpython/blob/3.7/Lib/io.py에서 io에 대한 코드를 살펴볼 수 있다.
  - builtin function [issubclass](https://docs.python.org/ko/3/library/functions.html#issubclass)를 통해 BuffredIOBase가 RawIOBase와 상속의 관계가 있는지 확인해볼 수 있다.
    ```python
    import io
    issubclass(io.BufferedIOBase, io.RawIOBase) # false
    issubclass(io.RawIOBase, io.BufferedIOBase) # false
    ```
  - minio python sdk issue 등록 (https://github.com/minio/minio-py/issues/1070)
  
---

##### 2021.01.21 (11)
- minIO
  - run minIO as daemon process using systemctl
    - https://github.com/minio/minio-service/tree/master/linux-systemd
      1. install minIO binary at /usr/local/bin: `cd /usr/local/bin; sudo wget https://dl.min.io/server/minio/release/linux-amd64/minio; sudo chmod +x minio`
      2. create config file: `/etc/default/minio`
      3. Install service file: `cd /etc/systemd/system/; curl -O https://raw.githubusercontent.com/minio/minio-service/master/linux-systemd/minio.service`
      4. Change `User` and `Group` config in service file
      5. `sudo systemctl restart minio`

---

##### 2021.01.20 (10)
- k8s
  - service account / role / rolebinding / rbac.authorization.k8s.io/v1
  - dynamic provisioning nfs
    - nfs와 `quay.io/external_storage/nfs-client-provisioner`를 이용해서 dynamic pv provisioning이 가능하다. on-premise(private cloud)에서 사용하기 좋은것같다.
    - https://blog.exxactcorp.com/deploying-dynamic-nfs-provisioning-in-kubernetes/
    - https://gruuuuu.github.io/cloud/k8s-volume/#

---

##### 2021.01.19 (9)
- nfs
  - nfs에서 이용하는 default port는 2049임 (TCP, UDP 둘 다) => **해당 port에 대한 접근을 허용해야함**
    - ```bash
      ubuntu@node1:/mnt$ rpcinfo -p | grep nfs
      100003    3   tcp   2049  nfs
      100003    4   tcp   2049  nfs
      100003    3   udp   2049  nfs
      ```
  - ubuntu18.04에 nfs server 구축하기
    - nfs server는 크게 kernel nfs server / user nfs server가 있는데 여기서는 kernel nfs server 구축을 설명함
      - https://www.nemonein.xyz/2020/09/4260/
      - https://sarc.io/index.php/os/1780-ubuntu-nfs-configuration
      - do below
        ```
        sudo apt-get install nfs-kernel-server
        sudo mkdir -p /mnt/nfsdir  # create nfs dir
        sudo chmod -R 777 /mnt/nfsdir
        ```
      - edit `/etc/exports`
        - add `/mnt/nfsdir *(rw,sync,no_subtree_check,insecure)`
        - **이유는 모르겠는데 0.0.0.0/0 같은 ip range로는 안됨** => https://stackoverflow.com/questions/65786683/0-0-0-0-0-is-not-feasible-for-nfs-configuration
      - `sudo exportfs -arv`
        - `/etc/exports` 설정 적용
      - `sudo systemctl restart nfs-kernel-server`
        - nfs server 시작
      - `sudo showmount -e`
        - nfs mount 확인
      - (client에서) `sudo mount <nfs server ip>:<nfs server mount path> <nfs client mount path>`로 nfs access 확인 가능
        - e.g, `sudo mount 10.1.3.58:/mnt/nfsdir /home/ubuntu/mount`
    - enable nfs logging
      - https://kerneltalks.com/config/nfs-logs-in-linux/
      - ```
        # enable logging
        $ rpcdebug -m nfsd all
        nfsd       sock fh export svc proc fileop auth repcache xdr lockd
        
        # disable logging
        $ rpcdebug -m  nfsd -c  all
        nfsd      <no flags set>
        ```
      - ubuntu18.04의 경우 `/var/log/syslog`에 logging됨
        - `sudo tail -f /var/log/syslog | grep nfs`
  - k8s에서 nfs를 이용하려고 할 때 node들에서는 nfs client 이용시 필요한 setup을 해줘야함, e.g, `sudo yum install nfs-utils` (centos) or `sudo apt-get install nfs-common` (ubuntu)
    - k8s에서 바로 이용하기전에 직접 node -> nfs server의 connection을 확인해보면 좋음
      - mount하기 `sudo mount <nfs server hostname or ip>:<nfs server mount path> <nfs client mount path>`
      - unmount하기 `sudo umount <fns client mount path>`

- ansible
  - k8s에서 nfs를 이용하려면 nfs server 뿐만아니라 nfs client도 구축을 해야한다. 각 node가 nfs client가 될 수 있으므로 모든 node(나의 경우는 총 5개)에 `sudo apt-get install nfs-common`을 해야한다.
    - inventory
      ```
      [master]
      master1 ansible_host=10.1.3.245 ip=10.1.3.245 ansible_python_interpreter=/usr/bin/python3

      [node]
      node1 ansible_host=10.1.3.58 ip=10.1.3.58 ansible_python_interpreter=/usr/bin/python3
      node2 ansible_host=10.1.3.191 ip=10.1.3.191 ansible_python_interpreter=/usr/bin/python3
      node3 ansible_host=10.1.3.88 ip=10.1.3.88 ansible_python_interpreter=/usr/bin/python3
      node4 ansible_host=10.1.3.74 ip=10.1.3.74 ansible_python_interpreter=/usr/bin/python3
      node5 ansible_host=10.1.3.228 ip=10.1.3.228 ansible_python_interpreter=/usr/bin/python3
      ```
    - command `ansible -i inventory.cfg node -m command -a 'apt-get install -y nfs-common' --become`
      - `--become`은 sudo로 실행하는것을 의미
      - `node`는 group을 의미하며 `node` group에는 node1~node5가 있음 (inventory에서 확인 가능)
      - https://www.whatwant.com/entry/Ansible-%EC%B2%98%EC%9D%8C%EC%9C%BC%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0?category=744443

---

##### 2021.01.15 (8)
- openVPN
  - https://github.com/Nyr/openvpn-install
  - https://ncube.net/%EC%9A%B0%EB%B6%84%ED%88%AC-18-04-%EC%97%90-openvpn-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95/
  - road warrior installer로 간편하게 openVPN 서버 구축 
- nodejs
  - aws EC2 ubuntu 18.04에 설치된 node/npm은 8.10/3.52였는데 이를 업그레이드 쉽게 업그레이드 할 수 있음
    - https://velopert.com/1351
- k8s에 react app 띄우기
  - nginx를 이용하여 react build file을 serving하면됨
    - https://koala42.com/create-a-react-app-in-kubernetes/
    - https://dev.to/rieckpil/deploy-a-react-application-to-kubernetes-in-5-easy-steps-516j
- private docker registry
  - docker registry는 external 에서 접근할 때 TLS를 강제하므로 인증서 작업을 해줘야함
    - http://www.zakariaamine.com/2018-07-22/creating-docker-registry-ip-address
    - https://novemberde.github.io/2017/04/09/Docker_Registry_0.html
    - self signed certificate 생성
      - https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04
    - https://figo2264.github.io/2017/11/28/docker-registry-with-self-signed-certificate/

---

##### 2021.01.14 (7)
- minIO
  - ubuntu에서 minIO 서버 띄우기
    - https://www.lesstif.com/system-admin/minio-server-open-source-object-storage-server-74743911.html
    - minIO는 단일 binary 파일로 바로 띄울 수 있어서 설치가 매우 편리함
    - ip/port binding을 하기위해 다음의 command를 이용하면됨
      ```bash
      ./minio server --address 0.0.0.0:9000 <data directory>
      ```
    - ip를 `0.0.0.0`으로 binding하는것보다 nginx 등의 reverse proxy를 이용하는게 더 일반적인 방법으로 보임
      - https://docs.min.io/docs/setup-nginx-proxy-with-minio.html

---

##### 2021.01.13 (6)
- minIO
  - [minIO operator](https://github.com/minio/operator)를 이용하여 k8s cluster위에 minIO (s3 compatible object storage) 구축
    - StorageClass, PersistentVolume이 문제여서 처음에는 컨테이너를 띄우지도 못했는데 오늘 띄우는데까지 성공함
    - 그러나 document가 outdated되어있고 k8s cluster가 aws private subnet안에 있어서 실제 webUI를 테스트해보지는 못하였음
    - NodePort 등 k8s service를 이용하여 외부에 노출하는 방법 / k8s를 이용하지 않고 그냥 EC2위에 minIO를 띄우는 방법 등 2가지 방법이 있는데 시간관계상 EC2위에 minIO를 띄우는 것으로 대체하기로 함
- ansible
  - minIO를 k8s에 띄울때 여러개의 node에 PV에 mount할 directory를 만들어야 했는데 이를위해 간단하게 ansible 이용
    > Ansible is a radically simple IT automation system. It handles configuration management, application deployment, cloud provisioning, ad-hoc task execution,  network automation, and multi-node orchestration. Ansible makes complex changes like zero-downtime rolling updates with load balancers easy.
  - 아주 간략히 설명하자면 ansible은 여러 remote machine을 ssh를 이용하여 쉽게 관리하는 tool이라고 할 수 있음
    - 예를들어, 나의 경우에는 5개의 k8s node에 동일한 directory를 생성해야했는데 이를 ansible을 이용하여 수월하게 진행하였음
      ```bash
      ansible -i inventory.ini all -a "mkdir -p ~/minIO"
      ```
  - components
    - inventory
      - ansible에서 관리하는 remote machine을 명시해놓은 파일
      - 대표적으로 `.ini`, `.yaml`의 확장자를 이용할 수 있음
        ```
        # inventory.ini
        [all]
        node1 ansible_host=10.1.3.58 ansible_python_interpreter=/usr/bin/python3
        node2 ansible_host=10.1.3.191 ansible_python_interpreter=/usr/bin/python3
        node3 ansible_host=10.1.3.88 ansible_python_interpreter=/usr/bin/python3
        node4 ansible_host=10.1.3.74 ansible_python_interpreter=/usr/bin/python3
        node5 ansible_host=10.1.3.228 ansible_python_interpreter=/usr/bin/python3
        ```

---

##### 2021.01.09 (5)
- kubespray
  ```
  TASK [kubernetes/preinstall : Stop if even number of etcd hosts] *****************************************************************
  fatal: [master1]: FAILED! => {
      "assertion": "groups.etcd|length is not divisibleby 2",
      "changed": false,
      "evaluated_to": false,
      "msg": "Assertion failed"
  }
  ```
  - 위 error는 inventory에 etcd가 짝수개 정의되어있을 때 발생한다. etcd는 k8s의 state를 저장하는 곳으로 split brain 문제로 인해 홀수개가 정의되어야 한다. 아주 간단하게 설명하면 장애가 생겼을 때 홀etcd가 홀수개이면 더 많은 노드가 가르키는 값을 이용할 수 있지만 짝수개일 경우 정확히 반반으로 나뉘어 어떤것이 맞는것인지 확신할 수 없게 된다.
    - [split brain](https://blog.naver.com/alice_k106/221310093541)
    
---

##### 2021.01.08 (4)
- k8s service
  - https://bcho.tistory.com/1262
    - pod은 시작될 때 마다 새로운 random IP를 받음
    - pod이 여러개일 경우 loadbalancing 기능도 있어야함
    - 이를 보완하기위해 service 라는 resource가 생겨남
      - service는 고정 IP를 받고 loadbalancing 기능을 수행할 수 있고 고유 DNS도 갖을 수 있음
      - service는 selector를 이용해서 자신이 관리하고자하는 pod들을 정할 수 있음
    - 하나의 service에 여러개의 port를 open할 수도 있음, e.g, HTTP(80), HTTPS(443)
    - 여러개의 pod을 운영할 때 session 등을 위해 특정 client가 특정 pod에 지속적으로 연결하게 하려면 `sessionAffinity`를 이용할 수 있음
- helm
  - https://velog.io/@makeitcloud/Kubernetes-Helm-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
    - helm은 chart repository에서 chart를 가져올 수 있음
    - chart 내부에는 k8s manifest file이 있고 helm은 내부적으로 kubectl을 이용해서 k8s resource를 배포함
    - chart의 instance를 `release`라고 부름
  - https://thorsten-hans.com/getting-started-with-helm3
    - helm 3 (version 3.x)을 이용한 튜토리얼
    - helm은 k8s를 위한 package manager
      - node의 npm, python의 pip와 비슷한 역할
    - k8s로 resource를 구성할 때 `.yaml` 형식의 manifest file을 이용하듯이 helm은 `set of manifest files`를 관리한다고 생각하면 됨
    - `set of manifest files`를 chart 형식이라고 부르고 chart가 helm에서 관리하는 단위가 됨
    - chart는 `.tgz` 형식으로 배포되며 해당 압축파일을 풀면 하나의 diretory가 나오는데 root directory 내부에는 다음과 같은 file 및 directory들이 존재함
      - Chart.yml
        - chart의 metadata
        - `name`, `version`, `description`, `kubeVersion` 등의 정보가 들어감
      - templates (dir)
        - set of k8s manifest files
        - 각 manifest file에는 변수로 값을 받을 수 있게 되어있음
      - values.yaml
        - `templates`에 존재하는 manifest file의 동적 변수값을 설정할 수 있음
  - mysql chart 배포
    - 내가 직접 k8s resource를 구성하는 것 보다 이미 구성되어진 유명한 셋팅을 사용하면 더 좋을것이라고 생각하여 helm 연습 겸 mysql chart (https://github.com/helm/charts/tree/master/stable/mysql)를 배포하려 시도하였음
    - 결과적으로 실패하였는데 이와 관련하여 server fault에 질문을 올려놓은 상태 https://serverfault.com/questions/1049040/deploy-mysql-release-using-helm/1049068#1049068

---

##### 2021.01.05 (3)
- aws VPC / subnet / IG(InternetGateway) / NAT(Natwork Addess Translation)
  - VPC는 `격리된` 네트워크 공간
  - VPC와 외부를 연결하기 위해서는 반드시 IG가 필요함
  - subnet은 VPC를 다시 나눈 sub network 영역
    - 흔히 public subnet과 private subnet으로 나누는데 aws에서는 단순히 subnet의 생성만을 지원할 뿐이고 public subnet처럼 이용하려면 subnet의 RT(route table)에 IG를 달아줘야함
    - **`public subnet 내부에 존재하는 component라도 public IP가 없으면 internet access가 불가능함`**
      - 심지어 public subnet 내부에 public IP가 없는 EC2 instance의 할당도 가능함 => internet access는 불가능
    - private subnet에서는 IG가 없으므로 당연히 internet access가 안되지만 NAT를 추가하면 internet access 가능
      - NAT는 자체적으로 public IP를 갖고 NAT를 생성할 때 subnet을 지정하는데 이때 public subnet을 지정해줘야함
      - 즉 NAT는 자체 public IP를 갖고 이 IP와 IG를 이용해서 private subnet 내부의 component들에게 internet access를 가능하게 해줌
- https://serverfault.com/questions/843243/ssh-jumping-with-aliases-and-f
  - proxyhost에 관한 ssh config
  - `~/.ssh/config` 대신 custom config file을 이용하여
    ```bash
    ssh -F <config file>
    ```
    과 같은 형식의 명령어를 사용할 때, `ProxyCommand` directive에서는 `<config file>`을 인식하지 못하고 default config인 `~/.ssh/config`를 이용함. 그 결과
    ```bash
    ProxyCommand ssh -F <config file> bastion
    ```
    과 같이 `<config file>` 내부에 자신의 경로를 한번 더 써줘야하는 번거로움 및 지저분함이 있음.  
    - `ProxyCommand` 대신 `ProxyJump` directive를 이용하면 깔끔함 (>= OpenSSH 7.3)
- ssh-copy-id 명령어
  - ssh로 linux machine에 접속하는 방법은 크게 2가지가 있음
    - password 인증
    - key file 인증
  - password 인증을 위해서는 `/etc/ssh/sshd_config`에서 `PasswordAuthentication no` 설정을 추가해야함
  - password 인증을 enable 한 후 password없이 접근하고 싶을 때 `ssh-copy-id` 명령어를 이용하면됨
    - `ssh-copy-id`는 `~/.ssh/id_rsa.pub`파일을 ssh target host의 `~/.ssh/authorized_keys` 파일에 추가해줌
    - `ssh-copy-id` 명령어를 이용하지 않고 직접 복사해서 추가해줘도 됨
    - `~/.ssh/authorized_keys`에 추가해준 이후에는 별도의 인증없이 ssh 접속 가능!
    
- kubespray
  - 1개의 bastion과 n개의 cluster node들이 있을 때도 kubespray를 이용해서 k8s cluster 구축이 가능함. 단 아래의 예시에서는 다음의 조건을 가정
    - bastion => node로 접속할 때 모두 동일한 key file을 이용한다고 가정
    - 각 node들은 internet access가 가능함
      - internet access가 불가능한 경우는 직접 docker registry, file server 등을 구축해야하는데 이 부분까지는 수행하지 못했음
  1. clone kubespray repo
  2. ssh config file 생성
    ```
    # ssh.cfg

    # node 접속정보
    Host <node ip 대역, e.g, 10.*.*.*>
    User <ssh login user, e.g, ubuntu>
    IdentityFile <bastion=>node 접속시 사용하는 key file>
    ProxyJump bastion1

    # bastion 접속정보
    Host bastion1
      Hostname <bastion host ip or bastion domain name>
      User <ssh login user>
      IdentityFile <local=>bastion 접속시 사용하는 key file>
    ```
  3. modify `ansible.cfg` file
    ```
    # ansible.cfg

    ...
    [ssh_connection]
    ...
    ssh_args = -C -F ./ssh.cfg
    ```
  
  4. create `inventory/inventory.cfg` file
    ```
    [all]
    # node-c1
    nodec1 ansible_host=10.1.3.111 ip=10.1.3.111 ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3
    # node-c2
    nodec2 ansible_host=10.1.3.224 ip=10.1.3.224 ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3
    # node-c3
    nodec3 ansible_host=10.1.3.71 ip=10.1.3.71 ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3
    # node-c4
    nodec4 ansible_host=10.1.3.170 ip=10.1.3.170 ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3

    # bastion 접속정보
    [bastion]
    bastion1 ansible_host=13.125.65.133 ansible_user=ubuntu

    [kube-master]
    nodec1

    [etcd]
    nodec1

    [kube-node]
    nodec2
    nodec3
    nodec4

    [k8s-cluster:children]
    kube-node
    kube-master

    ```
  5. run command
  ```
  ansible-playbook -i inventory/inventory.cfg -b -v cluster.yml
  ```

---

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
