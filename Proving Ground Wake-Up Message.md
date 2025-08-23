# Proving Ground Wake-Up Message

The drone operator sends this message to the proving ground. This message notifies the proving ground that a drone is ready for testing.

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

This value also corresponds to the `uavid` field in the status message. Please note the `uavid` property in the status message is all lowercase and that's intentional. It's otherwise identical to the `uavID` field found in other messages (including this one).