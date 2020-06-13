## Note
This blog is written in both Korean and English. Korean is my native language and English is the language I want to learn.
`Feel free to make any questions or advices to me`

***

### Motivation
After Windwos 10 May 2020 update, we can install WSL2. WLS1 is already installed in my desktop so I tried to upgrade it to
WSL2 because WSL2 provides much more feature than WSL1. To do this, I did
1. Delete Ubuntu 18.04 which is installed as WSL1
1. Install Ubuntu 18.04 again

But this shows error like below.
```
WslRegisterDistribution failed with error: 0x80370102
```

### Content
In my case, the reason was due to CPU virtualization.
I'm using **Ryzen3600 (AMD CPU)** with **B450M mainboard**.
I did
1. Go to BIOS menu
1. Go to `Advanced Frequency Settings`
1. Go to `Advanced CPU Core Settings`
1. Enable `SVM Mode`
1. Click `Save & Exit`
1. Click `Save & Exit Setup`

After doing above, Ubuntu 18.04 is successfully installed.

### Summary
It seems that there are several reasons for `0x80370102` error. For your information, I used these references.
- https://docs.microsoft.com/en-us/windows/wsl/install-win10
- https://windowsreport.com/error-0x80370102/
- https://www.youtube.com/watch?v=46DfLwX5AFQ

***

### 배경
2020년 5월에 WSL2가 정식으로 배포되었다. WSL1에 비하여 다양한 기능들 (예를들어 docker의 사용가능 / AMD GPU 사용가능 등)을 제공하기에
기존의 WSL1을 WSL2로 업그레이드 하기로 하였다. [공식홈페이지](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)를 참고하여
업그레이드 하던 중, Ubuntu 18.04를 설치하는 과정에서 다음과 같은 에러가 발생했다.
```
WslRegisterDistribution failed with error: 0x80370102
```

### 본문
나의 경우에는 **Ryzen3600 CPU**와 **B450M 메인보드**를 사용중이다. 구글링을하여 나온 여러가지 방법들을 시도해 보았는데, 원인은 CPU virtualization
때문이었다. 다음의 방법으로 CPU virtualization을 enable 해주니 정상적으로 Ubuntu 18.04를 설치할 수 있었다.

1. BIOS 메뉴로 진입
1. `Advanced Frequency Settings` 클릭
1. `Advanced CPU Core Settings` 클릭
1. `SVM Mode` enable 시키기
1. `Save & Exit` 클릭
1. `Save & Exit Setup` 클릭

위의 과정이 끝나고 Windows가 재시작 된 후 다시 Ubuntu 18.04를 설치하면 정상적으로 설치되는 것을 확인할 수 있었다.

### 결론
`0x80370102` 에러에는 여러가지 원인이 있는것 같아 보인다. 그 중에서 나는 CPU virtualization이 이유였고, 그에 따라 BIOS에 진입하여
SVM Mode (AMD CPU virtualization 인듯 하다)를 enable 해주니 에러가 해결되었다.
사용한 reference는 다음과 같다.
- https://docs.microsoft.com/en-us/windows/wsl/install-win10
- https://windowsreport.com/error-0x80370102/
- https://www.youtube.com/watch?v=46DfLwX5AFQ
