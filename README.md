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






