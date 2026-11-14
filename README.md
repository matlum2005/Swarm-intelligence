TRAFFIC SWARM INTELLIGENCE
A Simulation-First Multi-Agent Traffic Signal Optimization Platform
Project Maintainer: Matlum
Project Type: B.Tech Computer Science & Engineering — Final-Year Project
Core Theme: Multi-Agent Systems · Swarm Intelligence · Intelligent Transportation · SUMO Simulation
See It. Simulate It. Optimize It.

Traffic Swarm Intelligence (TSI) is a simulation-first research and engineering project that studies coordinated traffic-signal optimization across a network of connected intersections. The system models intersections as cooperating agents, shares traffic observations through a swarm blackboard / digital pheromone map, and evaluates signal plans using a hybrid Particle Swarm Optimization (PSO) and Ant Colony Optimization (ACO) approach.

The project combines SUMO and TraCI simulation, Python optimization services, a Spring Boot orchestration API, Supabase-hosted PostgreSQL storage, and a Next.js dashboard. It is designed for repeatable experiments and transparent measurement—not for direct control of real-world traffic infrastructure.
Contents
- Project Overview
- Core Capabilities
- System Architecture
- Technology Stack
- Development Phases
- Repository Structure
- Requirements
- Setup on Windows with VS Code
- Run the Services
- Run Experiments
- Network Presets
- OpenStreetMap Integration
- Live Camera Module
- Testing and Verification
- Safety, Privacy and Research Integrity
- Documentation
- Ownership and Use Policy
Project Overview
The baseline network represents a 3 × 3 grid of nine signalized intersections, identified as J1–J9. Each intersection is treated as an agent that observes local traffic conditions and participates in network-level coordination.

The optimization workflow is built around:
1. Collecting traffic-state information from SUMO through TraCI.
2. Publishing observations and neighbour pressure to a shared swarm blackboard.
3. Generating candidate signal decisions using hybrid PSO + ACO logic.
4. Checking candidate decisions through a safety gate before they are applied in simulation.
5. Comparing the swarm controller with baseline controllers using repeatable scenarios, seeds and recorded KPIs.
The repository also includes configurable generated grids, selected OpenStreetMap workflows, an experiment/lab API, dashboard modules and an optional camera-processing service.
Core Capabilities
Area	Capability
Multi-agent network	Nine cooperating junction agents (J1–J9) with configurable neighbour topology
Shared intelligence	Traffic Swarm Blackboard, shared observations, neighbour pressure and digital pheromone state
Optimization	Hybrid PSO + ACO with particle velocity, personal/global best and pheromone reinforcement/evaporation
Safety	Safety gate, bounded green-time decisions, risk-aware checks and conservative fallback behaviour
Objectives	Queue, delay, stops, throughput, emissions and safety-oriented scoring utilities
Routing	Optional pheromone-informed routing and proactive risk-aware routing components
Simulation	SUMO network execution and TraCI-based traffic-state collection
Experiments	Fixed-time baseline, scenario runner, fixed seeds, raw run artifacts and comparison reports
Backend	Spring Boot API for experiment orchestration and PostgreSQL persistence
Dashboard	Next.js / React interface for system status, experiment workflows, maps and result visualization
Live Lab	API for simulation jobs, job controls, recorded frames, events and experiment results
Live Camera	Optional independent service for video ingestion, detection, tracking and traffic analytics


