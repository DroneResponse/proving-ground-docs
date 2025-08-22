# Proving Ground Wake-Up Message

The **SAM** sends this message to the proving ground to notify it that a drone has been directed there for testing.

This message tells the proving ground which drone to expect.

## MQTT Topic

```
proving_ground/wake_up
```

## Example JSON
```json
{
    "uavID": "Red"
}
```

### Field Descriptions

#### `uavID`
- Type: string
- Description: Identifies a drone that has been sent to the proving ground.

**Notes**:

This value matches a `uavID` that previously made a SADE entry request. The SAM decided that this drone needs to go to the proving ground.

This value also corrosponds to the `uavid` field in the status message.