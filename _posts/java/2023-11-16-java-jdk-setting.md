---
title: "[Java] JDK버전 세팅"
category: Java
tags: java jdk java-version
---

우아한 테크코스를 진행하면서 알게된 JDK 버전 세팅 방법

-----

# JDK 버전 설정

나는 기존에 학교강의를 들으며 설치한 자바 19를 사용하고 있었다.

이번에 프리코스를 진행하면서 자바 17로 버전을 바꿔야 했고 그 과정을 기록했다.

----

```bash
# 자바 버전 확인 
java -version

# 로컬에 존재하는 자바 버전 확인
/usr/libexec/java_home -V

```

명령어 해석

> **/usr**: **시스템과 관련**된 여러 하위 디렉토리와 파일이 포함, 유닉스 및 리눅스 시스템에서 사용되는 최상위 디렉토리 중 하나
>
> **/usr/libexec:** 유닉스 및 리눅스 시스템에서 시스템 실행 파일 및 서비스를 관리하는 실행 가능한 파일을 저장하는 데 사용되는 위치 중 하나, 시스템 관리와 관련된 작업을 수행하는 실행 파일 **=
실행파일관리하는 디렉토리**
>
> **/java_home**: **디렉토리 X**, 현재 설치된 Java 버전 및 설치 위치에 대한 정보를 제공하는 **명령어**
>
> **.bash_profile:** macOS 및 리눅스 운영 체제에서 사용자 환경 및 Bash 셸의 구성을 설정하기 위한 중요한 환경 설정 파일 중 하나

- **환경 변수 설정**: **`export`** 명령을 사용하여 환경 변수를 설정하고, 사용자의 환경을 사용자 정의할 수 있다.
  예를 들어, **`PATH`** 환경 변수를 확장하여 사용자 지정 명령어 경로를 추가할 수 있다.

**환경변수**: 컴퓨터 시스템에서 사용되는 중요한 설정 및 정보를 저장하는 변수, 텍스트 형식으로 `키(key)`와 `값(value)`의 쌍으로 구성

- 시스템 환경변수: 시스템 자체에서 운영체제 수준에서 정의된 환경변수
- 사용자 환경변수: 특정 사용자에 대한 설정 및 환경 정보를 저장, 그 사용자의 홈디렉토리에 적용됨
- **환경 변수의 예**
    - **`PATH`**: 실행 가능한 파일(프로그램)을 찾는 경로 정보를 저장합니다.
    - **`HOME`**: 현재 사용자의 홈 디렉토리 경로를 저장합니다.
    - **`USER`**: 현재 사용자의 사용자 이름을 저장합니다.
    - **`JAVA_HOME`**: Java 개발 환경이 설치된 디렉토리를 저장(지정)

따라서, 자바의 버전 및 위치를 확인시켜주는 /usr/libexec/java_home -V 명령어를 수행해서
알아낸 `/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home`이 경로를 환경변수 JAVA_HOME(key)에 저장해주면

즉, `export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home` 를 .bash_profile에 저장해주면 환경변수JAVA_HOME의
경로가 자바 17이 설치된 경로로 설정된다.

근데 여기서 끝이 아님

/bin: 실행 가능한 프로그램 파일들을 저장하는 디렉토리 → 여기서 실행파일을 찾아 실행하는데

환경변수 PATH를 그 실행파일을 찾을 디렉토리로 지정해줘야한다.

어떤 프로그램을 실행하면 환경변수 PATH를 통해 실행 프로그램을 찾는데 그 PATH의 앞에 $JAVA_HOME/bin을 추가해 줌으로써 프로그램 실행시에 가장먼저 $JAVA_HOME/bin 디렉토리에서 호출된
프로그램의 실행프로그램이 있는지 찾게된다.

예를 들어 java 명령어를 실행하면 $JAVA_HOME/bin에 java17 실행파일이 존재하기 때문에 하위 디렉토리로 더이상 가지않고 java17을 실행한다

하지만 Python 명령어를 실행하면 $JAVA_HOME/bin에 Python 실행파일이 존재하지 않기 때문에 하위 디렉토리경로(=기본PATH경로)에서 Python 실행파일을 탐색하게 된다.

그래서 위에서 정한 JAVA_HOME을 이 PATH에 지정해주면

`PATH=$JAVA_HOME/bin:$PATH`이거다

```bash
# ~/.bash_profile 파일 편집
vi ~/.bash_profile

# 내용 추가
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
```

```bash
#변경된 환경변수 적용
source ~/.bash_profile
```

**MAC catalina 버전 이후로는 `~/.bash_profile` 대신 `~/.zshrc`를 변경해야한다.**

-----

## reference
[chatGPT](https://chat.openai.com/)