The presence of a module does not by itself establish that it improves real traffic. Performance statements must be supported by completed, reproducible experiments.
System Architecture
                         NEXT.JS / REACT DASHBOARD
                                  :3000
                                     |
                       Website API routes / proxies
                                     |
              +----------------------+----------------------+
              |                      |                      |
              v                      v                      v
       SPRING BOOT API          PYTHON LAB API         OPTIONAL CAMERA API
           :8080                   :8090                    :8091
              |                      |                      |
              |                      v                      v
              |                 SUMO + TraCI          Detection / Tracking
              |                      |                 Traffic Analytics
              |                      v                      |
              |              J1–J9 INTERSECTION AGENTS <----+
              |                      |
              |                      v
              |              SWARM BLACKBOARD
              |                      |
              |                      v
              |               HYBRID PSO + ACO
              |                      |
              |                      v
              |                 SAFETY GATE
              |                      |
              +----------------------+ 
                                     |
                                     v
                         SUPABASE POSTGRESQL
                       Experiment / application data

       STANDALONE OPTIMIZER API: :8001
       Exposes swarm optimization and blackboard endpoints.
The optimizer service and the Lab API are separate processes. The optimizer can be tested independently; a running optimizer endpoint does not automatically mean a SUMO Lab job is running. Start each required service in its own terminal.
Technology Stack
Layer	Technologies
Simulation	Eclipse SUMO, SUMO tools, TraCI
Optimization and services	Python, FastAPI, Uvicorn, Pydantic
Backend	Java 21, Spring Boot 3.5.x, Maven, JDBC
Database	Supabase PostgreSQL
Frontend	Next.js, React, JavaScript, CesiumJS / Leaflet where used
Maps and road data	OpenStreetMap, Overpass API
Optional camera pipeline	OpenCV, PyTorch, Ultralytics/YOLO, ByteTrack, Frigate / go2rtc integrations
Developer tooling	VS Code, Git, npm, pytest, Docker (optional for supported deployments)


Development Phases
1. Foundation and contracts — Python, Java, React, Docker structure and service APIs.
2. SUMO network and traffic-state collection — 3 × 3 signalized grid and TraCI adapters.
3. Fixed-time baseline and KPIs — baseline runner and comparison engine.
4. Hybrid PSO + ACO — particle velocity, personal/global best, pheromone reinforcement and evaporation.
5. Traffic Swarm Blackboard — shared observations, neighbour pressure and pheromone state.
6. Multi-agent coordination — J1–J9 topology and coordinated green constraints.
7. Multi-objective optimization — queue, delay, stops, throughput, emissions and safety-oriented utility.
8. Routing and proactive safety — optional pheromone routing, risk field, safety gate and fallback.
9. Dashboard — service health, experiment workflows, maps, history and KPI visualization.
10. Experiments and reproducibility — scenarios, fixed seeds, raw artifacts, comparison JSON, aggregation and validation tools.
Repository Structure
traffic-swarm-main/
├── backend/                 # Spring Boot orchestration API and PostgreSQL access
├── camera_service/          # Optional camera ingestion and analytics API
├── db/migrations/           # Supabase SQL migrations
├── deploy/live-camera/      # Example Frigate / go2rtc deployment assets
├── docs/                    # Research, implementation and deployment documentation
├── experiments/
│   ├── lab/                 # Simulation Lab API and job management
│   ├── tools/               # Experiment runners, KPIs and evaluation tools
│   └── config/              # Evaluation matrix configuration
├── frontend/                # Next.js / React dashboard
├── optimizer/               # FastAPI hybrid swarm optimizer
├── simulation/              # SUMO networks, scenarios, controllers and runtime
├── tools/                   # Runtime preflight and project integrity utilities
├── FINAL_RELEASE_STATUS.md  # Verification boundary and release status
├── THIRD_PARTY_NOTICES.md   # Third-party components, data and license notices
└── README.md
Requirements
Install the following tools before running the complete stack:
- Python 3.12 recommended for the documented Windows workflow.
- Eclipse SUMO, including sumo and netconvert, plus compatible Python TraCI bindings.
- Java Development Kit (JDK) 21.
- Node.js and npm compatible with the installed Next.js version.
- Git.
- Internet access for package installation and any enabled OSM/Overpass workflow.
- Supabase project credentials for database-backed features.
- Docker is optional and only needed for workflows that use the supplied container configuration.
Maven Wrapper files are included in backend/, so a separate global Maven installation is not normally required to run the backend from the repository.
Setup on Windows with VS Code
1. Clone or extract the repository
If using Git:
git clone <REPOSITORY-URL>
cd traffic-swarm-main
code .
If you downloaded a ZIP, extract it first and open the extracted traffic-swarm-main folder in VS Code. Run commands from this repository root unless a step explicitly changes directory.
2. Create and activate the Python environment
From the repository root:
py -3.12 -m venv .venv
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
Install simulation and optimizer dependencies:
pip install -r simulation\requirements.txt
pip install -r optimizer\requirements.txt
If SUMO is already installed system-wide, keep the TraCI package version compatible with that SUMO installation. Check that sumo, netconvert and traci are discoverable before launching simulations.
3. Configure Supabase and backend database access
The Java backend connects to Supabase PostgreSQL, not to the Supabase REST API. Configure the database connection using the Session Pooler details shown in your Supabase project dashboard.

