# 라운드2 발전 방향

- [라운드2 발전 방향](#라운드2-발전-방향)
  - [EPAS Embedded 개발](#epas-embedded-개발)
  - [EPAS Social Rewarding System 개발](#epas-social-rewarding-system-개발)
  - [EPAS API Plugin화](#epas-api-plugin화)
    - [EPAS 일반 사용자 API](#epas-일반-사용자-api)
    - [EPAS 응급차량 API](#epas-응급차량-api)
  - [EPAS 어플리케이션 최적화](#epas-어플리케이션-최적화)

## EPAS Embedded 개발

저희 팀은 EPAS를 더 많은 사람들이 사용할 수 있도록 EPAS Embedded를 개발하였습니다. 이를 통해 네비게이션이 없거나 네비게이션을 사용할 수 없는 사람들도 저렴한 비용으로 EPAS를 사용할 수 있게 되었습니다. EPAS Embedded는 아두이노와 GPS 모듈을 사용하여 개발되었습니다.

|                                          EPAS Implementaion                                            |             EPAS Client Divice Prototype               |
| :------------------------------------------------------------------------------------------------: | :----------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/db2253ca-8453-4697-8732-e4e98be4027a" alt="EPAS" style="display: block; margin-left: auto; margin-right: auto;" /> | <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/06179948-e7e7-4f62-894e-7a36c9a6e1fe" alt="Additional Information" style="display: block; margin-left: auto; margin-right: auto;" /> |

| EPAS Warning Message |
| :-------------------: |
| <img src="https://github.com/Ajou-Soft-19/EPAS-Embeded-Client/assets/32717522/4702b382-53fd-4043-834f-7d6f8b6fdb7e" width="700px"> |

EPAS Embedded는 아래와 같은 기능을 제공합니다.

1. 사용자의 위치를 실시간으로 서버에 전송
2. 사용자 주변에 응급차량이 운행 중일 때 음성 및 텍스트 경고 메시지 전송(응급차량의 차량번호, 방향 제공)

자세한 내용은 [EPAS Embedded](https://github.com/Ajou-Soft-19/EPAS-Client?tab=readme-ov-file#epas-embeded-client)를 통해 확인할 수 있습니다.

## EPAS Social Rewarding System 개발

저희 팀은 EPAS를 더 많은 사람들이 사용하도록 유도하기 위해 EPAS Social Rewarding System을 개발하였습니다. 이를 통해 지역별로 EPAS를 사용하는 사람들을 카운팅할 수 있고, 사용자들의 선의의 경쟁을 유도해 더 많은 사람들이 EPAS를 사용하도록 할 수 있습니다.

| EPAS Social Rewarding System1 | EPAS Social Rewarding System2 | EPAS Social Rewarding System3 |
| :--------------------------: | :--------------------------: | :--------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/4bcb2caa-0a3a-429c-bfb0-6e03fc9b979b"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/e65a5640-825e-4e7d-b469-c95d80eb1d67"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/91e7e258-4da6-4056-bf65-490fa58b16ab"> |

EPAS Social Rewarding System은 응급차량이 응급상황에서 운행 중일 때 주변의 차량에게 알림이 간 기록을 통해 카운팅합니다. 이를 통해 사용자들은 자신의 지역에서 EPAS를 사용하는 사람들이 많다는 것을 알 수 있고, 더 많은 사람들이 EPAS를 사용하도록 유도할 수 있습니다.

자세한 내용은 [EPAS Social Rewarding System](https://github.com/Ajou-Soft-19/service-web)을 통해 확인할 수 있습니다.

## EPAS API Plugin화

| EPAS API Documentation Example | EPAS API Documentation Example |
| :---------------------: | :---------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/ece5cd5c-d23d-4958-b0a0-94374cef753c"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/81365216-7733-4d71-ad59-c9bad3e8057b">

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
