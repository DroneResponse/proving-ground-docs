# SADE Entry Decision Message

The **SAM** sends this message to the drone to inform it of the entry decision.  
This occurs after the drone requests entry into the SADE zone.

## MQTT topic:
To receive all entry decision messages, you can subscribe to:

```
drone/+/sade_entry_response
```


Individual drones will subscribe to:
```
drone/{uavID}/sade_entry_response
```

## Example JSON

```json
{
    "uavID": "Red",
    "sade_zone_id": "PLACEHOLDER",
    "decision": "PROVING_GROUND",
    "fields": [
        "gps_status",
        "vibration",
        "rc_out",
        "ekf_status",
        "target_attitude"
    ]

}
```

## Field Descriptions

uavID
- Type: string
- Description: Name or identifier of the drone.
- Notes: Must match the {uavID} used in the MQTT topic.

sade_zone_id
- Type: string
- Description: Identifier of the SADE Zone.

decision
- Type: string (enum)
- Allowed Values:
    - `ALLOW` — Drone is permitted entry.
    - `DENY` — Drone is denied entry.
    - `PROVING_GROUND` — Drone must go to the proving ground.

fields
- Type: list of strings
- Description: Specifies optional status fields the drone must now include in its status messages.
- Example Values:
    - gps_status
    - vibration
    - rc_out
    - ekf_status
    - target_attitude
