## Map Matching Algorithm

We use GPS data to track the location of a vehicle in real time. However, GPS data may differ from the actual location. To correct this, we use a map matching algorithm. The map matching algorithm converts GPS data to a location on the actual road network.

Below are comparison images before and after using map matching.

|                     Before Map Matching                     |                    After Map Matching                     |
| :---------------------------------------------------------: | :-------------------------------------------------------: |
| ![Before Map Matching](/img/algorithm/before_map_match.jpg) | ![After Map Matching](/img/algorithm/after_map_match.jpg) |

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
