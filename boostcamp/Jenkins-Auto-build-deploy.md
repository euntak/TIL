# Jenkins를 이용한 빌드 자동화 및 배포




## 1.Root 계정으로 해야 할 일들

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

## 2. 일반 계정으로 해야 할 일들

### 2-1. JDK & MAVEN 설치

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

`java` 혹은 `mvn`명령어를 등록하기 위한 환경변수를 등록한다. `vi`, `vim`, `nano` 편한걸 쓰자.

```bash
$아이디> vi /etc/profile
```

제일 하단에 이렇게 쓰자 `/apps` 폴더가 아닌 다른곳에 jdk가 깔려있다면 해당 주소를 넣어준다. maven도 마찬가지

```bash
...
...
JAVA_HOME=/apps/jdk
CLASSPATH=.:$JAVA_HOME/lib/tools.jar

PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

export PATH CLASSPATH JAVA_HOME

export M2_HOME=/apps/maven
export PATH=$PATH:$M2_HOME/bin
```

입력하고, 저장했으면 수정 된 내용을 저장하기 위해 아래와 같은 명령을 수행 또는 재접속을 한다.

```bash
$> source /etc/profile
```

#### 확인

```bash
$> java -version

$> echo $JAVA_HOME
$> echo $PATH
$> echo $CLASSPATH

$> mvn -version
```

### 2-2. Jenkins 설치 

`&`는 백그라운드에서 실행하라는 명령. 계정이 로그아웃되어도 백그라운드에서 실행하면 계속해서 동작한다.

```bash
$아이디> cd /apps
$아이디> wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
$아이디> java -jar jenkins.war &
```

#### 접속 확인

http://ip:8080와 같은 주소로 접속해서 보면 `Unlock Jenkins` 문구가 나오는데 이때 ==빨간색==으로 어떤 디렉토리를 가르킨다.
===빨간색===으로 나오는 부분을 복사해서 `bash`에서 붙여넣기 하면 key가 나오는데, 이 키를 통해서 Jenkins를 활성화 한다.

```bash
$아이디> cat /home/아이디/.jenkins/secrets/initialAdminPassword
```

#### 설치 옵션

Customize Jenkins 화면에서 `Install Suggested plugins` 클릭~ 유용한 플러그인들이 설치 된다.


#### Admin User 설정

빈칸 열심히 채우면 된다.

#### Jenkins 백그라운드 실행 종료 방법

`ps` 명령을 통해서 해당 Jenkins의 프로세스 `id`를 볼 수 있다.
`|`은 파이프다. `grep`은 입력 받은 내용중 특정 문자열이 포함된 것을 찾는 역할을 한다.

```bash
$> ps -lef | grep jenkins
```

```bash
[carami@boostcamp-001 ~]$ ps -lef | grep jenkins
0 S carami    1427   871  6  80   0 - 903049 futex_ 16:23 pts/4   00:00:59 java -jar jenkins.war
0 S carami    2551  2530  0  80   0 - 25814 pipe_w 16:39 pts/6    00:00:00 grep jenkins
```

위에서 `1427`, `2551`이 프로세스 ID이며, `kill -9 ID`로 프로세스를 종료 할 수 있다.

### 2-3. Mysql 사용자 계정 생성 및 DB 생성

```bash
$> mysql -uroot mysql
```

```sql
mysql> create user 'carami'@localhost identified by '암호';
mysql> create user 'carami'@'%' identified by '암호';

mysql> create database tododb;

mysql> GRANT ALL on tododb.* TO 'carami'@'localhost';
mysql> GRANT ALL on tododb.* TO 'carami'@'%';

mysql> flush privileges;
```


### 2-4. Tomcat 설치

```bash
cd /apps
wget http://apache.mirror.cdnetworks.com/tomcat/tomcat-8/v8.5.16/bin/apache-tomcat-8.5.16.tar.gz
tar xvfz apache-tomcat-8.5.16.tar.gz
ln -s apache-tomcat-8.5.16 tomcat
```

#### Tomcat server.xml 포트 포워딩 사전 준비 작업

```bash
vi /apps/tomcat/conf/server.xml
```

```xml
<!-- <Connector port="8080" protocol="HTTP/1.1"
     connectionTimeout="20000"
     redirectPort="8443" /> -->

<!-- 위의 8080포트를 9090포트로 변경, URIEncoding UTF-8 지정 -->

<Connector port="9090" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" URIEncoding="UTF-8"/>
```

#### Tomcat 기본 웹 어플리케이션 정보 삭제

Jenkins가 빌드 및 배포를 하기위해서 Tomcat에 있는 기본 어플리케이션을 다 지워 주어야 한다.
Jenkins는 `/apps/tomcat/webapps` 경로안에 `ROOT`와 `ROOT.war`파일이 생성되기 때문.

```bash
cd /apps/tomcat/webapps
rm –rf *
ls -la
```

