# AI Smart Irrigation System with Reinforcement Learning

This is my current project where I am trying to build an AI-based smart irrigation system that can decide **when** and **how much** to irrigate crops.

The main idea is to use reinforcement learning to train an agent inside a crop-water simulation first, then gradually move the logic toward a real IoT-based irrigation setup using sensors and a microcontroller.

I started this project because water usage in agriculture is a real problem, especially in regions where irrigation decisions are still often based on fixed schedules, experience, or available water limits rather than real-time soil and crop conditions. I wanted to understand whether AI could help make irrigation more adaptive and reduce unnecessary water and electricity use.

This project is still under active development. It is not a finished product yet, but I am using it to learn reinforcement learning, crop simulation, embedded systems, and how AI can be applied to agriculture.

---

## Project Goal

The goal of this project is to create a system that can recommend or control irrigation based on data such as:

* soil moisture
* soil temperature
* air temperature
* humidity
* light intensity
* crop growth stage
* previous irrigation amount
* simulated crop-water stress

Instead of using a simple rule like “turn on the pump when soil moisture is below X,” I want the system to learn better irrigation strategies over time.

The long-term goal is to build a small MVP that can be tested with real farms or greenhouses and compared against normal irrigation methods.

---

## Why Reinforcement Learning?

I chose reinforcement learning because irrigation is not just a one-time prediction problem.

The system needs to make decisions day by day. If it gives too much water today, that can waste water and electricity. If it gives too little water, the crop may become stressed and yield can decrease later.

So the problem fits reinforcement learning quite naturally:

* the environment is the crop/soil/weather system
* the agent chooses irrigation actions
* the reward depends on crop health, yield, and water use
* the agent improves by testing different strategies over time

At this stage, I am using reinforcement learning mainly as a way to learn how irrigation decisions can be optimized inside a simulated environment before trying anything on real hardware.

---

## Main Idea

The project has two connected parts:

### 1. Simulation

I use AquaCrop / crop-water simulation as a virtual farm.

This lets me test irrigation decisions without needing a real field immediately. The RL agent can interact with the simulation and learn from the results.

In the simulation, the agent receives information about the current crop and soil condition, chooses an irrigation amount, and then gets feedback based on how good or bad that decision was.

### 2. IoT / Real-World Direction

The real-world version would use sensors connected to a microcontroller such as an ESP32.

The sensor data would be used as input for the model, and the output would eventually become an irrigation recommendation or pump control signal.

For example:

```text
Soil sensor / weather data
        ↓
AI decision model
        ↓
Irrigation amount recommendation
        ↓
Pump / valve control
```

Right now, I am focusing more on the simulation and RL part, but the hardware direction is part of the final goal.

---

## Current Technical Approach

The project currently focuses on building a custom reinforcement learning environment around irrigation.

The agent observes the current state of the simulated crop/soil system and chooses how much water to apply.

Example action idea:

```text
0 mm
5 mm
10 mm
15 mm
20 mm
25 mm
30 mm
```

Later, these water amounts can be converted into pump running time, for example:

```text
irrigation amount → pump seconds
```

The reward function is one of the most important parts of the project. I am experimenting with a reward that encourages the agent to:

* keep the crop healthy
* avoid water stress
* avoid over-irrigation
* reduce unnecessary water use
* achieve good yield at the end of the season

A simplified version of the reward idea is:

```text
reward = crop_growth_or_yield_score - water_use_penalty - stress_penalty
```

The exact reward design is still being improved because this is one of the hardest parts of the project.

---

## Technologies Used

* Python
* Reinforcement Learning
* Gymnasium / custom RL environment
* Stable-Baselines3
* PPO
* AquaCrop / AquaCrop-OSPy
* NumPy
* Pandas
* Matplotlib
* ESP32 / ESP32-S3
* Soil moisture sensors
* TinyML direction for future deployment

---

## What I Have Worked On So Far

So far, I have been working on:

* understanding how AquaCrop simulates crop growth and water balance
* extracting useful values such as soil water content from the simulation
* designing a custom RL environment for irrigation
* testing how an agent can choose irrigation amounts
* learning how PPO works in practice
* thinking about how simulated soil moisture can match real sensor values
* planning how sensor data could later be used as the state input
* researching how TinyML could help run models on low-power devices
* talking to potential users to understand real irrigation problems

This project is not only about writing code. A big part of it is understanding the actual problem: how farmers decide irrigation now, what data they use, what problems they face, and whether a tool like this would actually be useful.

---

## Early Customer Discovery

I also started talking to people from the agriculture/water sector to understand the problem better.

From early conversations, I learned that irrigation is often decided based on fixed periods, available water limits, and experience. In many cases, farmers do not use soil sensors or smart irrigation tools. Overwatering and underwatering can still happen because decisions are not always based on real-time crop or soil data.

This made me more interested in building something practical, not just a simulation.

The next step is to interview more farmers, greenhouse operators, and water-sector professionals to better understand:

* how much water is used
* how much electricity is used for pumping
* how irrigation decisions are made
* what problems happen most often
* whether they would be open to testing a simple MVP

---

## Planned MVP

The first MVP does not need to be a perfect automatic irrigation system.

My goal is to build something testable, such as:

* a simple dashboard or notebook showing irrigation recommendations
* a simulation demo where the RL agent is compared with fixed irrigation
* sensor input connected to an ESP32
* a basic recommendation flow: soil condition → suggested irrigation amount
* a way to estimate water and electricity savings

A possible MVP flow:

```text
Sensor or simulated data
        ↓
Model / RL policy
        ↓
Recommended irrigation amount
        ↓
Water use tracking
        ↓
Comparison with normal irrigation
```

