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

uavID:
- string
- name of the drone
- this should match the `uavID` in the MQTT topic.

sade_zone_id:
- string
- name of the SADE Zone

decition:
- `ALLOW`
- `DENY`
- `PROVING_GROUND`

fields:
- list of string
- list of optional fields to include in the status messages
