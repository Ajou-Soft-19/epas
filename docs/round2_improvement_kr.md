# 라운드2 발전 방향

## EPAS Embedded 개발

### 프로젝트 소개

EPAS는 처음에는 네비게이션에 내장하기 위한 플러그인을 목표로 개발되었으나, 네비게이션을 사용하지 않는(또는 사용하기 힘든) 사용자들을 위해 별도의 임베디드 장치로도 구현되었습니다.
EPAS 클라이언트 디바이스는 큰 컴퓨팅 파워가 필요하지 않으며, GPS와 네트워크 통신 기능만 있으면 됩니다. 따라서 아두이노와 같은 저성능 싱글 쓰레드 임베디드 시스템에서도 충분히 동작 가능합니다. 이를 통해 기존의 차량들은 값싼 비용으로 EPAS 시스템을 추가할 수 있고, 스마트폰이나 네비게이션 시스템이 차량에 내장된 경우에는 별도의 하드웨어 추가 없이 소프트웨어 업데이트만으로 EPAS 시스템을 사용할 수 있습니다.

소스코드는 이 링크에서 확인할 수 있습니다: [EPAS-Embeded-Client](https://github.com/Ajou-Soft-19/EPAS-Client/blob/main/embedded/gsc_node_mcu.ino)

|                                                EPAS 회로 구성                                            |             EPAS Client Divice Prototype               |
| :------------------------------------------------------------------------------------------------: | :----------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/db2253ca-8453-4697-8732-e4e98be4027a" alt="EPAS" style="display: block; margin-left: auto; margin-right: auto;" /> | <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/06179948-e7e7-4f62-894e-7a36c9a6e1fe" alt="Additional Information" style="display: block; margin-left: auto; margin-right: auto;" /> |

위 사진은 개발된 EPAS 클라이언트 디바이스의 프로토타입 사진입니다. 이 디바이스에 사용된 하드웨어는 아래와 같습니다.

| 번호 | 이름             | 수량 | 가격 (USD) |
|------|----------------------|------|------------|
| 1    | NodeMCU V3           | 1    | $1.8       |
| 2    | GPS 모듈 (Neo-7M)     | 1    | $5.7       |
| 3    | LCD (16x2)           | 1    | $1.0       |
| 4    | DFPlayer Mini        | 1    | $0.8       |
| 5    | Speaker              | 1    | $1.3       |
| 6    | Micro SD 카드(16GB)   | 1    | $3.0       |

**총 비용 : 약 $13.6**

프로토타입은 개발 및 테스트 목적으로 제작되었으며, 실제 제품은 도매를 통해 더 작고 저렴한 하드웨어를 사용할 수 있고 손바닥 크기 정도의 크기로 제작할 수 있습니다.

### 동작 방식

| EPAS 경고 메시지 출력 |
| :-------------------: |
| <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/4702b382-53fd-4043-834f-7d6f8b6fdb7e" width="700px"> |

- 응급 차량 경고 발행 시 **응급차량의 라이선스 번호**, **응급차량의 접근 방향**과 **주위 응급차량 수**를 출력합니다.

EPAS 클라이언트 디바이스는 GPS 모듈을 통해 자신의 현재 위치를 확인하고 소켓 통신을 통해 [EPAS 차량 모니터링 서버](https://github.com/Ajou-Soft-19/spring-socket-server)에 현재 위치를 전송합니다. EPAS 서버는 이를 기준으로 주위에 응급차량이 지나가는지 확인하고, 응급차량과 마주칠 것으로 예상된다면 클라이언트 디바이스에 경고 메시지를 전송합니다. 클라이언트 디바이스는 이를 받아 LCD에 경고 메시지를 출력하고, 동시에 DFPlayer Mini를 통해 경고음을 출력합니다. 이때 사용자는 응급차량의 라이선스 번호와 응급차량이 접근하고 있는 방향을 LCD 모니터와 스피커를 통해 확인할 수 있습니다.

만약 주위에 복수의 응급차량이 있다면, EPAS 클라이언튼는 차례대로 경고 메시지를 출력하며 사용자에게 알립니다. 사용자는 이를 통해 응급 차량을 미리 인지하고 대응할 수 있습니다.

### 기대 효과

EPAS 클라이언트 디바이스는 다음과 같은 효과를 기대할 수 있습니다.

1. **사용자의 안전 보장**: 응급차량의 접근을 미리 인지하고 피할 수 있어, 사용자의 안전을 보장합니다.
2. **기존 차량에 대한 접근성**: 기존 차량에도 저렴한 비용으로 EPAS 시스템을 사용할 수 있어, 보급성이 높습니다.
3. **사용자 편의성**: 사용자는 응급차량의 접근 방향을 LCD와 스피커를 통해 확인할 수 있어, 주변 상황을 파악하기 쉽습니다.
4. **교통 흐름 개선**: 일반 차량 운전자가 응급차량의 접근을 미리 인지하고 대응할 수 있게 되면, 응급차량을 위한 길을 효율적으로 만들어 줄 수 있습니다. 이는 교통 흐름을 개선하고, 교통 혼잡을 줄이는 데 기여할 수 있습니다.
5. **사회적 인식 개선**: EPAS 시스템의 보급과 사용을 통해 운전자들 사이에 응급차량에 대한 인식이 개선될 수 있습니다. 응급차량에 길을 양보하는 문화가 자연스럽게 정착됨으로써, 사회 전체의 응급 상황 대응 능력이 강화될 수 있습니다.
6. **기술적 확장성**: EPAS 클라이언트 디바이스는 기존의 차량에 쉽게 설치할 수 있는 저렴한 솔루션이지만, 향후 다양한 기술적 확장이 가능합니다. 예를 들어, 인공지능을 활용하여 사용자의 운전 습관을 분석하고, 응급차량의 접근에 대한 더 효과적인 대응 방안을 제시할 수 있습니다.

EPAS 시스템을 사회 기반 시스템에 도입한다면 값싼 비용으로 응급차량의 통행에 큰 도움을 줄 수 있으며, 사용자들의 안전 운행에 기여할 수 있습니다.

## EPAS Social Rewarding System 개발

저희 팀은 EPAS를 더 많은 사람들이 사용하도록 유도하기 위해 EPAS Social Rewarding System을 개발하였습니다. 이를 통해 지역별로 EPAS를 사용하는 사람들을 카운팅할 수 있고, 사용자들의 선의의 경쟁을 유도해 더 많은 사람들이 EPAS를 사용하도록 할 수 있습니다.

| EPAS Social Rewarding System1 | EPAS Social Rewarding System2 | EPAS Social Rewarding System3 |
| :--------------------------: | :--------------------------: | :--------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/4bcb2caa-0a3a-429c-bfb0-6e03fc9b979b"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/459218a8-f8b7-4bc5-83ed-839efae5e41b"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/14191736-ec43-475d-9ef6-666865dd3296"> |

EPAS Social Rewarding System은 응급차량이 응급상황에서 운행 중일 때 주변의 차량에게 알림이 간 기록을 통해 카운팅합니다. 이를 통해 사용자들은 자신의 지역 또는 다른 지역에서 EPAS를 사용하는 사람들이 많다는 것을 알 수 있고, 더 많은 사람들이 EPAS를 사용하도록 유도할 수 있습니다.

EPAS Social Rewarding System도 구글맵 API를 사용하기 때문에 지도 데이터만 추가한다면 전 세계에서 사용할 수 있도록 확장성 있게 개발되었습니다.

소스코드는 [EPAS Social Rewarding System](https://github.com/Ajou-Soft-19/service-web)을 통해 확인할 수 있습니다.

## EPAS API Plugin화

| EPAS API Documentation Example | EPAS API Documentation Example |
| :---------------------: | :---------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/ece5cd5c-d23d-4958-b0a0-94374cef753c"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/81365216-7733-4d71-ad59-c9bad3e8057b">

EPAS를 더 많은 사람들이 사용할 수 있게 하기 위해 EPAS API를 정리하고 사용법을 자세히 설명한 문서를 작성해 개발자들이 쉽게 EPAS를 사용할 수 있게 했습니다. 이를 통해 EPAS 앱뿐만 아니라 다른 어플리케이션에서도 EPAS를 사용할 수 있어 더 많은 사람들이 EPAS 시스템을 사용할 수 있습니다.

### EPAS 일반 사용자 API

EPAS의 일반 사용자 API는 개발자가 쉽게 사용할 수 있도록 제공됩니다. 이를 통해 개발자들은 EPAS의 기능을 쉽게 자신의 어플리케이션에 적용할 수 있습니다. 또한 일반 사용자 API의 경우 사용자의 정보를 수집하지 않고, 익명 사용자의 위치 정보만을 필요로 하기 때문에 쉽게 사용할 수 있습니다.

소켓 통신을 통해 정해진 포맷의 메시지를 주고 받기 때문에 아두이노와 같은 임베디드 시스템에서도 쉽게 사용할 수 있습니다. 개발자는 사용자의 위치 정보를 서버에 전송하기만 하면 되고, 모든 처리는 EPAS 서버에서 이뤄집니다.

EPAS의 일반 사용자 API에 대한 자세한 내용은 [Ordinary User Application API](https://github.com/Ajou-Soft-19/EPAS-Client?tab=readme-ov-file#epas-api-documentation)를 통해 확인할 수 있습니다.

### EPAS 응급차량 API

응급 차량 API의 경우 회원가입, 로그인, 차량 등록, 응급 상황 등록 및 취소 등 EPAS 일반 사용자 API 보다 많은 API 엔드포인트를 필요로 합니다. 이에 대한 자세한 설명을 [Emergency Vehicle User Application API](https://github.com/Ajou-Soft-19/EPAS-Client?tab=readme-ov-file#emergency-vehicle-driver-application-api)에 작성해 두어 개발자들이 쉽게 사용할 수 있도록 하였습니다.

## EPAS 어플리케이션 최적화

| EPAS Mentoring |
| :-------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/b5900fdf-dc4e-4d41-a9a6-5f51c4fd140b" width="700px"> |

구글 멘토링을 통해 EPAS 어플리케이션의 신뢰성을 개선하였습니다. 기존에는 어플리케이션이 백그라운드에서 소켓 통신을 제대로 컨트롤하지 못 해 사용자의 이전 위치 정보가 그대로 유지되는 문제가 있었습니다. 지난 제출에서는 서버에서 주기적으로 더 이상 메시지를 보내지 않는 세션을 종료하도록 수정하였습니다.

이번 제출하서는 문제의 원인을 해결하기 위해 멘토님께 이에 대한 문제를 설명하였고, 멘토님께서는 이에 대한 해결책을 제시해 주셨습니다. 이를 통해 앱이 Dispose 될 때 소켓 연결이 정상적으로 종료되도록 수정하였습니다.
감사합니다 멘토님!
