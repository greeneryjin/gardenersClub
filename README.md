# [gardenersClub](https://www.notion.so/cea50616f805494b9018a1494b43b282?p=b8e48c9e89eb430d81f9ba0fad42a1dd&pm=c)
식물에 대한 정보를 얻거나 자신이 키우는 식물을 공유하며 소통할 수 있는 식물 커뮤니티 사이트입니다. 


### 목차 
[0. 프로젝트 설명](#explain)

[1. 트러블 슈팅](#troubleshooting) 


[2. 리팩토링](#refactoring)


[3. 사용한 개발 도구 및 라이브러리](#tool)


[4. 완성된 기능 및 이미지](#image)

# explain
> 노션 [프로젝트 설명](https://www.notion.so/Gardener-s-Club-b8e48c9e89eb430d81f9ba0fad42a1dd)



# troubleshooting
##### 1. 배포 에러

###### 에러 문구
```
CodeDeploy agent was not able to receive the lifecycle event. Check the CodeDeploy agent logs on your host and make sure the agent is running and can connect to the CodeDeploy server.
```

###### 원인
Fail 원인: EC2에 CodeDeploy 관련 IAM Role이 부여되기 전에 CodeDeploy Agent가 실행되면서 IAM Role을 못 가져간 것입니다. 


###### 해결 방법
1. Code Deploy 역할이 인스턴스에 부여되어 있는지 확인
2. CodeDeploy 의 Agent가 띄워져 있는지 확인
권한 요청 실패 발생 tail -F /var/log/aws/codedeploy-agent/codedeploy-agent.log
ec2에서 iam 역할을 나중에 추가할 경우, ec2 안에 aws agent를 다시 시작해야 합니다.


##### 2. AllowTraffic 에러

###### 에러 문구
```
The EC2 instance did not turn into the expected healthy state in target group. In private-a1-tg: {State: unhealthy,Reason: Target.ResponseCodeMismatch,Description: Health checks failed with these codes: [404]}.
```

###### 원인
: ALB에서 대상 그룹의 헬스 체크가 실패해서 발생했습니다


###### 해결 방법
웹사이트가 정상적인 응답을 줄 수 있도록 /{get매핑된 주소}을 작성해주고 200OK를 던져주면 ALB가 받고 정상적으로 처리합니다. 


3. 기본 생성자 에러
사진을 저장할 때, dto에서 기본 생성자가 없어서 에러가 터졌습니다.
```java
 @PostMapping(value = "/picture/save")
 public Header savePicture(@Validated SaveList saveList) {
        Picture picture = pictureService.savePicture(saveList);
        return Header.description(picture.getId(), "사진글이 등록되었습니다.");
}

//기본 생성자가 없을 경우, 에러가 터짐 
@Getter
public class SaveList  {
    private String tagName;
    
    public SaveList (String tagName){
        this.tagName = tagName;
    }
}
```

###### 원인 
스프링을 사용할 때, 스프링은 빈에 등록하고 DI를 주입합니다. 

이때, 리플렉션을 사용해서 객체를 생성합니다.

하지만 리플렉션에서도 확인할 수 없는 정보가 생성자의 인자 정보입니다. 기본 생성자를 반드시 명시를 해야 스프링이 객체를 생성해서 활용할 수 있습니다.  


#### 수정된 코드 
```java
@Getter
@NoArgsConstructor //추가
public class SaveList  {
    private String tagName;
    
    public SaveList (String tagName){
        this.tagName = tagName;
    }
}
```


# refactoring
1. 쿼리 수 개선하기
마이페이지에서 회원이 좋아요, 스크랩 개수를 합한 것을 뿌려주는 페이지가 있습니다.
기존이 팀원이 데이터베이스에서 쿼리를 작성한 후, dto로 해당 정보를 담을 때 쿼리가 8번이 조회되는 것을 보고

리팩토링이 필요하다는 생각이 들었습니다.


querydsl를 사용해서 1번의 쿼리로 유지 보수를 진행했습니다. 

###### 기존 코드 
```java
AccountDto accountDto = AccountDto.builder()
         .accountId(account.getId())
         .nickName(account.getNickName())
         .selfInfo(account.getSelfInfo())
         .profileUrl(account.getProfileUrl())
         .followerCount(account.getFollower().size())
         .followingCount(account.getFollowing().size())
         .scrapCount(account.getPictureScrapList().size() + account.getMagazineScraps().size() + account.getPlantDicScrapList().size())
         .likeCount(account.getPictureLikeList().size() + account.getMagazineLikes().size())
         .build();
```

###### 결과
![Untitled (2)](https://github.com/greeneryjin/gardenersClub/assets/87289562/c0c7a9d2-514d-4c50-bca0-4e324d96a0ee)


###### 수정된 코드 
```java
List<AccountFeed> accountFeeds = queryFactory
                .select(new QAccountFeed(
                        account.id,
                        account.nickName,
                        account.selfInfo,
                        account.profileUrl,
                        account.follower.size(),
                        account.following.size(),
                        account.pictureScrapList.size().add(account.plantDicScrapList.size()).add(account.magazineScraps.size()),
                        account.pictureLikeList.size().add(account.magazineLikes.size())
                ))
                .from(account)
                .where(account.id.eq(accountId))
                .fetch();

        log.info("팔로우 개수 {}", accountFeeds.get(0).getFollowerCount());
        log.info("팔로워 개수 {}", accountFeeds.get(0).getFollowingCount());
        log.info("좋아요 개수 {}", accountFeeds.get(0).getLikeCount());
        log.info("스크랩 개수 {}", accountFeeds.get(0).getScrapCount());
```

###### 결과 
![Untitled (3)](https://github.com/greeneryjin/gardenersClub/assets/87289562/bbfbdcf8-6a6c-4a7f-9ac0-3b58fe73a719)


2. 페이징 처리 조회 속도 개선
개발 팀원이 페이징을 처리를 하지 않고 List를 사용해서 데이터를 반환했습니다.

식물 사전에서 식물의 종은 총 183개로 2개의 테이블이 연관 관계로 맵핑되어 있습니다.

식물 테이블의 컬럼 개수는 총 183개, 식물의 사진을 구성하는 테이블의 컬럼 개수는 약 600개입니다. 

리스트로 반환하니 포스트맨에서 불러올 때, 13초가 걸렸습니다.

리팩토링이 필요하다는 생각이 들었습니다

###### 기존 코드 
```java
private List<PlantDicSimpleDto> searchPage(PlantDicSearchCondition condition) {
        //조건에 맞는 식물사전 조회
        List<PlantDicSimpleDto> plantDicSimpleDtoList = queryFactory
                .select(new QPlantDicSimpleDto(
                        plantDic.id,
                        plantDic.plantName,
                        //생략//,
                        plantDic.flowerLanguage))
                .from(plantDic)
                .join(plantDic)
                .where(
                        filter_classification(condition.getClassification()),
                        //생략//),
                        filter_dog(condition.getDog())
                )
								.orderBy(plantDic.id.asc())
                .fetch();
```
###### 결과 
![Untitled (4)](https://github.com/greeneryjin/gardenersClub/assets/87289562/28ea82dd-401e-418f-91b5-8216802dcbb9)



###### 수정된 코드 
```java
private Page<PlantDicSimpleDto> searchPage(PlantDicSearchCondition condition, Pageable pageable) {
        //조건에 맞는 식물사전 조회
        QueryResults<PlantDicSimpleDto> plantDicSimpleDtoList = queryFactory
                .select(new QPlantDicSimpleDto(
                        plantDic.id,
                        plantDic.plantName,
												//생략//,
                        plantDic.flowerLanguage))
                .from(plantDic)
                .join(plantDic)
                .where(
                        filter_classification(condition.getClassification()),
												//생략//
                        filter_dog(condition.getDog())
                )
                .orderBy(plantDic.id.asc())
                .offset(pageable.getOffset())
                .limit(pageable.getPageSize())
                .fetchResults();
```
###### 결과 
![Untitled (5)](https://github.com/greeneryjin/gardenersClub/assets/87289562/eea35b42-a4d6-4d41-ac3f-ba93dcd08b7f)

페이징 처리를 하고 나니 13초였던 기존 시간대에서 761ms 단위로 떨어졌습니다. 



# tool
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

1. Pull Request의 요청 및 머지는 Github을 활용해 이뤄집니다.
2. Pull Request 이벤트가 발생하면 Git actions 실행되고 빌드를 시작합니다.
3. Github Actions는 프로젝트 파일을 압축하고 AWS S3로 전송합니다. 
4. CodeDeploy에게 배포를 요청합니다.

**파트 별 파이프라인** 

백엔드: git action → s3 → codeDeploy → ec2 → docker → spring 빌드

프론트엔드 : git action → s3 → codeDeploy → ec2 → docker → react 빌드
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


# image
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




