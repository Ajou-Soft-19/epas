# 백엔드 서버 스택

|                   Server Stack                   |
| :----------------------------------------------: |
| ![Server Stack](/img/algorithm/server_stack.png) |

백엔드 서버는 다음과 같은 기술 스택을 사용합니다. WAS는 Spring Boot를 사용하고, DB는 PostgrSQL와 Redis를 사용합니다. 또한, 서버 간 통신을 위해 REST API와 Redis Pub/Sub을 사용합니다.

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
| emergency sockey |  wss   | /ws/emergency-location | 응급차량을 위한 소켓 통신 API 입니다.                               |

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
