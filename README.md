# [gardenersClub](https://www.notion.so/cea50616f805494b9018a1494b43b282?p=b8e48c9e89eb430d81f9ba0fad42a1dd&pm=c)
식물에 대한 정보를 얻거나 자신이 키우는 식물을 공유하며 소통할 수 있는 식물 커뮤니티 플랫폼입니다. 


사용 언어
```
- JAVA 11
- js
```


사용 기술
```
- springboot
- spring security
- Querydsl
- jpa
- jwt
- docker
- nginx
- git action
- AWS codeDeploy
- AWS route53
- AWS alb
- AWS cloudfront
- AWS Mysql
- AWS S3
```

라이브러리
```
- react 
- lombok
- gradle
- Swgger
```
<br/> 

서버 구성
  * 도메인(개발/운영) 기준으로 구성(현재 서비스 종료)
    + www.gardenersclub.co.kr
    + dev.gardenersclub.co.kr
   
<div align="center"> 
   <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/0a5ae9b6-e312-4ef7-80ce-a66c688c2a25" width="48%">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/11cd3156-e0cf-4012-b3bd-7a911f5a729f" width="48%">
</div>

---
배포
```
- git action
- s3
- aws codeDeploy
```
<div align="center"> <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/39b3c60e-c393-4f81-b86c-1819a91cc559"> </div>

* * *


인프라 환경 
```
- aws vpc
- public subnet
- private subnet
- aws ALB
- router53
- CM
- ec2
```
<div align="center"> <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/9889ea83-1a97-42a5-a977-2ed2d2e6fed7"> </div>

* * *

Swaager api
- api 총 개수는 20개입니다.

<div align="center"> <img width="1280" alt="스웨거" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/a34dbdfb-12f0-410d-a39f-778b1cabe315"> </div>

* * *

ERD
- 테이블의 총 개수는 20개입니다. 


외부 데이터 
- 기상청 Open api(단기예보, 중기예보)
- 농사로 Open api(실내 정원용 식물)
- 동사무소 Open api(동사무소 행정 주소)

웹사이트 기능
1. 커뮤니티
   * 식물사전
      - 식물사전 전체 조회
      - 특정 식물 조회
      - 식물 사전 리뷰 작성

<p align="center">
  <img alt="KakaoTalk_20231010_120429019" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7d150468-c7ea-45ce-ad82-3c3f1d729522" align="center" width="32%">
  <img alt="가드너스클럽 사진" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/a91b399c-bc13-4d38-8722-95f78bc261e4" align="center" width="32%">
  <img alt="KakaoTalk_20231010_120628782" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/33d19971-9331-4ef6-80eb-0b569c5e97c8" align="center" width="32%">
</p>

<br/>


   - 식물사전 리뷰 
       * 최신순 리뷰
       * 인기순 리뷰 
       * 내가 작성한 리뷰
<p align="center">
  <img alt="KakaoTalk_20231010_121850224" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/fd623b6e-709d-4d11-aa09-0093acd57d9f" align="center" width="32%">
  <img alt="KakaoTalk_20231010_121835304" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/13099f03-f371-4eb4-8b1f-7766dfaca21d" align="center" width="32%">
  <img alt="KakaoTalk_20231010_121844221" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/434b0b39-9fe6-43c6-9fad-6651b9ba778b" align="center" width="32%">
</p>

<br/> 

  * 사진
    - 사진 전체 조회
    - 특정 사진 조회
    - 사진 저장
   
<p align="center">
  <img alt="7" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/01bfe4fe-0871-41c4-adc5-a3ddda2cdf29" align="center" width="32%">
  <img alt="3" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/8bae42ca-502a-470b-bf49-796549e96081" align="center" width="32%">
  <img alt="15" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/fdffe102-5b53-4d27-955a-1bea72d41bc5" align="center" width="32%">
</p>

* * *



2. 마이페이지
  - 나의 피드
  - 스크랩 북
  - 회원 수정

<p align="center">
  <img alt="11" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/033f6614-7d2b-4095-bcf9-2b7f2c0605c0" align="center" width="32%">
  <img alt="13" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/e598b521-6a44-4dee-99ff-4ea85fef3b75" align="center" width="32%">
  <img alt="14" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/9d4396d8-471f-482c-b81f-e36b3490c519" align="center" width="32%">
</p>

* * *


3. 로그인
  - 네이버 
  - 카카오

로그인 순서도
  1. 로그인 화면 $\rightarrow$ 신규 가입자 화면 $\rightarrow$ 신규 가입 로그인 완료 $\rightarrow$ 웹사이트 리다이렉트 화면
<p align="center">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7a1dcd0f-89bd-4a13-93c5-f78ed7baced8" align="center" width="32%">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/3809aca4-5d6f-44d4-b13d-86f065eb4b39" align="center" width="32%" width="200" height="200">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/d0854d64-d6ff-4404-8356-e32b85964e4a" align="center" width="32%" alt="KakaoTalk_20231010_121902047" >
</p>

<br/> 

  2. 로그인 화면 $\rightarrow$ 기존 가입자 로그인 $\rightarrow$ 웹사이트 리다이렉트 화면 
<p align="center">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7a1dcd0f-89bd-4a13-93c5-f78ed7baced8" width="48%">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/d0854d64-d6ff-4404-8356-e32b85964e4a" width="48%" alt="KakaoTalk_20231010_121902047">
</p>

* * *




