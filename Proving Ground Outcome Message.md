# Proving Ground Outcome Message

This message communicates the outcome of **all** completed test. 

## MQTT Topic

```
proving_ground/outcome
```

## Example JSON
```json
{
  "uavID": "Polkadot",
  "outcome": "PASS",
  "score": 0.93,
  "message": "remarks about the test"
}
```

```

### Field Descriptions

#### `uavID`
- Type: string
- Description: Identifies a drone that has been sent to the proving ground.

#### `outcome`
- Type: string (enum)
- Description: Could be `PASS` or `FAIL`

#### `score`
- Type: number
- Description: from 0 to 1. 1 is best

#### `message`
- Type: string
- Description: optional

**Notes**:

This value matches a `uavID` that previously made a SADE entry request. The SAM decided that this drone needs to go to the proving ground.

This value also corresponds to the `uavid` field in the status message. Please note the `uavid` property in the status message is all lowercase and that's intentional. It's otherwise identical to the `uavID` field found in other messages (including this one).