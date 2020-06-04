## Note
This blog is written in both Korean and English. Korean is my native language and English is the language I want to learn.
`Feel free to make any questions or advices to me`

***

### Motivation
I created compute instance in OCI and wanted to make a simple HTTP server.
(For example, with `python3 -m http.server` command)
Even though I allowed all protocol/port in security rules, I could not access
to this compute instance.

### Content
In oracle linux 7, firewall is enabled by default. So if you want to access
certain port from outside, in our case with HTTP protocol, you have to allow
ports with firewall. One simple way is that you can just disable firewall.
(In security point, this can be dangerous but just for simplicity it works)

```
$ sudo systemctl stop firewalld         # stop firewalld
$ sudo systemctl disable firewalld      # disabled firewalld after reboot
```

Also you have to disable SELinux too.
In `/etc/sysconfig/selinux`, change line `SELINUX=enforcing/permissive` to
`SELINUX=disabled`.

`This is just for simplicity. I used this information for testing simple HTTP
server, not for production. In production, I guess that proper change in config
is needed rather than just disabling it.`

### Summary
If you want to access oracle linux 7 from outside, you can disable firewall
and selinux which is enabled by default. In Ubuntu, `ufw` does same role.


***

### 배경
간단한 http 서버 테스트를 위해 OCI(Oracle Cloud Infrastructure)에서 compute
instance를 만들었으나 http request가 제대로 전송되지 않았다. 보안규칙(Security Rule)
설정도 확인하였고 public IP가 할당된 것도 확인하였으나 무엇이 문제인지 한참을 헤맸다.
구글링을 해보니 oracle linux 7에서 기본적으로 사용하는 방화벽(firewall)이 문제였다.

### 본문
Oracle Linux 7에서는 firewalld라는 이름의 방화벽을 기본적으로 사용한다. 또한 SELinux
라는 보안이 강화된 kernel도 사용한다고 한다. 이 두가지로 인해 cloud level에서 보안
규칙을 제대로 정의했더라고 실제 compute instance에 요청이 가지 않았다.

firewalld를 해제하기 위해서는 다음과 같은 command를 이용하면 된다.
```
$ sudo systemctl stop firewalld         # 현재 실행중인 firewalld 정지
$ sudo systemctl disable firewalld      # 재부팅 후에도 firewalld를 사용하지 않도록 설정
```

SELinux를 해제하기 위해서는 `/etc/sysconfig/selinux` 파일에서 `SELINUX=enforcing`
또는 `SELINUX=enforcing/permissive`로 된 부분을 `SELINUX=enforcing/disabled`로
바꾸면 된다.

`위의 해결 방법은 간단한 테스트를 위한 임시적인 방법으로, 실제 production 등에서 사용
하기 위해서는 단순히 disable 시키는 것이 아닌 적절한 설정 변경을 하는것이 좋아보인다.`

### 결론
Oracle LInux 7에는 기본적으로 firewalld라는 방화벽과 SELinux라는 보안 커널이 적용되므로
http 등을 통해 외부에서 접근하기 위해서는 적절한 조취가 필요하다. Ubuntu에서는 `ufw`
라는 프로그램이 방화벽의 역할을 한다.