The main thing I want to prove first is whether the system can make more efficient irrigation decisions than a simple fixed schedule.

---

## Project Architecture

Planned structure:

```text
smart-irrigation-rl/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── aquacrop_experiments.ipynb
│   ├── rl_training.ipynb
│   └── results_visualization.ipynb
│
├── src/
│   ├── environment/
│   │   └── irrigation_env.py
│   │
│   ├── training/
│   │   └── train_agent.py
│   │
│   ├── evaluation/
│   │   └── evaluate_agent.py
│   │
│   └── utils/
│       └── data_processing.py
│
├── hardware/
│   ├── esp32_sensor_test/
│   └── pump_control_test/
│
├── references/
│   └── README.md
│
└── docs/
    ├── project_log.md
    ├── customer_interviews.md
    └── roadmap.md
```

The structure may change as the project develops.

---

## Reinforcement Learning Environment

The RL environment is designed around a simple loop:

1. Get the current crop/soil state.
2. Agent chooses an irrigation action.
3. Apply the irrigation amount in the simulation.
4. Move the simulation forward.
5. Calculate reward.
6. Repeat until the end of the season.

Simplified version:

```text
state → action → simulation step → reward → next state
```

Possible state values:

```text
soil moisture
temperature
humidity
light
day of season
previous irrigation
crop water stress
```

Possible actions:

```text
choose irrigation amount in millimeters
```

Possible reward factors:

```text
+ crop growth / yield
- excessive water use
- water stress
- inefficient irrigation
```

---

## Hardware Direction

For the physical prototype, I am planning to use:

* ESP32 or ESP32-S3
* soil moisture sensor
* soil temperature sensor
* air temperature and humidity sensor
* light sensor
* relay module
* water pump or valve
* optional RS485 soil sensor later

The first hardware version can be much simpler than the final idea. It can start with just reading sensor values and sending them to a notebook, dashboard, or simple model.

Later, the system can move toward edge deployment with TinyML, where a smaller model runs directly on the microcontroller.

---

## TinyML Direction

One of the future goals is to make the system work without depending fully on the cloud.

This matters because farms may not always have stable internet. A small model running on a low-power device could make the system cheaper and more practical.

TinyML could help with:

* local sensor data processing
* simple irrigation recommendations
* low-power operation
* faster decisions
* less dependence on internet connection

This part is still future work, but it is part of the project direction.

---

## Current Status

Current stage:

```text
Research + simulation + early prototype development
```

Completed / in progress:

* learning reinforcement learning basics
* testing PPO on simpler environments
* working with AquaCrop simulation
* studying soil moisture values inside AquaCrop
* designing the RL irrigation environment
* planning the ESP32 sensor setup
* starting customer interviews
* preparing the project for GitHub documentation

Not completed yet:

* final trained RL model
* full real-world sensor integration
* pump control with trained policy
* real farm pilot
* measured water/electricity savings
* final MVP dashboard

I am keeping this section honest because the project is still being built.

---

## Roadmap

### Stage 1 — Simulation

* Build a working AquaCrop-based irrigation environment
* Extract useful state values from the simulation
* Define action space for irrigation amount
* Design reward function
* Train PPO agent
* Compare RL agent with fixed irrigation schedule

### Stage 2 — Better Evaluation

* Track total water use
* Track crop yield
* Track crop stress
* Plot irrigation decisions over time
* Compare water savings and yield trade-offs

### Stage 3 — Sensor Prototype

* Connect ESP32 with soil moisture sensor
* Read real sensor values
* Calibrate sensor values
* Compare real sensor readings with simulated soil moisture
* Store data for testing

### Stage 4 — MVP

* Build a simple dashboard or notebook demo
* Show recommended irrigation amount
* Estimate water and electricity savings
* Test with greenhouse/farm data if possible

### Stage 5 — Pilot Direction

* Interview more farmers and water-sector professionals
* Find a small test location
* Compare normal irrigation vs AI-assisted recommendations
* Measure actual water use and electricity use

---

## What I Am Learning

Through this project, I am learning:

* how reinforcement learning works beyond basic examples
* how to create a custom Gymnasium environment
* how crop-water models simulate irrigation and stress
* how to design reward functions
* how sensor data can be used in AI systems
* how embedded systems can connect AI with the physical world
* how hard it is to turn a model into something useful for real users

This project is also helping me understand that building AI for real-world problems is not only about training a model. It also requires understanding users, hardware limitations, data quality, cost, and whether the solution can actually be tested.

---

## References and Credits

Some files, examples, and ideas in this repository may be based on public tutorials, documentation, research papers, and open-source GitHub repositories that helped me understand reinforcement learning, AquaCrop, Gymnasium environments, and IoT systems.

I will keep references and credits in the `references/` folder or in this section as the project grows.

Important note: if I use code or files from another GitHub repository, I will credit the original author and respect the license of that repository.

Reference format I plan to use:

```text
Original repository:
Author:
What I used it for:
Changes I made:
License:
Link:
```

---

## Disclaimer

This project is experimental and educational.

It is not ready to control real irrigation systems without testing, calibration, and safety checks. Any real-world irrigation decision should be verified carefully, especially because wrong irrigation can damage crops or waste water.

The current goal is to build a testable prototype and learn how reinforcement learning can be applied to smart irrigation.

---

## Future Vision

My bigger goal is to build a low-cost AI irrigation assistant for farms and greenhouses.

The system should help answer a simple but important question:

```text
How much should I irrigate today?
```

If this project works well, it could help reduce unnecessary water use, lower electricity costs from pumping, and make irrigation decisions more data-driven.

For now, I am starting with simulation, sensors, and small experiments.