Set these environment variables in the same PowerShell terminal used to start the backend:
$env:DB_URL = "jdbc:postgresql://YOUR_SESSION_POOLER_HOST:5432/postgres?sslmode=require"
$env:DB_USER = "YOUR_SESSION_POOLER_USERNAME"
$env:DB_PASSWORD = "YOUR_DATABASE_PASSWORD"
Replace the placeholders with the current values from Supabase. Do not paste real credentials into this README, source files, screenshots, Git commits or public issue trackers.

For the Next.js admin integration, configure the server-side Supabase variables and admin settings described in docs/ADMIN_PANEL_DEPLOYMENT.md and .env.example. Keep privileged keys server-side; never expose a service-role/secret key through a NEXT_PUBLIC_ variable. Create the frontend's local environment file as frontend/.env.local and do not commit it.
4. Apply database migrations
In Supabase Dashboard → SQL Editor, execute the migration files in order:
1. db/migrations/001_admin_panel.sql
2. db/migrations/002_site_theme.sql
3. db/migrations/003_view_3d_module.sql
4. db/migrations/004_live_city_section.sql
5. db/migrations/005_live_camera_module.sql — optional; required only for the Live Camera module.
Review each SQL file before execution and back up important data before applying schema changes to a production project. The Java backend's own schema initialization is configured in backend/src/main/resources/application.properties.
Run the Services
Run each service in a separate VS Code terminal. Keep each terminal open while using that service.
A. Optimizer API — port 8001
From the repository root:
cd optimizer
python -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8001
Open the API documentation at http://127.0.0.1:8001/docs and health endpoint at http://127.0.0.1:8001/health.
B. Simulation Lab API — port 8090
Open a new terminal, return to the repository root, activate .venv if needed, and run:
python -m uvicorn experiments.lab.api:app --host 127.0.0.1 --port 8090
Health endpoint: http://127.0.0.1:8090/api/lab/health
API documentation: http://127.0.0.1:8090/docs

This command must be run from the repository root because the experiments package is located there—not from inside optimizer/.
C. Spring Boot backend — port 8080
Open a new terminal, set DB_URL, DB_USER and DB_PASSWORD in that terminal as shown above, then run:
cd backend
.\mvnw.cmd spring-boot:run
Health endpoint: http://127.0.0.1:8080/actuator/health (when the configured actuator endpoint is available).
D. Next.js frontend — port 3000
Open a new terminal:
cd frontend
npm install
npm run dev
Open http://localhost:3000 in your browser. The admin panel is available at /admin when its environment variables and database migrations are configured.
E. Optional Live Camera API — port 8091
Only start this service when configuring the Live Camera module and its dependencies. From the repository root, with the Python environment active:
pip install -r camera_service\requirements.txt
python -m uvicorn camera_service.api:app --host 127.0.0.1 --port 8091
The camera service has separate configuration and optional model/runtime requirements. See camera_service/ and docs/ADMIN_PANEL_DEPLOYMENT.md before enabling it.
Local service summary
Service	Port	Start location
Next.js dashboard	3000	frontend/
Optimizer API	8001	optimizer/
Spring Boot backend	8080	backend/
Simulation Lab API	8090	Repository root
Optional Camera API	8091	Repository root


