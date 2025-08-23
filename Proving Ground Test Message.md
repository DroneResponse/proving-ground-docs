# Proving Ground Test Message

This (draft) document describes the JSON message format used to define a proving ground test. Each JSON message specifies the plan that a drone must follow for evaluation. These messages are published over the AWS MQTT broker. The Ground Station of the drone receives these messages and commands the drone accordingly.

## Topic:

These messages are sent to this topic:
```
proving_ground/test/{uavID}
```

NOTE: `{uavID}` is a placeholder. The name or identifier for the drone under test would replace that. For example, for the Red drone, the topic would be `proving_ground/test/Red`

## Example JSON Message

```json
{
    "uavID": "Red",
    "test_name": "Wind Test",
    "test_uuid": "c0a28d2c-c143-45e2-8e03-427d1d9516c8",
    "origin": {
        "latitude": 41.60667079046232,
        "longitude": -86.35600414023853,
        "altitude_amsl": 228.5,
        "relative_altitude": 0
    },
    "approach_point": {
        "latitude": 41.60667567201603,
        "longitude": -86.35583770543897,
        "altitude_amsl": 233.5,
        "relative_altitude": 5
    },
    "start_point": {
        "latitude": 41.60667079046232,
        "longitude": -86.35600414023853,
        "altitude_amsl": 233.5,
        "relative_altitude": 5
    },
    "waypoints": [
        {
            "latitude": 41.60667079046232,
            "longitude": -86.35600414023853,
            "altitude_amsl": 233.5,
            "relative_altitude": 5,
            "hover_time": 0
        },
        {
            "latitude": 41.60669026712962,
            "longitude": -86.35597393539116,
            "altitude_amsl": 233.5,
            "relative_altitude": 5,
            "hover_time": 5
        },
        {
            "latitude": 41.60667079046232,
            "longitude": -86.35600414023853,
            "altitude_amsl": 233.5,
            "relative_altitude": 5,
            "hover_time": 0
        }
    ]
}
```

---

## Field Documentation

### `uavID`

- **Type**: String
- **Description**: A name or identifier for the drone under test. This may be a human-readable label (e.g., `Red`) or a formal identifier such as the FAA registration number.
- **Purpose**: Links the test plan to a specific aircraft. Useful for test logs, correlation, and reporting.

### `test_name`

- **Type**: String
- **Description**: The name or type of test being performed. For example, `Wind Test`, `Battery Test`.
- **Purpose**: Human readable name for the test. The goal is to identify which test procedure is being carried out. Useful for operators, drones, and logs.

### `test_uuid`

- **Type**: String 
- **Format**: Canonical 36-character form `8-4-4-4-12` hex with hyphens, e.g., `c0a28d2c-c143-45e2-8e03-427d1d9516c8`.
- **Purpose**: Globally unique ID for the specific test. Gives us a way to reference a specific test later.
- **Generation**: The proving ground  should generate this once per test. Nobody should ever modify it.
- **Example (Python)**:
  ```python
  import uuid

  def make_test_uuid():
      test_uuid = uuid.uuid4()
      return str(test_uuid)
  ```

### `origin`

- **Type**: LLA Object
- **Description**: The reference location for the **proving ground**. Serves as the baseline for relative altitude calculations.
- **Fields**:
  - `latitude` (float): Latitude in decimal degrees (WGS84).
  - `longitude` (float): Longitude in decimal degrees (WGS84).
  - `altitude_amsl` (float): Absolute altitude above mean sea level, in meters.
  - `relative_altitude` (float): always `0.0` 🙃

### `approach_point`

- **Type**: LLA Object
- **Description**: The designated location where the drone must approach before entering the proving ground airspace. The drone must enter the proving ground from a known safe location.
- **Fields**:
  - `latitude` (float): Latitude in decimal degrees.
  - `longitude` (float): Longitude in decimal degrees.
  - `altitude_amsl` (float): Altitude above mean sea level, in meters.
  - `relative_altitude` (float): Altitude relative to the `origin` altitude, in meters.

### `start_point`

- **Type**: LLA Object
- **Description**: The drone’s entry location into the proving ground test corridor. The drone must fly to this location from the `approach_point` when entering the prooving ground.
- **Fields**:
  - `latitude` (float): Latitude in decimal degrees.
  - `longitude` (float): Longitude in decimal degrees.
  - `altitude_amsl` (float): Altitude above mean sea level, in meters.
  - `relative_altitude` (float): Altitude relative to the `origin` altitude, in meters.

### `waypoints`

- **Type**: Array of LLA Objects
- **Description**: The ordered sequence of points that the drone must navigate during the test..
- **Fields** (per waypoint):
  - `latitude` (float): Latitude in decimal degrees.
  - `longitude` (float): Longitude in decimal degrees.
  - `altitude_amsl` (float): Absolute altitude above mean sea level, in meters.
  - `relative_altitude` (float): Altitude relative to the `origin` altitude, in meters.
  - `hover_time` (float): Duration in seconds to hover at this waypoint before moving to the next.

---

## How to enter the proving ground

The drones must:
1. Navigate to the `approach_point` 
2. Proceed to the  `start_point`, then
3. Execute `waypoints` in listed order.
    - Each waypoint will specify `hover_time` (seconds). The vehicle should maintain the waypoint position for the specified duration before continuing.

---

## LLA Object (Latitude/Longitude/Altitude)

A reusable object type used by `origin`, `approach_point`, `start_point`, and each entry in `waypoints`.

**Fields**

- `latitude` (float, required): Decimal degrees in WGS84. Range **[-90, 90]**. Positive = North.
- `longitude` (float, required): Decimal degrees in WGS84. Range **[-180, 180]**. Positive = East.
- `altitude_amsl` (float, optional): Altitude **Above Mean Sea Level** in meters.
- `relative_altitude` (float, optional): Altitude relative to `origin.altitude_amsl` in meters. Positive values indicate meters **above origin**.


