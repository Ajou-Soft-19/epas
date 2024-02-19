 # [EPAS (Emergency vehicle Pre-Alerting System)]

---
## 프로젝트 소개
- EPAS는 차량 주행자가 응급차량의 접근을 미리 알 수 있습니다.
- 응급차량은 응급상황에서 보다 쉽고 빠르게 목적지에 도달할 수 있습니다.
- 주행자는 응급차량의 접근을 보고 받아 사고 위험없이 미리 길을 터줄 수 있습니다.
---
## 팀원 구성
| 정선문 | 장연지 | 김민규 |
|-|-|-|
|[![정선문 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/32717522?v=4)](https://github.com/bandall)|[![장연지 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/48924755?v=4)](https://github.com/MillPRE)|[![김민규 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/48954288?v=4)](https://github.com/kmkkkp)|
|Backend|Backend|Frontend|

---

## 1. 개발 환경
### Backend
| SpringBoot                                                                                   |PostgreSQL|---|
|----------------------------------------------------------------------------------------------|---|---|
| ![SpringBoot](https://pbs.twimg.com/profile_images/1235868806079057921/fTL08u_H_200x200.png) |![PostgreSQL](https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Postgresql_elephant.svg/200px-Postgresql_elephant.svg.png) | |

### Frontend
| Flutter                                                                             |
|-------------------------------------------------------------------------------------|
| <img src="https://logowik.com/content/uploads/images/flutter5786.jpg" width="200"/> |

### 버전 및 이슈 관리
| Github                                                                                         |
|------------------------------------------------------------------------------------------------|
| <img  src="https://github.githubassets.com/assets/GitHub-Mark-ea2971cee799.png" width="200" /> |

### 협업 툴
| Notion                                                                                                                      | Discord                                                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Notion-logo.svg/1200px-Notion-logo.svg.png" width="200"> | <img src="https://assets-global.website-files.com/6257adef93867e50d84d30e2/636e0a6a49cf127bf92de1e2_icon_clyde_blurple_RGB.png" width="200" /> |

### 서비스 배포 환경
| Google Cloud                                                                                               |
|------------------------------------------------------------------------------------------------------------|
| <img src="https://static-00.iconduck.com/assets.00/google-cloud-icon-1024x823-wiwlyizc.png" width="200" /> |

### 디자인
| Figma                                                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <img src="https://cdn.sanity.io/images/599r6htc/localized/46a76c802176eb17b04e12108de7e7e0f3736dc6-1024x1024.png?w=804&h=804&q=75&fit=max&auto=format" width="200" /> |
---
## 2. 기술 소개

### API
| Google Map API                              | Naver Map API                               | Osrm API                                   |
|---------------------------------------------|---------------------------------------------|--------------------------------------------|
| <img src="img/api/google.png" width="200" /> | <img src="img/api/naver.png" width="200" /> | <img src="img/api/osrm.png" width="200" /> |

---

## 3. 페이지별 기능
### 3-1. 회원가입 화면
| 계정 생성 초기 화면 | 계정 생성 폼                         | 계정 생성 버튼 클릭                        | 
|-------------|---------------------------------|------------------------------------|
| ![계정 생성 초기 화면 ](img/4/signup1.jpeg)       | ![계정 생성 폼 ](img/4/signup2.jpeg) | ![계정 생성 버튼 클릭](img/4/signup3.jpeg) |
### 3-2. 로그인 화면
| 로그인 초기 화면                        | 로그인 폼                        | 로그인 성공                        |
|----------------------------------|------------------------------|-------------------------------|
| ![로그인 초기 화면 ](img/4/login1.jpeg) | ![로그인 폼 ](img/4/login2.jpeg) | ![로그인 성공](img/4/account.jpeg) |
### 3-3. Map 화면
| Map 화면                                           |
|--------------------------------------------------|
| <img src="img/4/navigation1.jpeg" width="200" /> |
### 3-4. 네비게이션 화면
| 네비게이션 초기 화면                           | 네비게이션 목적지 검색                            | 찾은 경로 보여줌                           | 네비게이션 시작                            |
|---------------------------------------|-----------------------------------------|-------------------------------------|-------------------------------------| 
| ![네비게이션 초기 화면 ](img/4/navigation1.jpeg) | ![네비게이션 경로 검색 ](img/4/navigation2.jpeg) | ![네비게이션 시작](img/4/navigation3.jpeg) | ![네비게이션 시작](img/4/navigation4.jpeg) |

### 3-5. Account 화면
| Account 메인 화면                                | 사용자 이름 변경                                     | 사용자 이름 변경 완료                                  | 응급차량 권한 요청                                    | 응급차량 권한 요청 승인 결과 확인                           |
|----------------------------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <img src="img/4/account1.jpeg" width="200"/> | <img src="img/4/account2.jpeg" width="200" /> | <img src="img/4/account3.jpeg" width="200" /> | <img src="img/4/account4.jpeg" width="200" /> | <img src="img/4/account5.jpeg" width="200" /> |
### 3-6. 차량 등록 화면
| 차량 등록 초기 화면                             | 차량 추가 화면                            | 차량 추가 완료                            |
|-----------------------------------------|-------------------------------------|-------------------------------------|
| ![차량 등록 초기 화면 ](img/4/addVehicle1.jpeg) | ![차량 추가 화면](img/4/addVehicle2.jpeg) | ![차량 추가 완료](img/4/addVehicle3.jpeg) |
### 3-7. 응급차량 응급 모드
| 응급차량 응급모드                         | 응급차량 일반모드                         | 응급차량 응급상황 주행시작                    |
|-----------------------------------|-----------------------------------|-----------------------------------|
| <img src="img/4/emergency1.jpeg"> | <img src="img/4/emergency2.jpeg"> | <img src="img/4/emergency3.jpeg"> |
### 3-8. 경고 알람 발생 화면
| 경고 알람 발생 화면                               |
|-------------------------------------------|
| <img src="img/4/alert1.png" width="200" /> |
### 3-9. Admin 화면
| 관리자 Account 화면                      |   모니터링 페이지                     |
|-------------------------------------|-----------------------------|
| <img src="img/4/adminAccount.jpeg"> |   <img src="img/4/admin3.png"> |

### 3-10. 권한 관리 화면
|응급차량 권한 요청 승인/거절 페이지|
|-|
|<img src="img/4/admin2.png">|

---
## 4. 알고리즘
### 4-1. ERD
![ERD](img/algorithm/ERD.png)
### 4-2. Alert Logic

### 4-3. Map Matching Logic

### 4-4. Front Alert Logic