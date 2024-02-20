# [EPAS (Emergency vehicle Pre-Alerting System)]

## Project Introduction

- EPAS is a social infrastructure service that provides warning notifications to vehicle drivers so they can be aware of the approach of emergency vehicles in advance.
- The service provides warning notifications by considering the distance, speed, and direction of emergency and surrounding vehicles through a filtering algorithm.
- Through these warning notifications, vehicle drivers can yield the road to emergency vehicles without panic, and emergency vehicles can reach their destinations quickly.

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

## Map Matching Algorithm

We use GPS data to track the location of a vehicle in real time. However, GPS data may differ from the actual location. To correct this, we use a map matching algorithm. The map matching algorithm converts GPS data to a location on the actual road network.

Below are comparison images before and after using map matching.

|                      Before Map Matching                      |                     After Map Matching                      |
| :-----------------------------------------------------------: | :---------------------------------------------------------: |
| ![Before Map Matching](../img/algorithm/before_map_match.jpg) | ![After Map Matching](../img/algorithm/after_map_match.jpg) |

This allows us to accurately determine which checkpoint the emergency vehicle is passing through, which road it is passing through, etc. Using the location and azimuth on the road network obtained here, we select the target for the alert and send the alert message.

## Alerting Target Selection Algorithm

The biggest technical problem we faced was deciding the standard for issuing alert notifications and how to deliver them. If the distance from the emergency vehicle is simply used as the criterion, many unnecessary alerts may be received because the distance from the emergency vehicle is close, even though it is far on the road network, which can degrade the user experience. To solve this problem, we selected the following algorithm.

When an emergency vehicle activates an emergency situation, the backend server continuously monitors the emergency vehicle (approximately every 1 second) and sends alerts to vehicles that are expected to encounter the emergency vehicle.

### **Alerting Target Selection Algorithm Sudo Code**

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

The above code is the pseudocode of the alert target selection algorithm we selected.

