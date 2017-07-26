__TOC__

# Jenkins를 이용한 빌드 자동화 및 배포




## 1.Root 계정

### 1-1. [Root 계정으로 일반 계정 생성](#createuser)

```bash
$> adduser 아이디
```

### 1-2. 일반 계정이 접근할 수 있는 디렉토리 생성 및 권한 주기

```bash
$root> mkdir /apps
$root> chown 아이디.아이디 /apps
$root> ls -la
```

### 1-3. JDK & MAVEN 설치

[jdk-8u141-linux-x64.tar.gz](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
윈도우 & 맥의 경우 `ftp`를 이용하여 업로드 한다. windows 사용자는 [FileZillar](https://filezilla-project.org/)를 사용한다.

> `~/`는 홈 디렉토리를 말한다. `/home/아이디`와 같은 디렉토리이다.
> 여기서 우리는 JDK, MAVEN, TOMCAT을 `/apps`라는 폴더를 만들어서 관리 한다. 

#### JDK 설치

위의 JDK 링크에서 JDK 압축파일을 받은 후, 서버에 업로드해서 `/apps` 디렉토리로 옮긴다.

```bash
$아이디> cp [JDK 파일있는 경로] [복사할 경로]
$아이디> cp /home/oz/jdk-8u141-linux-x64.tar.gz /apps
```

위와 같이 해서 `/apps`폴더에 JDK파일이 옮겨졌다면, 다음과 같이 압축을 풀고, 심볼릭 링크를 걸어주자.

```bash
$아이디> cd /apps
$아이디> cp ~/jdk-8u141-linux-x64.tar.gz .
$아이디> tar xvfz jdk-8u141-linux-x64.tar.gz
$아이디> ln -s jdk1.8.0_141/ jdk
$아이디> ls -la
```

#### MAVEN 설치

MAVEN도 마찬가지로 `/apps` 폴더에 설치하며, 심볼릭 링크도 걸어주기

```bash
$아이디> cd /apps
$아이디> wget http://mirror.navercorp.com/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.tar.gz
$아이디> tar xvfz apache-maven-3.5.0-bin.tar.gz
$아이디> ln -s apache-maven-3.5.0 maven
```


#### JDK & MAVEN 환경 변수 설정

`java` 혹은 `mvn`명령어를 등록하기 위한 환경변수를 등록한다.

```bash
$아이디>
```
