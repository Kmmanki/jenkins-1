# jenkins와 깃 허브를 이용한 자동빌드 

jenkins를 사용하면 깃허브 push 시 자동으로 빌드 및 배포가 가능하다고 하여 공부하기 위해 진행하였다.
전채적인 방식은 다음과 같다.

1. 프로젝트를 만든 뒤 깃허브에 push한다.
2. 도커에 jenkins 이미지를 받고 실행한다.
3. jenkins와 github를 연동시킨다.
4. 프로젝트를 push하여 jenkins가 자동빌드를 하는지 확인한다.
시작!!!!


1 프로젝트를 만든 뒤 깃허브에 push한다.
프로젝트는 springBoot gradle로 만들었다. 일반 java 프로젝트로 만들어도 무관 할 듯하다.
build.gradle에 빌드 완료 후 확인할 수 있도록 아래의 코드를 추가하였다.


![스크린샷 2020-08-03 오전 11 20 43](https://user-images.githubusercontent.com/50133267/89140131-b5710c80-d57b-11ea-9dea-48323a537083.png)


이후 커밋, 푸쉬를 하여 깃허브에 올린다.

2. 도커에 jenkins 이미지를 받고 실행한다.

docker search jenkins로 젠킨스 이미지들을 찾아보았다.
처음에는 jenkins가 공식 이미지여서 받아 실행 했더니 설치가 잘 되지 않아 검색해보니 jenkins/jenkins 이미지를 받아 실행하면 된다고 하여 jenkins/jenkins 이미지르 사용하였다.

![스크린샷 2020-08-03 오전 11 26 39](https://user-images.githubusercontent.com/50133267/89140313-3fb97080-d57c-11ea-9119-25788fa6e195.png)


jenkins/jenkins 이미지를 풀 받는다.
docker pull jenkins/jenkins
이미지 받은 것 확인
docker images


![스크린샷 2020-08-03 오전 11 29 57](https://user-images.githubusercontent.com/50133267/89140480-b6566e00-d57c-11ea-8e66-3be62126d065.png)


jenkins/jenkins 이미지를 받았다면 jenkins를 실행한다.
sudo docker run -i -t -p 32760:8080 --name jenkins1 jenkins/jenkins  

-i 는 상호 입출력
-t 는 bash쉘 사용
-p 는 포트바인딩 jenkins의 기본포트는 8080으로 내 로컬에서 32760로 접근 하면 jenkins로 접근하게 해준다.

인스톨 완료가 안나올텐데 로컬의 32760으로 접속해보자
127.0.0.1:32760


![스크린샷 2020-08-03 오전 11 50 02](https://user-images.githubusercontent.com/50133267/89141397-7fce2280-d57f-11ea-843d-6cf11c5f2713.png)

위와 같은 이미지가 나올 것이다. 이제 bash를 사용하여 jenkins 접근한 뒤 
/var/jenkins_home/secrets/initialAdminPassword 위치에 있는 파일의 인스톨 패스워드를 찾아야한다.

새로운 터밀널에 아래 명령어를 입력하면 jenkins 컨테이너에 접근 가능하다.
이후 파일에 접근 하여 읽으면 인스톨 패스워드를 알 수 있다.
docker exec -it jenkins1 /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword


![스크린샷 2020-08-03 오전 11 56 18](https://user-images.githubusercontent.com/50133267/89141724-611c5b80-d580-11ea-83ef-c4df0922a294.png)

확인한 패스워드를 입력하면 아래의 화면이 나온다.
왼쪽을 눌러주자

![스크린샷 2020-08-03 오전 11 57 38](https://user-images.githubusercontent.com/50133267/89141793-945eea80-d580-11ea-9172-d16a305540f9.png)

인스톨 화면이 나오고 여러 플러그인을 인스톨한다. 만약 여기서 Folders 인스톨이 잘 진행이 안된다면 jenkins/jenkins 이미지가 아닌 jenkins 이미지가 아닌지 확인해보자

![스크린샷 2020-08-03 오후 12 22 24](https://user-images.githubusercontent.com/50133267/89142985-0a188580-d584-11ea-9db9-1ec87141d910.png)


이후 계정 생성을 한뒤 로그인 하면

![스크린샷 2020-08-03 오후 12 25 45](https://user-images.githubusercontent.com/50133267/89143136-7b583880-d584-11ea-9927-15c5837b8955.png)

젠킨스 인스톨이 되었다!



3. jenkins와 github연동

좌측 상단 새로운 아이템 -> freeStyle Project -> OK

![스크린샷 2020-08-03 오후 12 29 48](https://user-images.githubusercontent.com/50133267/89143299-0df8d780-d585-11ea-9dfa-486e4ef2fa89.png)

general tap에 깃허브 클릭 -> 자신의 깃허브 레퍼지토리 url

소스코드관리 -> 깃 체크 -> 자신의 깃허브 레퍼지토리 url

![스크린샷 2020-08-03 오후 12 34 48](https://user-images.githubusercontent.com/50133267/89143563-fec65980-d585-11ea-9436-4781628d3db4.png)

소스코드 관리 -> 깃체크 -> credentials -> add -> jenkins

userName : github Id
password : github PW

이후 add


<img width="994" alt="스크린샷 2020-08-03 오후 12 39 03" src="https://user-images.githubusercontent.com/50133267/89143711-7a280b00-d586-11ea-904e-9efe0b78b4e5.png">

ceredentials 를 방금 설정한 것으로 변경

Branches to build -> 빌드할 브랜치 설정 (일단 마스터브랜치로 했다.)


빌드유발 -> github hook trigger for GTIScm Polling

<img width="1052" alt="스크린샷 2020-08-03 오후 12 42 56" src="https://user-images.githubusercontent.com/50133267/89143844-e571dd00-d586-11ea-94d7-1d46a8da9692.png">


build -> add build step 선택 -> excute shell
command에 ./gradlew clean print 입력 (빌드 커맨드로 maven이나 일반 java라면 달라질 수 있다. ) 저장

<img width="775" alt="스크린샷 2020-08-03 오후 12 45 36" src="https://user-images.githubusercontent.com/50133267/89143934-3f72a280-d587-11ea-9ea0-2e5d621d6ed5.png">


github에 웹훅을 적용해야한다. 그러나 웹훅 적용 시 url이 필요하다. 이를 해결하기위해 ngrok을 사용한다.
ngrok은 아이피+port를 url로 사용할 수 있게 해준다
ngrok 설치 후 해당 디렉토리로 이동하여 아래 커맨드를 실행 한다.
./ngrok http 32760

![스크린샷 2020-08-03 오후 1 02 21](https://user-images.githubusercontent.com/50133267/89144767-9aa59480-d589-11ea-9728-a23d473cf8a3.png)


![스크린샷 2020-08-03 오후 1 03 33](https://user-images.githubusercontent.com/50133267/89144816-c4f75200-d589-11ea-8ab1-78ecc04dca2d.png)


http://05f0f56a3f40.ngrok.io 가 나의 젠킨스 주소가 되는것이다. 
해당 url을 브라우저에 넣으면 젠킨스로 접속 가능할 것이다.
이제 깃허브에 젠킨스 웹훅을 설정하자.


github -> 할당한 레퍼지토리 -> settings -> webhooks -> add webhook -> payload에
http://05f0f56a3f40.ngrok.io/github-webhook/
처럼 뒤에 /github-webhook/을 넣어서 저장하자


4. 프로잭트를 push하여 자동빌드 되는지 확인하자
프로젝트를 깃허브에 푸쉬하면 빌드 대기목록에 해당 프로젝트가 나온다!


<img width="1059" alt="스크린샷 2020-08-03 오후 1 18 22" src="https://user-images.githubusercontent.com/50133267/89148149-f6751b00-d593-11ea-927c-62521af58134.png">


젠킨스 내부의 해당 프로젝트의 콘솔을 확인하면 gredle.build에 넣은 print를 확인 할 수 있다.

<img width="1319" alt="스크린샷 2020-08-03 오후 2 20 58" src="https://user-images.githubusercontent.com/50133267/89148409-ad719680-d594-11ea-8217-a9e5c1d11dcd.png">




끝!
