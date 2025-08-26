# proving-ground-docs
reference documents for drone proving ground tests, including JSON message formats, MQTT topics, and event flows.

- [Drone Status Message.md](/Drone%20Status%20Message.md)
- [Proving Ground Test Message.md](Proving%20Ground%20Test%20Message.md)
- [Proving Ground Wake-Up Message.md](Proving%20Ground%20Wake-Up%20Message.md)
- [Sade Entry Decision Message.md](Sade%20Entry%20Decision%20Message.md)

# Proving-Ground Use Case Scenario
The scenario is triggered when a drone is given a "proving_ground" decision by the sade_entry component.
1. The **GCS** publishes a wake-up call on *proving_ground/wake_up* carrying the uav_id (e.g., PolkaDot).
2. The **GCS** subscribes to *drone/{uavID}/sade_entry_response*
3. The **proving ground** receives the wake_up call, subscribes to *drone/starts waiting for *drone/{uavID}/ready*. This implies that the drone has reached the starting point of the proving ground test. [COORDINATES WILL BE ADDED HERE FOR THE NASA DEMO].
4. Upon receipt of a *drone/PolkaDot/ready* message, the **proving ground** assigns its first task to the drone using *proving_ground/test/{uavID}* 
5. The **GCS** receives the test message and translates it into flight instructions for the drone (this part is black-box to the proving ground).
6. Upon completion of the test, the **DR-Onboard** publishes a *drone/{uavID}/ready* message and  Steps 3 - 5 are repeated as long as the proving ground has more tests.
7. When the **proving ground** has no more tests to be run it issues a *drone/{uavID}/sade_entry_response* message bearing a decision. This decision can only be *admit* or *deny*.
8. The **GCS** receives this message.  If the message is admit the GCS generates a flight that takes is safely out of the proving ground (for safety it will ascend above the height of the cameras / or along a straight path out? and then receive a flight path back into the SADE zone).  Similarly a deny decision will result in a safe path out of the proving ground and then a path home to land.
