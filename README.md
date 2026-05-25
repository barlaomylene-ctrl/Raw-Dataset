Alangilan T-intersection SUMO Simulation
=========================================
Capstone Project — BS Electronics Engineering, Batangas State University
"Designing a Machine Learning Based Predictive Traffic Light System"

LOCATION
--------
Alangilan T-intersection, Batangas City
- Highway: Alangilan-Balagtas (west) <-> Alangilan-Kumintang (east)
- Local:   Golden Country Homes (GCH) subdivision exit/entrance (south)

SCENARIO
--------
Morning Peak:  6:00 – 7:00 AM  (simulation time 0–3600 s)
Data source:   Manual Count, Dec 9 2025

Traffic volumes used (morning peak, per hour):
  Highway  Balagtas → Kumintang : ~502 vph
  Highway  Kumintang → Balagtas : ~435 vph
  GCH Exiting → Highway         :  ~54 vph
  Highway Entering → GCH        :  ~88 vph

Vehicle types included:
  Tricycle, Private Car, Van/UV, Jeepney, Mini Bus, Bus,
  Light Truck, Heavy Truck

FILES
-----
  alangilan.nod.xml   — Junction nodes (4 nodes)
  alangilan.edg.xml   — Road edges (6 edges)
  alangilan.con.xml   — Lane connections at intersection
  alangilan.rou.xml   — Vehicle types + routes + demand flows
  alangilan.sumocfg   — SUMO run configuration
  build_network.bat   — Generates alangilan.net.xml via netconvert
  output/             — Simulation output files written here

HOW TO RUN
----------
1. Open a command prompt in this folder (t2test).

2. Build the network (run once):
      build_network.bat

3. Run the simulation (headless):
      sumo -c alangilan.sumocfg

   OR with the GUI:
      sumo-gui -c alangilan.sumocfg

4. Outputs appear in the output\ folder:
      summary.xml   — per-step vehicle counts and waiting times
      tripinfo.xml  — per-trip statistics (waiting time, travel time)
      fcd.xml       — floating car data (position/speed per step, for ML)
      queue.xml     — queue lengths per lane per step

TRACI INTEGRATION
-----------------
To connect Python/TraCI for adaptive signal control:

    import traci
    traci.start(["sumo", "-c", "alangilan.sumocfg"])
    tls_id = "intersection"
    while traci.simulation.getMinExpectedNumber() > 0:
        traci.simulationStep()
        # query detectors, compute phase, set new phase:
        # traci.trafficlight.setPhase(tls_id, new_phase)
    traci.close()

ADDITIONAL SCENARIOS
--------------------
To simulate midday or evening peak, adjust the flow vehsPerHour values
in alangilan.rou.xml using the following source data counts:

  Midday 12:00–1:00 PM:
    Highway Lane 1 (Balagtas): ~454 vph
    Highway Lane 2 (Kumintang): ~658 vph
    GCH Exit: ~48 vph  |  GCH Enter: ~48 vph

  Evening Peak 5:30–6:30 PM:
    Highway Lane 1 (Balagtas): ~1103 vph
    Highway Lane 2 (Kumintang): ~1071 vph
    GCH Exit: ~74 vph  |  GCH Enter: ~60 vph

