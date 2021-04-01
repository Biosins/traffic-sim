# traffic-sim

Streets, a bundle of lanes

    Streets can be one-line, multi-lane, single-direction or bi-direction.
    For simplicity, let's discount lane-changing behaviors and all sources of obstructions:
        Entering of vehicle from a lot into a lane.
        Exiting of a vehicle from a lane into a lot.
        Lane-changing in general.
            Multiple vehicles are allowed to enter one street in one direction, being initially assigned a lane number from 1 to N.
            Upon exiting from the other end of the street, these vehicles can be found to have magically switched lane assignments.
            For simplicity, the simulation will not look into how that happened.
        Lane-passing.
        Parking on the curb, and the occasional slight traffic obstruction it causes.
        Jaywalking pedestrians

Lane, a one-dimensional position on a curve.

    Mathematically speaking, the (traversal) position of a vehicle on a lane is a real number between 0 and x, where x is the physical arc-length of the lane (one stretch of a street).
    While a vehicle is on a lane, it will have an instantaneous velocity, and also an acceleration/deceleration (rate of change of velocity).
        For simplicity, let's discount rotational mechanics, torque, turning radius, or rollover (accident).
        The simulation will treat all lane traversals as strictly linear motion as far as the physics is concerned.

Intersection, a complicated Venn diagram of exclusive zones

    Even with the most simplistic simulation, we must agree that we can't let north-south and east-west traffic flowing at the same time. If it is so, it would not be an at-grade intersection; it would be an overpass or underpass.
    Below we make the typical assumption of a four-way intersection, i.e. having four ingress (incoming) and four egress (leaving) directions.
    4 x 4 makes 16 possible vehicle trajectories. (This includes U-turns.)
    By using pen-and-paper, one can work out the exclusion rules that allow different combinations of these trajectories to be used at different traffic signals.
    A vehicle entering the intersection will have a velocity
    Each trajectory takes a certain amount of time to traverse given a velocity. Work backwards and one can get a fake "length" value.
        It is common knowledge that a vehicle must slow down upon entering an intersection and return to normal speed upon exiting, but for sake of simplicity it will be omitted from simulation.

How do I run a simulation, with many agents, with time, and with many different lengths and velocities?

    Event-based time simulation:
        For each agent, one would forecast the next future timestamp at which one has to re-examine the agent's whereabouts and responses. Store this future timestamp ("event") in a priority queue.
        After forecasting future events from all agents, the agent with the nearest timestamp will be processed. The vehicles surrounding the agent will also have their status updated. If all calculations work out to be correct (no conflicts), the global simulation timestamp is updated, and the next future timestamp will be picked from the queue and processed.