#### Tomcat Start & Stop

시작!

```bash
cd /apps/tomcat/bin
./startup.sh
```

종료!

```bash
cd /apps/tomcat/bin
./shutdown.sh
```

### 2-5. Tomcat 포트포워딩 (80->9090)

[포트포워딩 참고](https://blog.outsider.ne.kr/580)
> 1024 이하로 동작하는 어플리케이션을 실행하기 위해서는 root권한이 필요하다. 
> tomcat을 일반 사용자 계정으로 실행하기 위하여 9090포트로 설정하였다. 
> 그런데 http://ip 로 웹 어플리케이션이 호출되기 위해서는 80포트로 동작해야한다. 
> 이런 문제를 해결하기 위해서 root사용자로 포트포워딩 설정을 한다. 80으로 오는 요청을 9090으로 전달하는 것이다.

```bash
iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
iptables -t nat -L
```

### 2-6. github계정과 연동하여 Jenkins 빌드 및 배포 준비

Jenkins에 `commend shell`로 Tomcat의 `deploy.sh`을 통해서 배포 및 서버를 재시작하는 스크립트를 작성했다.
`dontKillMe`는 Jenkins가 빌드가 완료되면, 모든 프로세스를 종료하기 때문에, tomcat을 시작하는 프로세스는 종료하지 말라는 명령어다.

```bash
vi /apps/tomcat/deploy.sh
```

```bash
#!/bin/sh

if [ -z "`ps -eaf | grep java|grep /apps/tomcat/bin`" ]; then
        cp /home/oz/.jenkins/workspace/todo/target/todo-0.0.1-SNAPSHOT.war /apps/tomcat/webapps/ROOT.war
        BUILD_ID=dontKillMe /apps/tomcat/bin/startup.sh
else
       ps -eaf | grep java | grep /apps/tomcat/bin | awk '{print $2}' |
       while read PID
               do
               echo "Killing $PID ..."
               kill -9 $PID
               echo
               echo "Tomcat  is being shutdowned."
               done
        cp /home/oz/.jenkins/workspace/todo/target/todo-0.0.1-SNAPSHOT.war /apps/tomcat/webapps/ROOT.war
        BUILD_ID=dontKillMe /apps/tomcat/bin/startup.sh
fi
```

`deploy.sh`를 자신의 디렉토리구조에 알맞게 작성을 했다면, permission을 `755`로 변경해 준다.

```bash
chmod 755 deploy.sh
```

### 2-7. Jenkins Item 등록하기

1. 이름은 Github 프로젝트 이름과 동일하게 한다.
2. Credentials는 `None`으로 두어도 된다.
3. Branch Specifier...는 해당 브랜치에 소스코드가 업데이트 (push) 되면 Jenkins는 빌드를 자동으로 알아서 해준다.
4. `Add Build Step > Excute Shell`을 통해서 Jenkins가 빌드가 완료되면, 어떤 일을 수행 할 것인지 사용자가 지정해 줄 수 있다.

**GITHUB Repository**가 실제 `pom.xml` 이 없는 디렉토리거나, 실제 구동되는 디렉토리가 아니라면, cd를 통해서 어플리케이션이 구동되는 디렉토리로 옮겨준다.
`mvn`을 통해서 `-Dmaven.test.skip` 옵션을 수행한다. `true`인 경우 테스트코드를 건너 뛴다, `false`인 경우 테스트코드를 수행 한다.
`/apps/tomcat/deploy.sh` 아까 위에서 설정한 `deploy.sh`를 Jenkins가 수행할 수 있도록 지정해준다.

```bash
cd ./reservation-service
mvn  -Dmaven.test.skip=true clean package
/apps/tomcat/deploy.sh
```

#### Jenkins가 제 역할을 다하며 빌드에 성공을 했을 경우!

`Console Output`에 나오는 메세지들이다.

```bash
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.497 s
[INFO] Finished at: 2017-07-26T19:43:50+09:00
[INFO] Final Memory: 19M/48M
[INFO] ------------------------------------------------------------------------
+ /apps/tomcat/deploy.sh
Killing 29275 ...

Tomcat  is being shutdowned.
Tomcat started.
Finished: SUCCESS
```


### 마무리 확인

spring에서 사용하는 `application.properties`나 다른 Environment Value들이 있는 파일이 있다면, 
`/home/아이디/.jenkins/workspace/젠킨스아이템/...` 경로에 `application.properties`파일을 넣어주어야 한다.

`application.properties`가 없는 경우, 빌드에서는 Error가 나지 않지만, Tomcat에서 실행 할 때에 `404`를 보게 될 것이다.

Tomcat에 포트포워딩이 잘되어있다면, http://ip 만으로도 Tomcat에 접근이 가능 할 것이다. (80으로 들어온 포트를 9090)으로 포워딩 했기 때문에!


