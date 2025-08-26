# proving-ground-docs
reference documents for drone proving ground tests, including JSON message formats, MQTT topics, and event flows.

- [Drone Status Message.md](/Drone%20Status%20Message.md)
- [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md)
- [Proving Ground Wake-Up Message.md](Proving%20Ground%20Wake-Up%20Message.md)
- [Proving Ground Outcome Message.md](Proving%20Ground%20Wake-Up%20Message.md)
- [Sade Entry Decision Message.md](Sade%20Entry%20Decision%20Message.md)

# Proving-Ground Use Case Scenario

NOTE: Coordinates of the Proving Ground will be decided ahead of time. We need to know:
- The Proving Ground Origin
- The Approach Point
- The Start Point
- The Boundry of the Proving Ground
- The location of each camera?

The scenario is triggered when a drone is given a "proving_ground" decision by the sade_entry component.
1. Drone flies to SADE entry point and asks for entry. The drone reports a task outcome of `proving_gound`. This informs the choreographer that it needs to send the `wake_up` message to proving ground.
2. The proving ground sends one or more [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md).
3. The **GCS** receives the test message, creates a task that flies to approach point, then into starting point, then it flies the test.
4. The proving ground knows when the test is done. the proving ground sends a another [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md). in this case go back to step 2. Otherwise if there are no more tests the proving ground sends the [Proving Ground Outcome Message.md](Proving%20Ground%20Outcome%20Message.md).
5. The **GCS** receives the [Proving Ground Outcome Message.md](Proving%20Ground%20Outcome%20Message.md). The GCS then send the drone a new task to request entry into the SADE zone.
6. This drone reports the outcome of the SADE entry request using a "custom" task outcome. See [Drone Ready for Task](https://github.com/DroneResponse/Onboarding/blob/main/topics.md#task-drone-ready-for-task) message

