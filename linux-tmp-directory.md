---
title: "Linux tmp directory"
date: 2020-01-18T10:42:42+09:00
---

<!--more-->

## Note
This blog is written in both Korean and English. Korean is my native language and English is the language I want to learn.

***

### Motivation
Recently I was to deploy simple app from my local machine. According to document, I put config file in `/tmp` directory and deployed with this config file. After few days, I tried to deploy again but config file had gone away. I was really depressed and found that `/tmp` is special directory in linux.

### Content
According to document written by LinuxFoundation (https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf), as it's name states, `/tmp` directory is for keeping temporary files. That is, **files in** `/tmp` **directory can be removed regardless of your intention.** The condition where `/tmp` directory is cleaned is dependent on operatring system. Normally, it's cleaned up whenever OS is rebooted. In case of my local mac book, `/tmp` directory is cleaned up every 3 days. Config file which states period of clean up may differ from OS.

```
[18/01/20 11:18:25] ❯ cat /etc/defaults/periodic.conf | grep tmp
# 110.clean-tmps
daily_clean_tmps_enable="YES"				# Delete stuff daily
daily_clean_tmps_dirs="/tmp"				# Delete under here
daily_clean_tmps_days="3"				# If not accessed for
daily_clean_tmps_ignore=".X*-lock .X11-unix .ICE-unix .font-unix .XIM-unix"
daily_clean_tmps_ignore="$daily_clean_tmps_ignore quota.user quota.group"
daily_clean_tmps_verbose="YES"				# Mention files deleted
```

### Summary
`/tmp` directory is special directory is linux based OS to keep temporary files. (My loacl is Mac OS but most things are similar to linux) When you use this directory, you need to know that files in this directory can be removed unexpectedly.

***

### 배경
최근에 로컬 머신에서 config 파일을 이용하여 remote에 간단한 app을 배포할 일이 있었다. 문서를 보고 설정 파일을 `/tmp` 디렉토리에 위치시킨 후 배포를 하였더니 잘 실행이 되었다. 며칠 후에 다시 배포를 하려고 `/tmp` 디렉토리를 확인해 보았더니 설정 파일이 없어졌다. 실수인 줄 알고 다시 설정파일을 저장하였으나 며칠 후에 다시 확인했을 때, 설정 파일이 또 사라졌다. 알고봤더니 `/tmp`는 임시로 파일을 저장하는 linux의 특별 용도 디렉토리였다.

### 본문
Linux Foundation에서 작성한 파일시스템에 관한 다음 글을 보면 (https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf) `/tmp` 디렉토리에 대한 설명이 있다.
```
The /tmp directory must be made available for programs that require temporary files.
...
Although data stored in /tmp may be deleted in a site-specific manner, it is recommended that
files and directories located in /tmp be deleted whenever the system is booted.
```
`/tmp` 디렉토리는 temporary file을 위한 디렉토리이며 system이 재시작 할 때 마다 `/tmp` 디렉토리 내부의 파일을 지우는 것이 추천된다고 한다. 실제로 `/tmp` 디렉토리가 초기화 되는 주기는 운영체제마다 다르지만, 내 mac book의 경우에는 3일에 한번씩 초기화 되는것으로 설정되어있었다.

```
[18/01/20 11:18:25] ❯ cat /etc/defaults/periodic.conf | grep tmp
# 110.clean-tmps
daily_clean_tmps_enable="YES"				# Delete stuff daily
daily_clean_tmps_dirs="/tmp"				# Delete under here
daily_clean_tmps_days="3"				# If not accessed for
daily_clean_tmps_ignore=".X*-lock .X11-unix .ICE-unix .font-unix .XIM-unix"
daily_clean_tmps_ignore="$daily_clean_tmps_ignore quota.user quota.group"
daily_clean_tmps_verbose="YES"				# Mention files deleted
```

### 결론
`/tmp` 디렉토리는 임시 파일을 저장하기 위한 linux의 특별 디렉토리이다. 그래서 이 디렉토리를 사용할 때에는 본인이 원치 않게 파일이 지워질 수 있음을 인지하고 사용하는것이 좋다.

#### Feel free to make any questions or advices to me