Run Experiments
Run one reproducible experiment from the repository root:
python experiments/tools/run_experiment.py --scenario normal --seed 42 --root .
Run the configured experiment matrix:
python experiments/tools/run_matrix.py --root .
Run the staged evaluation matrix and inspect its status:
python -m experiments.tools.evaluate_matrix run --stage core --workers 4
python -m experiments.tools.evaluate_matrix status
python -m experiments.tools.analyze_evaluation
Available evaluation stages are core, ablation, emergency and mixed. The evaluation is preliminary until every planned cell for the selected matrix has completed. Reports and run artifacts are written under experiments/.
Network Presets
Network size and timing are configured in simulation/config/networks.json. The configuration includes these presets:
- grid3x3_legacy — original J1–J9 network, retained for regression checks.
- grid3x3 — generated 3 × 3 grid.
- grid5x5 — generated 5 × 5 grid.
- grid7x7 — generated 7 × 7 grid.
- grid9x9 — generated 9 × 9 grid.
- Additional demonstration and India-style presets are defined in the same configuration file.
Build and validate a generated network:
python -m simulation.network.build_network --preset grid9x9
Run an experiment on that network:
python experiments/tools/run_experiment.py --network grid9x9 --scenario normal --seed 42 --root .
SUMO runs headless by default. Add --gui or set TSI_SUMO_GUI=1 when a graphical SUMO window is required. Install the Python simulation dependencies with pip install -r simulation/requirements.txt.
OpenStreetMap Integration
Signal Map
The dashboard's Signal Map (/ghaziabad) can display mapped traffic-signal locations, search places and scan the visible map. Signal locations are based on OpenStreetMap tags such as highway=traffic_signals; the completeness and accuracy of mapped features depend on OpenStreetMap contributors.
Live Lab real-world areas
The Live Lab (/live) can download a selected small-area OSM extract and use it to build a SUMO network. The simulation uses modelled traffic demand. OpenStreetMap does not provide live traffic counts or live signal timings through this workflow. Queues, platoons, fuel and other simulation outputs are generated by the model, not measured from live road traffic.

OpenStreetMap data is © OpenStreetMap contributors and is made available under the Open Database License (ODbL) 1.0. Preserve attribution and comply with the applicable OSM terms. See THIRD_PARTY_NOTICES.md and the OSM copyright page.
Live Camera Module
The optional /live-camera module is implemented as a separate service in camera_service/. Its pipeline includes video ingestion, detection, tracking, traffic analytics, agent integration and safety validation. A camera/model failure is isolated from the Simulation Lab service.

