# Drone Status Message

The **drone** sends this message periodically to summarize its current state.

## MQTT Topic

```
status_message
```

## Example JSON
```json
{
    "uavid": "Red",
    "status": {
        "status": "ACTIVE",
        "mode": "OFFBOARD",
        "onboard_pilot": "FlyWaypoints",
        "state_type": "Waypoint",
        "speed": 5,
        "location": {
            "latitude": 41.8595181,
            "longitude": -85.9038124,
            "altitude": 235.66229347800862
        },
        "armed": true,
        "battery": {
            "voltage": 15.724000930786133,
            "current": 1,
            "level": 0.85
        },
        "geofence": false,
        "heartbeat_status": "CONTINUE",
        "drone_attitude": {
            "x": 0.18232254884527244,
            "y": -0.21858598706362908,
            "z": -0.6791555939869834,
            "w": -0.6765547709051098
        },
        "drone_heading": 358.76,
        "gimbal_attitude": {
            "x": 0,
            "y": -0.19509032201612825,
            "z": 0,
            "w": 0.9807852804032304
        },
        "gimbal_heading": 0,
        "air_lease_state": "IDLE"
    },
    "timestamp": "2025-08-23T00:05:59.156+00:00"
}
```
### Field Descriptions
#### `uavid`
- Type: string
- Description: Identifier for the drone. Must match the value used in other messages.

#### `status`
The flight controller's MAVLink system status.
- Type: string (enum)
- Possible values: `UNKNOWN`, `BOOT`, `CALIBRATE`, `STANDBY`, `ACTIVE`, `CRITICAL`, `EMERGENCY`, `POWER_OFF`, and `TERMINATE`.

#### `mode`
The current flight controller mode.

- Type: string (enum)
- Allowed Values: `ALT_HOLD`, `LAND`, `LOITER`, `MANUAL_OTHER`, `MISSION`, `OFFBOARD`, `POS_HOLD`, `RTL`, `STABILIZE`, `TAKEOFF`, `UNKNOWN`
- Notes: The underlying flight controllers (PX4 & Ardupilot) may use different names for it's modes, but dr-onboard maps them to the above.

#### `onboard_pilot`

- Type: string
- Description: Current active state in the dr-onboard state machine. This value matches the name of a state specified in the mission.

#### `state_type`
- Type: string
- Description: The current state's `class` used by the onboard system. This is the class that is currently operating the drone.

#### `speed`
- Type: float
- Description: Ground speed (magnitude of horizontal velocity), in meters per second.

#### `location`
Current location of the drone, altitude in meters above the wgs84 ellipsoid

- latitude: float — WGS84 latitude in decimal degrees
- longitude: float — WGS84 longitude in decimal degrees
- altitude: float — Altitude in meters above sea level. Note: the drone senses this value from GPS and uses EGM96 to convert to AMSL from height above the WGS84 ellipsoid)

#### `armed`
- Type: boolean
- Description: true if the drone is armed, false otherwise.

#### `battery`
- Type: object
- Fields:
    - voltage: float — Battery voltage in volts
    - current: float — Current draw in amperes
    - level: float — Charge level between 0 and 1 (1 = full, 0 = empty)
#### `geofence`
- Type: boolean
- Description: true if the flight controller is enforcing a geofence.
- Notes: Always false as of 2024-09-18.
#### `heartbeat_status`
- Type: string (enum)
- Description: Status of the Ground Control Station heartbeat.
- Allowed Values:
    - CONTINUE — Heartbeat is being received, comms are up.
    - HOVER — Heartbeat temporarily lost; drone hovers, waiting for recovery or timeout.
    - RTL — Heartbeat lost beyond threshold; drone returns-to-launch.
#### `drone_attitude`
The attitude of the drone specified as a quaternion. The quaternion describes the rotation of the drone's body frame in the local ENU frame. The quaternion is normalized (it's a unit quaternion).

- Type: object (quaternion)
- Description: Drone attitude (rotation of body frame in local ENU frame).
- Notes: Quaternion is normalized (unit quaternion).
#### `drone_heading`
Specifies the yaw of the drone with respect to the local **East, North, Up frame** in radians. A value of zero points east. The angle is specified with the right hand rule around the **UP axis**.

- Type: float
- Description: Yaw angle (in radians) relative to local **East-North-Up** frame.
- Notes:
    - 0 points east.
    - Right-hand rule around the up axis.

#### `gimbal_attitude`
 quaternion describing the gimbal's status. This attitude is in the Gimbal's Operating Frame
- Type: object (quaternion) or null
- Description: Gimbal attitude in the Gimbal Operating Frame.
#### `gimbal_heading`
- Type: float or null
- Description: The heading of the gimbal. See GH451 for more info.
#### `air_lease_state`
- Type: string
- Description: Current air lease state of the drone
- Possible Values:
    - `IDLE` The drone is in this state when the drone has the air lease it targets
    - `WAITING_RESPONSE` The drone enters this state when the drone is waiting for a message from the GCS that communicates whether or not a recently requested air lease is approved or denied.
    - `LEASE_DENIED` the drone enters this state when the ground station denied it's air lease request. The drone will wait before  asking again.
    - `DISCONNECTED` the drone enters this state when it's connection to the Ground Station is down. You're unlikely to see this state, but it's theoretically, possible that the drones connection to the AWS IoT broker is up while the connection to the ground station is down.

#### `timestamp`
- Type: string (ISO 8601)
- Description: A timestamp in ISO format with milliseconds in the UTC timezone.