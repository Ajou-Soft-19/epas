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
