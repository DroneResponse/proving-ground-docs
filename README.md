# proving-ground-docs
reference documents for drone proving ground tests, including JSON message formats, MQTT topics, and event flows.

- [Drone Status Message.md](/Drone%20Status%20Message.md)
- [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md)
- [Proving Ground Wake-Up Message.md](Proving%20Ground%20Wake-Up%20Message.md)
- [Proving Ground Outcome Message.md](Proving%20Ground%20Outcome%20Message.md)
- [Sade Entry Decision Message.md](Sade%20Entry%20Decision%20Message.md)

## Proving-Ground Use Case Scenario

NOTE: Coordinates of the Proving Ground will be decided ahead of time. We need to know:
- The Proving Ground Origin
- The Approach Point
- The Start Point
- The Boundry of the Proving Ground
- The location of each camera?

The scenario is triggered when a drone is given a "proving_ground" decision by the sade_entry component.
1. The Drone asks for entry into the SADE zone. The SADE authorization manager decides that the drone may not enter the SADE zone unless it demonstrates its capability by flying some tests in the `proving_ground`.
2. The drone flies to the proving ground _Approach Point_.
3. Once the drone is hovering at the _Approach Point_, the drone (or its operator or its GCS) send the `wake_up` message to the proving ground. The drone sends a wake_up message to signal it's ready to receive the first test. (See [Proving Ground Wake-Up Message.md](Proving%20Ground%20Wake-Up%20Message.md))
4. The proving ground responds to the wake_up message by sending _one_ [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md).
5. The **Drone** (or its operator or its GCS) receive the test message. The receiver creates a task that flies to the starting point, before flying the test.
6. When the test is complete, the **drone sends another wake_up message** to signal it's ready for the next instruction.
7. The proving ground responds to the wake_up message with either:
   - Another [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md) if there are more tests to run, OR
   - A [Proving Ground Outcome Message.md](Proving%20Ground%20Outcome%20Message.md) if testing is complete (either all tests passed/failed, or testing is being stopped early due to failure)
8. If a test message was sent, return to step 5. If an outcome message was sent, continue to step 9.
9. The drone (or its operator or its GCS) receives the [Proving Ground Outcome Message.md](Proving%20Ground%20Outcome%20Message.md). The drone/gcs/operator then request entry into the SADE zone.
10. The drone/gcs/operator receives the an entry decision from the SADE authorization manager. The drone proceeds to either:
   - Fly a mission that goes through the SADE zone (in case the SADE authorization manager grants entry)
   - Fly home and land or fly a mission that stays outside the SADE zone (in case the SADE authorization manager denies entry into the SADE zone)

## Proving Ground Example Code

https://github.com/DroneResponse/ProvingGround
