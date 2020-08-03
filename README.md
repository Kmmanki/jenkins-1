# jenkins와 깃 허브를 이용한 자동빌드 

jenkins를 사용하면 깃허브 push 시 자동으로 빌드 및 배포가 가능하다고 하여 공부하기 위해 진행하였다.
전채적인 방식은 다음과 같다.

1. 프로젝트를 만든 뒤 깃허브에 push한다.
2. 도커에 jenkins 이미지를 받고 실행한다.
3. jenkins와 github를 연동시킨다.
4. 프로젝트를 push하여 jenkins가 자동빌드를 하는지 확인한다.
시작!!!!

1. 프로젝트를 만든 뒤 깃허브에 push한다.
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






