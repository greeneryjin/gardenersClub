# gardenersClub
식물 관리 기능을 제공하고 식물 집사님들과 식물팁을 공유하며 소통할 수 있는 식물 관리 & 커뮤니티 플랫폼입니다. 


사용 언어
```
- JAVA 11
```


사용 기술
```
- springboot
- Mysql
- S3
- Querydsl
- jpa
- jwt
- docker
- nginx
- AWS
- git action
```

서버 구성(운영도메인, 개발도메인)
![image.jpg1](https://github.com/greeneryjin/gardenersClub/assets/87289562/0a5ae9b6-e312-4ef7-80ce-a66c688c2a25) |![image.jpg2](https://github.com/greeneryjin/gardenersClub/assets/87289562/11cd3156-e0cf-4012-b3bd-7a911f5a729f)
--- | --- | 
* * *


배포
```
- git action
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


라이브러리
```
- react 
- lombok
- gradle
- Swgger
```

도메인(개발/운영)
- dev.gardenersclub.co.kr
- www.gardenersclub.co.kr


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
- 식물사전 전체 조회 페이지
- 특정 식물 조회 페이지
- 식물 사전 리뷰 작성 페이지

<p align="center">
  <img alt="KakaoTalk_20231010_120429019" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7d150468-c7ea-45ce-ad82-3c3f1d729522" align="center" width="33%">
  <img alt="가드너스클럽 사진" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/a91b399c-bc13-4d38-8722-95f78bc261e4" align="center" width="33%">
  <img alt="KakaoTalk_20231010_120628782" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/33d19971-9331-4ef6-80eb-0b569c5e97c8" align="center" width="33%">
</p>
<br/> 

- 최신순 리뷰
- 인기순 리뷰 
- 내가 작성한 리뷰
<p align="center">
  <img alt="KakaoTalk_20231010_121850224" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/fd623b6e-709d-4d11-aa09-0093acd57d9f" align="center" width="33%">
  <img alt="KakaoTalk_20231010_121835304" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/13099f03-f371-4eb4-8b1f-7766dfaca21d" align="center" width="33%">
  <img alt="KakaoTalk_20231010_121844221" src="https://github.com/greeneryjin/gardenersClub/assets/87289562/434b0b39-9fe6-43c6-9fad-6651b9ba778b" align="center" width="33%">
</p>

* * *

2. 마이페이지
- 회원 수정
- 좋아요
- 스크랩

  <p align="center">
  <img src="" align="center" width="33%">
  <img src="" align="center" width="33%" width="200" height="200">
  <img src="" align="center" width="33%" alt="KakaoTalk_20231010_121902047" >
</p>

* * *

3. 로그인
- 네이버 
- 카카오

로그인 순서도
  1. 로그인 화면 $\rightarrow$ 신규 가입자 화면 $\rightarrow$ 신규 가입 로그인 완료 $\rightarrow$ 웹사이트 리다이렉트 화면
<p align="center">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7a1dcd0f-89bd-4a13-93c5-f78ed7baced8" align="center" width="33%">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/3809aca4-5d6f-44d4-b13d-86f065eb4b39" align="center" width="33%" width="200" height="200">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/d0854d64-d6ff-4404-8356-e32b85964e4a" align="center" width="33%" alt="KakaoTalk_20231010_121902047" >
</p>


  2. 로그인 화면 $\rightarrow$ 기존 가입자 로그인 $\rightarrow$ 웹사이트 리다이렉트 화면 
<p align="center">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/7a1dcd0f-89bd-4a13-93c5-f78ed7baced8" align="center" width="40%">
  <img src="https://github.com/greeneryjin/gardenersClub/assets/87289562/d0854d64-d6ff-4404-8356-e32b85964e4a" align="center" width="40%" alt="KakaoTalk_20231010_121902047" >
</p>

* * *



