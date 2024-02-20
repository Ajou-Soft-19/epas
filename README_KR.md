# [EPAS (Emergency vehicle Pre-Alerting System)]

- [[EPAS (Emergency vehicle Pre-Alerting System)]](#epas-emergency-vehicle-pre-alerting-system)
  - [프로젝트 소개](#프로젝트-소개)
  - [Team Ajou Moses](#team-ajou-moses)
  - [Project Repository](#project-repository)
  - [🛠️ Tech Stack 🛠️](#-tech-stack-)
  - [🧰 Development Tools 🧰](#-development-tools-)
  - [API Used](#api-used)
- [알고리즘](#알고리즘)
  - [맵 매칭 알고리즘](#맵-매칭-알고리즘)
  - [경고 대상 선정 알고리즘](#경고-대상-선정-알고리즘)
  - [경고 메시지 타입](#경고-메시지-타입)
- [백엔드 서버 스택](#백엔드-서버-스택)
  - [데이터베이스](#데이터베이스)
  - [OSRM (Open Source Routing Machine)](#osrm-open-source-routing-machine)
  - [Spring boot server](#spring-boot-server)
- [프론트엔드 스택](#프론트엔드-스택)

## 프로젝트 소개

- EPAS는 차량 주행자가 응급 차량의 접근을 미리 알 수 있도록, 경고 알림을 제공하는 서비스입니다.
- 필터링 알고리즘을 통해 응급차량과 주변 차량의 거리, 속도, 방향 등을 고려하여 경고 알림을 제공합니다.
- 경고 알림을 통해 차량 주행자는 당황하지 않고 응급 차량에게 길을 양보할 수 있고, 응급 차량은 빠르게 목적지에 도달할 수 있습니다.

## Project Repository

|         리포지토리명          |             설명              |                                         링크                                          |
| :---------------------------: | :---------------------------: | :-----------------------------------------------------------------------------------: |
|           EPAS APP            |       Flutter EPAS App        |                [EPAS APP](https://github.com/Ajou-Soft-19/service-app)                |
|       EPAS 서비스 서버        |       EPAS 백엔드 서버        |          [EPAS 서비스 서버](https://github.com/Ajou-Soft-19/service-server)           |
|    EPAS 차량 모니터링 서버    |    EPAS 차량 모니터링 서버    |    [EPAS 차량 모니터링 서버](https://github.com/Ajou-Soft-19/spring-socket-server)    |
|        EPAS 인증 서버         |        EPAS 인증 서버         | [EPAS Authentication server](https://github.com/Ajou-Soft-19/Spring-JWT-Login-server) |
| EPAS 도로 네트워크 시뮬레이터 | EPAS 도로 네트워크 시뮬레이터 |    [EPAS 도로 네트워크 시뮬레이터](https://github.com/Ajou-Soft-19/road-simulator)    |

## Team Ajou Moses

| 정선문                                                                                                           | 장연지                                                                                                           | 김민규                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| [![정선문 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/32717522?v=4)](https://github.com/bandall) | [![장연지 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/48924755?v=4)](https://github.com/MillPRE) | [![김민규 깃허브 프로필 사진](https://avatars.githubusercontent.com/u/48954288?v=4)](https://github.com/kmkkkp) |
| Backend                                                                                                          | Backend                                                                                                          | Frontend                                                                                                        |

## 🛠️ Tech Stack 🛠️

<div align="center">
    <img src="https://img.shields.io/badge/SpringBoot-6DB33F?style=for-the-badge&logo=spring&logoColor=white">
    <img src="https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white">
    <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white">
    <br>
    <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white">
    <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white">
</div>

<br>

## 🧰 Development Tools 🧰

<div align="center">
    <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white">
    <img src="https://img.shields.io/badge/Notion-ffffff?style=for-the-badge&logo=notion&logoColor=black">
    <img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white">
    <img src="https://img.shields.io/badge/Jira-0052CC?style=for-the-badge&logo=jira&logoColor=white">
    <br>
    <img src="https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white">
    <img src="https://img.shields.io/badge/GoogleCloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white">
    <img src="https://img.shields.io/badge/Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white">
</div>

## API Used

<div align="center">
    <img src="https://img.shields.io/badge/GoogleMapsAPI-4285F4?style=for-the-badge&logo=googlemaps&logoColor=white">
    <img src="https://img.shields.io/badge/NaverMapsAPI-03C75A?style=for-the-badge&logo=naver&logoColor=white">
    <img src="https://img.shields.io/badge/OSRMAPI-7D669E?style=for-the-badge&logo=openstreetmap&logoColor=white">
</div>

<br>

# 알고리즘

## 맵 매칭 알고리즘

차량의 위치를 실시간으로 추적하기 위해 GPS 데이터를 사용합니다. 하지만 GPS 데이터는 실제 위치와 차이가 있을 수 있습니다. 이를 보정하기 위해 맵 매칭 알고리즘을 사용합니다. 맵 매칭 알고리즘은 GPS 데이터를 실제 도로 네트워크 상의 위치로 변환해줍니다.

아래는 맵 매칭 사용 전과 후의 비교 사진입니다.

|                 맵 매칭 사용 전                  |                 맵 매칭 사용 후                  |
| :----------------------------------------------: | :----------------------------------------------: |
| <img src="./img/algorithm/before_map_match.jpg"> | <img src="./img/algorithm/before_map_match.jpg"> |

이를 통해 응급차량이 어떤 체크포인트를 지나가고 있는지, 어떤 도로를 통과하고 있는지 등을 정확하게 파악할 수 있습니다. 여기서 구한 도로 네트워크 상의 위치와 방위각을 이용해 경고 대상을 선정하고 경고 메시지를 전송합니다.

## 경고 대상 선정 알고리즘

저희가 직면한 가장 큰 기술적 문제는 경고 알림의 발행 기준과 그 전달 방식을 결정하는 것이었습니다. 단순히 응급차와의 거리만을 기준으로 한다면, 도로 네트워크 상에서는 멀지만 응급차와의 거리가 가깝다는 이유로 불필요한 경고를 받는 경우가 많아져 사용자 경험을 저하시킬 수 있습니다. 이 문제를 해결하기 위해 아래와 같은 알고리즘을 선정하였습니다.

응급 차량이 응급 상황을 활성화하면 백엔드 서버에서 지속적으로 (약 1초 간격) 응급 차량을 모니터링하고, 응급 차량과 마주칠 것으로 예상되는 차량에게 경고를 발송합니다.

### **경고 대상 선정 알고리즘**

```c
def issue_alert(emergency_car, checkpoints, vehicles):
  current_location = emergency_car.current_location
  next_checkpoint = get_next_checkpoint(current_location, checkpoints)

  # 1. Get vehicles within 500m radius of the next checkpoint
  nearby_vehicles = get_nearby_vehicles(next_checkpoint, vehicles, 500)

  # 2. Issue alert to vehicles using navigation
  for vehicle in nearby_vehicles:
    if vehicle.is_using_navigation:
      emergency_info = get_emergency_info(emergency_car)
      send_alert(vehicle, emergency_info)

  # 3. Issue alert to vehicles not using navigation but predicted to reach the checkpoint around the same time
  for vehicle in nearby_vehicles:
    if not vehicle.is_using_navigation:
      if will_arrive_simultaneously(vehicle, emergency_car, next_checkpoint):
        emergency_info = get_emergency_info(emergency_car)
        send_alert(vehicle, emergency_info)

  # 4. Issue additional alert to vehicles very close to the emergency vehicle
  very_close_vehicles = get_nearby_vehicles(current_location, vehicles, 160)
  for vehicle in very_close_vehicles:
    emergency_info = get_emergency_info(emergency_car)
    send_alert(vehicle, emergency_info)
```

위 코드는 저희가 선정한 경고 대상 선정 알고리즘의 수도 코드입니다.

차량 쿼리 범위와 체크포인트 간격 등은 제작한 시뮬레이션을 통해, 여러 시나리오를 테스트한 후 결정하였습니다. [도로 네트워크 시뮬레이션 도구](https://github.com/Ajou-Soft-19/road-simulator)

**[차량 모니터링]**

|                                                  차량 모니터링                                                   |
| :--------------------------------------------------------------------------------------------------------------: |
| ![vehicle_status.jpg](https://github.com/Ajou-Soft-19/epas/assets/32717522/d5e4424f-bc26-43f3-95c9-fb3693e7ad1f) |

사용자가 네비게이션 앱을 실행하면 차량 추적 서버를 통해 차량의 위치, 속도, 방위각을 모니터링 합니다. 이때 사용자의 정보는 익명 정보로 저장됩니다.

|                                             응급 차량 네비게이션 경로                                             |                                          응급 차량 네비게이션 체크포인트                                          |
| :---------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/addea671-3a1f-421c-ba28-a1c723cf2e50" width="500"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/93a2f2a4-6e6c-4f85-b9f4-1f90fc096e71" width="400"> |

1. 응급차량의 예상 주행 경로를 약 400m 간격으로 나누어 체크포인트로 저장하고, 응급차량이 현재 향하고 있는 다음 체크포인트를 중점으로 경고 대상을 선정합니다.

|                                                  경고 대상 선정                                                   |
| :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/de23df98-adfa-4580-8f65-b9d674ef36d8" width="500"> |

2. 다음 체크포인트를 중심으로 반경 `500m 내의 차량`을 `PostGIS`의 공간 쿼리를 이용하여 찾습니다. (위 사진의 파란색 원이 500m 반경입니다.)

3. 네비게이션을 사용하는 차량에게는 응급차량의 예상 경로와 현재 위치 정보를 담은 경보 메시지를 소켓을 통해 전달합니다. 각 네비게이션 애플리케이션은 설정된 네비게이션 주행 경로와 응급차량의 경로를 비교하여 교차하는 경우 사용자에게 경고 알림을 보여줍니다.

|                                                 경고 메시지 전송                                                  |
| :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/e7ae7c75-944a-44da-b45d-8e6156a4762d" width="500"> |

4. 네비게이션을 사용하지 않는 차량에 대해서는 차량의 현재 위치와 이동 방향을 고려하여, **다음 체크포인트에 응급차량보다 빠르게 도착하거나 비슷한 시간에 도착할 것으로 예상되는 경우 경고를 발행**하도록 하였습니다. (OSRM의 table api를 활용해 도로 네트워크 상에서 계산했습니다.)

빨간색 점은 응급차량, 파란색 점은 일반 차량 그리고 검은색 점은 경고를 받은 차량을 나타냅니다.

|                    거리가 먼 경고 대상 필터링                    |
| :--------------------------------------------------------------: |
| ![경도 대상 필터링 예시](/img/algorithm/algorithm_example_1.gif) |

응급차량과 직선 거리로는 가깝지만 도로 네트워크 상에서 멀리 떨어진 차량은 경고 대상에서 제외됩니다.

5. 또한, 응급차량과 매우 가까운 차량에게 경고를 확실히 전달하기 위해, 이동 방향을 고려하지 않고 도로 네트워크 상에서 응급차량과 `160m 이내에 있는 차량`에게 추가적으로 경고를 발행하도록 하였습니다. (빨간원이 이에 해당합니다.)

|                                               일반 차량의 경고 화면1                                               | 일반 차량의 경고 화면2                                                                                             |
| :----------------------------------------------------------------------------------------------------------------: | ------------------------------------------------------------------------------------------------------------------ |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/cbf3a24f-202b-412e-86ba-0cdc08adc0e5" height="500"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/d7fcf378-b9f3-4d6a-b787-84ebc3022db5" height="500"> |

발행된 경고에는 응급차량의 현재 위치, 라이센스 번호, 다음 체크포인트까지의 예상 주행 경로 등의 정보가 포함되어 있습니다. 이를 통해 네비게이션 앱에서는 응급차량의 접근 방향과 예상 주행 경로를 시각화하여 보여줍니다. 응급차량의 현재 위치는 경고를 받은 후 40초 간격으로 계속해서 전송됩니다. 사용자는 이 정보를 토대로 응급차량의 위치를 실시간으로 확인하고 대응할 수 있습니다.

## 경고 메시지 타입

경고 메시지는 일반 차량과 연결된 소켓을 통해 전송해줍니다. 응급 차량의 API 핸들러를 통해 응급 차량 경로를 다룰 수 있습니다. 아래는 각 메시지 타입입니다.

**[ALERT]**

```json
{
  "code": 200,
  "messageType": "ALERT",
  "data": {
    "emergencyEventId": 410,
    "checkPointId": 16,
    "licenseNumber": "947Y1201",
    "vehicleType": "FIRE_TRUCK_MEDIUM",
    "currentPathPoint": 12,
    "pathPoints": [
      {
        "index": 3,
        "location": [
          127.10739,
          37.342598
        ]
      },
      {
        "index": 4,
        "location": [
          127.108576,
          37.342592
        ]
      },
      ...more data
      {
        "index": 33,
        "location": [
          127.116302,
          37.346454
        ]
      }
    ]
  }
}
```

- 경고가 처음 발행되었을 때 전송되는 메시지 타입입니다. 응급차량 및 예상 경로 정보를 전송해줍니다. 경로 정보는 응급차량의 현재 위치에서 다음 체크포인트까지 포함되어 있습니다.
- 응급차량이 여러 대일 경우 프론트에서 `licenseNumber`와 `emergencyEventId`를 이용해 구분해 처리 합니다.

**[ALERT_UPDATE]**

```json
{
  "code": 200,
  "messageType": "ALERT_UPDATE",
  "data": {
    "licenseNumber": "947Y1201",
    "longitude": 127.109039,
    "latitude": 37.343817
  }
}
```

- 경고를 받은 차량을 대상으로 약 40초간 응급 차량의 위치를 실시간으로 업데이트합니다. 이 정보를 이용해 EPAS 앱에서 응급차의 실시간 위치를 사용자에게 표시합니다.

**[ALERT_END]**

```json
{
  "code": 200,
  "messageType": "ALERT_END",
  "data": {
    "licenseNumber": "947Y1201"
  }
}
```

- 경고 대상자에게 보낼 경고가 종료되면 위의 메시지를 전송합니다.

# 백엔드 서버 스택

|                   Server Stack                   |
| :----------------------------------------------: |
| ![Server Stack](/img/algorithm/server_stack.png) |

백엔드 서버는 다음과 같은 기술 스택을 사용합니다. WAS는 Spring Boot를 사용하고, DB는 PostgrSQL와 Redis를 사용합니다. 또한, 서버 간 통신을 위해 REST API와 Redis Pub/Sub을 사용합니다.

각 서버 컴포넌트는 도커를 사용하여 빌드하고 배포합니다.

## 데이터베이스

|              PostgreSQL ERD               |
| :---------------------------------------: |
| ![PostgreSQL ERD](/img/algorithm/ERD.png) |

핵심 데이터베이스는 PostgreSQL을 사용합니다. 가장 중요한 테이블은 아래와 같습니다.

|   Table Name    |                                                        Description                                                        |
| :-------------: | :-----------------------------------------------------------------------------------------------------------------------: |
|     member      |                                                 사용자 정보를 저장합니다.                                                 |
|     vehicle     |                  사용자 차량 정보를 저장합니다. 차량의 등록 번호, 차량 종류, 차량 번호 등을 저장합니다.                   |
| vehicle_status  | 사용자 차량의 위치, 속도, 방위각 등을 저장합니다. Postgis의 공간 데이터 타입과 인덱싱을 통해 위치 기반 검색을 지원합니다. |
| navigation_path |                                           사용자의 경로 탐색 정보를 저장합니다.                                           |
|   check_point   |                                응급 상황 등록 시 응급차량의 체크포인트 정보를 저장합니다.                                 |
| emergency_event |                          응급 상황 등록 정보를 저장합니다. 응급 상황 관리 및 로깅에 사용됩니다.                           |
|   warn_record   |                                   응급 상황 발생 시 전송된 경고 알림 정보를 저장합니다.                                   |

## OSRM (Open Source Routing Machine)

서비스 특성상 길찾기 API와 도로 네트워크 상에서 매우 많은 연산이 발생합니다. 이로 인해 네이버나 카카오 등 상용 길찾기 API를 사용하기에는 비용이 많이 들어가게 됩니다. 따라서, OSRM을 사용하여 길찾기 서비스를 제공합니다. [OSRM](https://project-osrm.org/)은 OpenStreetMap 데이터를 기반으로 도로 네트워크를 구축하고, 이를 기반으로 길찾기 서비스를 제공합니다.

감사하게도 한국의 도로 데이터를 제공받아 사용할 수 있었고, 이를 기반으로 응급차량 경고 서비스를 제작할 수 있었습니다.

| 사용한 API |             설명              |
| :--------: | :---------------------------: |
|   route    |         경로 탐색 API         |
|   match    | GPS 데이터를 기반 맵 매칭 API |
|   table    |  행렬 경로 탐색 및 계산 API   |

## Spring boot server

스프링 부트로 구성한 WAS는 총 3가지가 있습니다. 각각의 서버는 다음과 같은 역할을 합니다.

|       Server Name       |                                                                             Description                                                                              |
| :---------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  Authentication Server  |                                                                  사용자 인증 및 인가를 담당합니다.                                                                   |
|   Main Service Server   |                                     차량 등록, 길찾기 API, 응급 상황 관리, 경고 발행 대상 계산 등 핵심 서비스 기능을 담당합니다.                                     |
| Vehicle Tracking Server | 차량의 정보를 실시간으로 모니터링합니다. 차량의 위치, 속도, 방위각 등을 실시간으로 업데이트하고, 맵 매칭을 통해 차량의 위치를 도로로 매핑하여 사용자에게 전달합니다. |

### Authentication Server

사용자 인증 및 인가를 담당합니다. 사용자의 로그인, 회원가입, 비밀번호 찾기, 회원 정보 수정 등의 기능을 제공합니다. 또한, 사용자의 인증 정보를 저장하고, 인증된 사용자에게 JWT을 발급하여 인증 및 인가 기능을 제공합니다.

다른 서비스에 접근하기 위해서는 Authentication Server에서 발급받은 JWT를 헤더에 담아 요청을 보내야 합니다. 이를 통해 사용자의 인증 및 인가를 보장합니다.

자세한 내용은 [Authentication Server](https://github.com/Ajou-Soft-19/Spring-JWT-Login-server)를 참고해주세요.

### Main Service Server

차량 등록, 길찾기 API, 응급 상황 관리, 경고 발행 대상 계산 등 핵심 서비스 기능을 담당합니다. 사용자의 요청에 따라 길찾기 API를 호출하고, 응급 상황 발생 시 응급차량의 경로를 계산하여 응급차량에게 전달합니다. 또한, 응급차량의 위치와 네비게이션 경로를 이용해 지속적으로 응급차량을 추적하고, 주변 차량에게 경고 메시지를 전송합니다.

제공하는 핵심 API는 다음과 같습니다.

**길찾기 API**
| API Name | Method | URL | Description |
| :------: | :----: | --- | ----------- |
| route | POST | /api/navi/route | OSRM 기반의 경로를 제공합니다. |
| emergency route | POST | /api/emergency/navi/route | OSRM 기반의 비상 경로를 제공합니다. |
| get path | GET | /api/emergency/navi/path | 저장된 경로를 반환합니다. |
| remove path | POST | /api/emergency/navi/path/remove | 경로를 제거합니다. |

**응급 상황 관리 API**

|         API Name         | Method | URL                           | Description                    |
| :----------------------: | :----: | ----------------------------- | ------------------------------ |
| register emergency event |  POST  | /api/emergency/event/register | 응급 상황을 등록합니다.        |
|   end emergency event    |  POST  | /api/emergency/event/end      | 응급 상황을 종료합니다.        |
|   get emergency event    |  GET   | /api/emergency/event          | 등록된 응급 상황을 반환합니다. |

**경고 대상 계산 API**

```java
    @Bean
    public ChannelTopic alertBroadcast() {
        return new ChannelTopic("alertBroadcast");
    }

    @Bean
    public ChannelTopic updateCurrentPathPoint() {
        return new ChannelTopic("updateCurrentPathPoint");
    }
```

- Redis Pub/Sub을 사용하여 경고 대상 계산 및 경고 메시지 전송을 구현합니다.
- 차량 위치 추적 서버에서 응급차량의 현재 위치를 `updateCurrentPathPoint` 채널로 업데이트받고 경고 대상을 계산합니다. 경고 대상이 되는 차량에게 경고 메시지를 전달하기 위해 `alertBroadcast` 채널을 통해 차량 위치 추적 서버에 경고 메시지를 전달합니다.

### Vehicle Tracking Server

차량의 정보를 실시간으로 모니터링합니다. 차량의 위치, 속도, 방위각 등을 실시간으로 업데이트하고, 맵 매칭을 통해 차량의 위치를 도로로 매핑하여 사용자에게 전달합니다.

소켓 통신을 통해 서버와 메시지를 주고 받습니다.

**소켓 통신 API**

|     API Name     | Method | URL                    | Description                                                         |
| :--------------: | :----: | ---------------------- | ------------------------------------------------------------------- |
|      socket      |  wss   | /ws/my-location        | 소켓 통신을 통해 차량의 위치 정보나 경고 메시지 등을 주고 받습니다. |
| emergency socket |  wss   | /ws/emergency-location | 응급차량을 위한 소켓 통신 API 입니다.                               |

**소켓 통신 클라이언트 메시지 타입**

| Message Type | Description                                    |
| :----------: | ---------------------------------------------- |
|     INIT     | 소켓 연결 시 초기 데이터를 세팅합니다.         |
|    UPDATE    | 차량의 위치, 속도, 방위각 등을 업데이트합니다. |

**소켓 통신 서버 메시지 타입**

| Message Type | Description                                                                                                                          |
| :----------: | ------------------------------------------------------------------------------------------------------------------------------------ |
|   RESPONSE   | 클라이언트가 UPDATE 메시지를 보내면, 서버는 클라이언트에게 RESPONSE 메시지를 보냅니다. RESPONSE에는 맵매칭된 위치 정보를 전송합니다. |
|    ALERT     | 경고 메시지를 전송합니다. 응급차량의 정보가 포함됩니다.                                                                              |
| ALERT_UPDATE | 경고 메시지를 업데이트합니다.                                                                                                        |
|  ALERT_END   | 경고 메시지 종료를 알립니다.                                                                                                         |

# 프론트엔드 스택

프론트엔드는 Flutter를 이용해 구현하였습니다. 내용이 길어져 프론트엔드에 대한 자세한 설명은 [여기](https://github.com/Ajou-Soft-19/service-app)를 참고해주세요.
