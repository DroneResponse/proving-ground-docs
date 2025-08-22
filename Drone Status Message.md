# Drone Status Message


## MQTT Topic

```
status_message
```

## Example JSON
```json
{
    "uavid": "Red1",
    "status": {
        "status": "STANDBY",
        "mode": "LOITER",
        "onboard_pilot": "ReceiveMission",
        "state_type": "ReceiveMission",
        "speed": 0.01,
        "location": {
            "latitude": 41.8593642,
            "longitude": -85.9038305,
            "altitude": 228.71429552028434
        },
        "armed": false,
        "battery": {
            "voltage": 16.200000762939453,
            "current": 1,
            "level": 1
        },
        "geofence": false,
        "heartbeat_status": "CONTINUE",
        "drone_attitude": {
            "x": 0.004123492900346553,
            "y": -0.009503374793724081,
            "z": 0.06074064722789972,
            "w": -0.9980998765143123
        },
        "drone_heading": 96.97,
        "gimbal_attitude": null,
        "gimbal_heading": null,
        "air_lease_state": "IDLE"
    },
    "timestamp": "2025-08-22T23:53:40.984+00:00"
}
```
### Field Descriptions

This message summarizes the drones current state. The drone sends this message periodically. The message structure is identical to the "new_drone" message. The fields in the status message are as follows:

- `status`: The flight controller's MAVLink system status. Possible values include UNKNOWN, BOOT, CALIBRATE, STANDBY, ACTIVE, CRITICAL, EMERGENCY, POWER_OFF, and TERMINATE.
- `mode`: Flight controller mode. Possible values inlcude ALT_HOLD, LAND, LOITER, MANUAL_OTHER, MISSION, OFFBOARD, POS_HOLD, RTL, STABILIZE, TAKEOFF, and UNKNOWN. The underlying flight controller may have different names for these modes, but dr-onboard maps them to the ones listed here.
- `onboard_pilot`: **Current active state within the dr-onboard state machine**
- `speed`: Ground speed of the drone (the magnitude of the horizontal velocity) in meters per second
- `location`: Current location of the drone, altitude in meters above the wgs84 ellipsoid
- `armed`: True if the drone is armed, False otherwise
- `battery`: **Current battery status. voltage in volts, current in amperes, and level is a number between 0 and 1 where 1 is fully charged and 0 is empty**
- `geofence`: true if the flight controller is enforcing a geofence, false otherwise. Note this field is always false as of 2024-09-18
- `heartbeat_status`: Status of the heartbeat. CONTINUE if the heartbeat is being received and comms are up, RTL if the heartbeat has been lost and the drone needs to abort. HOVER if the heartbeat has been temporarily lost. In the HOVER state the drone will wait for the heartbeats to resume or it will enter the RTL if the heartbeat does not return after a timer.
- `drone_attitude`: The attitude of the drone specified as a quaternion. The quaternion describes the rotation of the drone's body frame in the local ENU frame. The quaternion is normalized (it's a unit quaternion).
- `drone_heading`: Specifies the yaw of the drone with respect to the local East, North, Up frame in radians. A value of zero points east. The angle is specified with the right hand rule around the up axis.
- `gimbal_attitude` A quaternion describing the gimbal's status. This attitude is in the Gimbal's Operating Frame
- `gimbal_heading`: The heading of the gimbal. See GH451 for more info.
- `timestamp`: A timestamp in ISO format with milliseconds in the UTC timezone.