Before enabling it:
1. Review camera_service/requirements.txt and the model/runtime prerequisites.
2. Apply db/migrations/005_live_camera_module.sql after the earlier migrations.
3. Configure the camera service URL and gateway secret on the website and service.
4. Configure the server-side Supabase credentials required by the service.
5. Review THIRD_PARTY_NOTICES.md, especially the Ultralytics licensing terms.
Important operating boundaries:
- Webcams and USB cameras are classified as TEST sources; uploaded or file-backed sources are REPLAY. A LIVE label is an administrator's declaration that a source is a real roadside camera.
- Video access is restricted according to the admin settings. Public previews are disabled by default; review access controls before enabling them.
- Frames remain in memory unless a configured retention option saves snapshots. Follow applicable privacy, consent, retention and data-protection requirements for any real camera feed.
- Without calibration, queue measurements are vehicle counts and speeds are pixel-based estimates. Calibrated metric values remain estimates because a single scale does not remove camera perspective.
- REAL CONTROLLER mode is refused. This project does not send commands to real traffic-signal hardware. Simulation mode applies validated plans only inside SUMO; Shadow mode records recommendations.
- Camera-derived traffic is not automatically the same demand stream as the SUMO scenario. Do not present the demonstration as proof of real-world traffic improvement.
The optional Frigate and go2rtc deployment examples are in deploy/live-camera/. The real end-to-end camera test requires additional services and a road video; see camera_service/tests/test_e2e_real.py.
Testing and Verification
Run the runtime preflight from the repository root:
python tools/runtime_preflight.py
Run Python tests:
python -m pytest -q
python -m pytest optimizer/tests
python -m pytest camera_service/tests
Run frontend checks:
cd frontend
npm install
npm test
npm run build
Run backend tests:
cd backend
.\mvnw.cmd test
The exact verification boundary for this release is documented in FINAL_RELEASE_STATUS.md. SUMO, Maven, Docker, database connectivity and frontend production builds depend on the local machine's installed tools and configuration. Do not describe an environment-dependent check as passed unless it has actually been run successfully on that environment.
Safety, Privacy and Research Integrity
- This is a simulation and research platform, not a certified traffic-management product.
- The system must not be connected to real signal controllers or used to alter live road infrastructure.
- Safety constraints take precedence over optimizer scores. Review the safety gate and controller configuration before experiments.
- Protect Supabase database passwords, service-role/secret keys, admin password hashes and gateway secrets. Never commit local environment files or publish credentials in screenshots, logs or documentation.
- Use lawful, authorized camera sources. Minimize collected data, restrict access, define retention periods and obtain any required permissions before processing identifiable video.
- OpenStreetMap data and third-party software remain subject to their respective licenses and attribution requirements.
- Do not claim a KPI improvement without completed, reproducible SUMO experiments and the corresponding raw artifacts and analysis.
- Literature percentages in a supplied research report are not results produced by this implementation. Report measured results separately and disclose limitations.
Documentation
Document	Purpose
FINAL_RELEASE_STATUS.md	Included features and verification boundary
docs/IMPLEMENTATION_STATUS.md	Implementation history, evaluation progress and known limitations
docs/INTEGRATION_STATUS.md	Integration status
docs/RESEARCH_METHODOLOGY.md	Experiment methodology, metrics and statistical approach
docs/ADMIN_PANEL_DEPLOYMENT.md	Admin panel configuration and deployment
docs/MASTER_UPGRADE_PLAN.md	Upgrade planning reference
THIRD_PARTY_NOTICES.md	Third-party software, data and license notices


Ownership and Use Policy
Project name: Traffic Swarm Intelligence
Project maintainer: Matlum
Copyright: © 2026 Matlum. All rights reserved.

The original project source code, project-specific documentation, architecture descriptions, interface design and other original materials in this repository are proprietary to Matlum, except for components or materials expressly identified as third-party works.

No permission is granted to copy, modify, redistribute, publish, sublicense, sell, commercially exploit or create derivative works from the proprietary project materials without prior written permission from Matlum. Viewing this repository does not grant a license to reuse its original code or project materials.

Third-party packages, libraries, map data, models, assets and referenced works are not owned by Matlum merely because they are used or mentioned in this project. Their original licenses and attribution requirements continue to apply; see THIRD_PARTY_NOTICES.md and the relevant upstream project documentation.

This README and the accompanying ownership notice communicate the intended use policy; they do not technically prevent someone from accessing or cloning a public GitHub repository. To restrict repository access, set the GitHub repository visibility to Private, review collaborator access, and do not publish downloadable source archives. Keep a private backup and retain your project history. For formal legal protection or a tailored proprietary license, consult a qualified legal professional.
Built and maintained by Matlum
Traffic Swarm Intelligence — Simulation-first research into safer, coordinated and measurable traffic-signal optimization.