The vehicle query range, checkpoint interval, etc. were determined after testing various scenarios through the simulation we created. [Road Network Simulation Tool](https://github.com/Ajou-Soft-19/road-simulator)

**[Vehicle Monitoring]**

|                                                Vehicle Monitoring                                                |
| :--------------------------------------------------------------------------------------------------------------: |
| ![vehicle_status.jpg](https://github.com/Ajou-Soft-19/epas/assets/32717522/d5e4424f-bc26-43f3-95c9-fb3693e7ad1f) |

When a user runs the navigation app, the vehicle tracking server monitors the vehicle's location, speed, and azimuth. At this time, the user's information is stored as anonymous information.

|                                         Emergency Vehicle Navigation Path                                         |                                      Emergency Vehicle Navigation Checkpoint                                      |
| :---------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/addea671-3a1f-421c-ba28-a1c723cf2e50" width="500"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/93a2f2a4-6e6c-4f85-b9f4-1f90fc096e71" width="400"> |

1. The expected driving route of the emergency vehicle is divided into checkpoints at intervals of about 400m, and the next checkpoint that the emergency vehicle is currently heading to is selected as the alert target.

|                                              Alert Target Selection                                               |
| :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/de23df98-adfa-4580-8f65-b9d674ef36d8" width="500"> |

2. Find `vehicles within a 500m radius` of the next checkpoint using the spatial query of `PostGIS`. (The blue circle in the picture above represents a 500m radius.)

3. For vehicles using navigation, an alert message containing the current location and expected route information of the emergency vehicle is sent via a socket. Each navigation application compares the set navigation driving route with the emergency vehicle's route and displays an alert notification to the user if they intersect.

|                                            Alert Message Transmission                                             |
| :---------------------------------------------------------------------------------------------------------------: |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/e7ae7c75-944a-44da-b45d-8e6156a4762d" width="500"> |

4. For vehicles not using navigation, considering the current location and direction of the vehicle, **an alert is issued if it is expected to arrive at the next checkpoint faster than the emergency vehicle or at a similar time**. (Calculated on the road network using OSRM's table api.)
   The red dot represents the emergency vehicle, the blue dot represents the general vehicle, and the black dot represents the vehicle that received the alert.

|                       Filtering Alert Targets Far Away                        |
| :---------------------------------------------------------------------------: |
| ![Filtering Warning Targets Far Away](/img/algorithm/algorithm_example_1.gif) |

Vehicles that are close to the emergency vehicle in a straight line distance but far away on the road network are excluded from the warning targets.

5. In addition, to ensure that alerts are delivered to vehicles very close to the emergency vehicle, an additional alert is issued to `vehicles within 160m of the emergency vehicle` on the road network, regardless of the direction of movement. (The red circle corresponds to this.)

|                                         Alert Screen on Ordinary Vehicle 1                                         | Alert Screen on Ordinary Vehicle 2                                                                                 |
| :----------------------------------------------------------------------------------------------------------------: | ------------------------------------------------------------------------------------------------------------------ |
| <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/cbf3a24f-202b-412e-86ba-0cdc08adc0e5" height="500"> | <img src="https://github.com/Ajou-Soft-19/epas/assets/32717522/d7fcf378-b9f3-4d6a-b787-84ebc3022db5" height="500"> |

The issued alert contains information such as the current location of the emergency vehicle, the license number, and the expected driving route to the next checkpoint. This allows the navigation app to visualize the direction of approach and expected route of the emergency vehicle. The current location of the emergency vehicle is continuously transmitted after receiving the alert for 40 seconds. Users can check and respond to the location of the emergency vehicle in real time based on this information.

### Alert Message Type

Alert messages are sent through a socket connected to a regular vehicle. You can handle the emergency vehicle route through the emergency vehicle's API handler. Below are each message type.

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
      ...
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

- This is the message type sent when an alert is first issued. It sends emergency vehicle and expected route information. Route information includes from the current location of the emergency vehicle to the next checkpoint.
- If there are multiple emergency vehicles, the front distinguishes and processes using `licenseNumber` and `emergencyEventId`.

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

- For vehicles that have received alerts, the location of the emergency vehicle is updated in real time for about 40 seconds. EPAS app displays the real-time location of the emergency vehicle to the user using this information.

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

- When the alert to be sent to the alert target is terminated, the above message is sent.

# Backend Server Stack

|                   Server Stack                   |
| :----------------------------------------------: |
| ![Server Stack](/img/algorithm/server_stack.png) |

The backend server uses the following technology stack. The WAS is Spring Boot, and the DB uses PostgreSQL and Redis. Also, REST API and Redis Pub/Sub are used for communication between servers.

Each server components are build by using Docker and deployed.

## Database

|              PostgreSQL ERD               |
| :---------------------------------------: |
| ![PostgreSQL ERD](/img/algorithm/ERD.png) |

The core database uses PostgreSQL. The most important tables are as follows:

|   Table Name    |                                                                   Description                                                                    |
| :-------------: | :----------------------------------------------------------------------------------------------------------------------------------------------: |
|     member      |                                                             Stores user information.                                                             |
|     vehicle     |                      Stores user vehicle information such as the registration number, type of vehicle, and vehicle number.                       |
| vehicle_status  | Stores the location, speed, and azimuth of the user's vehicle. Supports location-based searches through Postgis' spatial data type and indexing. |
| navigation_path |                                                  Stores information on the user's route search.                                                  |
|   check_point   |                                          Stores checkpoint information when an emergency is registered.                                          |
| emergency_event |                             Stores information on registered emergencies. Used for emergency management and logging.                             |
|  alert_record   |                                       Stores alert notification information sent when an emergency occurs.                                       |

## OSRM (Open Source Routing Machine)

Due to the nature of the service, there are many calculations on the road network and the route search API. As a result, it is expensive to use commercial route search APIs such as Naver or Kakao. Therefore, we provide a route search service using OSRM. [OSRM](https://project-osrm.org/) builds a road network based on OpenStreetMap data and provides a route search service based on it.

Fortunately, we were able to use road data from Korea, and based on this, we were able to create an emergency vehicle alert service.

| Used API |               Description               |
| :------: | :-------------------------------------: |
|  route   |            Route search API             |
|  match   |   Map matching API based on GPS data    |
|  table   | Matrix route search and calculation API |

## Spring boot server

There are a total of 3 WAS configured with Spring Boot. Each server has the following roles:

|       Server Name       |                                                                                                    Description                                                                                                     |
| :---------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  Authentication Server  |                                                                                   Manages user authentication and authorization.                                                                                   |
|   Main Service Server   |                                                  Manages vehicle registration, route search API, emergency management, and calculation of alert issuance targets.                                                  |
| Vehicle Tracking Server | Monitors vehicle information in real time. Updates the location, speed, azimuth, etc. of the vehicle in real time, maps the location of the vehicle to the road through map matching, and delivers it to the user. |

### Authentication Server

Manages user authentication and authorization. It provides features such as login, sign-up, password recovery, and user information modification. Also, it stores user authentication information and provides authentication and authorization features by issuing JWT to authenticated users.

To access other services, you must send a request with the JWT issued by the Authentication Server in the header. This ensures user authentication and authorization.

For more details, please refer to [Authentication Server](https://github.com/Ajou-Soft-19/Spring-JWT-Login-server).

### Main Service Server

Manages vehicle registration, route search API, emergency management, and calculation of alert issuance targets. It calls the route search API according to the user's request, calculates the route of the emergency vehicle when an emergency occurs, and delivers it to the emergency vehicle. Also, it continuously tracks the emergency vehicle using the location of the emergency vehicle and the navigation route, and sends alert messages to nearby vehicles.

The main APIs provided are as follows:

**Route Search API**
| API Name | Method | URL | Description |
| :------: | :----: | --- | ----------- |
| route | POST | /api/navi/route | Provides a route based on OSRM. |
| emergency route | POST | /api/emergency/navi/route | Provides an emergency route based on OSRM. |
| get path | GET | /api/emergency/navi/path | Returns the saved route. |
| remove path | POST | /api/emergency/navi/path/remove | Removes the route. |

**Emergency Management API**

|         API Name         | Method | URL                           | Description                     |
| :----------------------: | :----: | ----------------------------- | ------------------------------- |
| register emergency event |  POST  | /api/emergency/event/register | Registers an emergency.         |
|   end emergency event    |  POST  | /api/emergency/event/end      | Ends an emergency.              |
|   get emergency event    |  GET   | /api/emergency/event          | Returns registered emergencies. |

Here is the English version of the provided description.

**Alert Target Calculation API**

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

- Implements alert target calculation and alert message transmission using Redis Pub/Sub.
- The server receives updates on the current location of the emergency vehicle from the vehicle location tracking server through the `updateCurrentPathPoint` channel and calculates the alert targets. The server sends an alert message to the vehicle location tracking server via the `alertBroadcast` channel to deliver the alert message to the vehicles that become alert targets.

### Vehicle Tracking Server

The server monitors vehicle information in real time. It updates the location, speed, azimuth, etc. of the vehicle in real time, maps the location of the vehicle to the road through map matching, and delivers it to the user.

The server communicates messages with the client through socket communication.

**Socket Communication API**

|     API Name     | Method | URL                    | Description                                                                                      |
| :--------------: | :----: | ---------------------- | ------------------------------------------------------------------------------------------------ |
|      socket      |  wss   | /ws/my-location        | Communicates location information of the vehicle, alert messages, etc. via socket communication. |
| emergency socket |  wss   | /ws/emergency-location | Socket communication API for emergency vehicles.                                                 |

**Client Message Type for Socket Communication**

| Message Type | Description                                                |
| :----------: | ---------------------------------------------------------- |
|     INIT     | Sets initial data when socket connection is made.          |
|    UPDATE    | Updates the location, speed, azimuth, etc. of the vehicle. |

**Server Message Type for Socket Communication**

| Message Type | Description                                                                                                                                      |
| :----------: | ------------------------------------------------------------------------------------------------------------------------------------------------ |
|   RESPONSE   | When the client sends an UPDATE message, the server sends a RESPONSE message to the client. The RESPONSE sends map-matched location information. |
|    ALERT     | Sends an alert message. Includes information about the emergency vehicle.                                                                        |
| ALERT_UPDATE | Updates the alert message.                                                                                                                       |
|  ALERT_END   | Notifies the end of the alert message.                                                                                                           |
