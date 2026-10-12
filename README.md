# Hi, I'm Edgar 🦆

I build **embodied AI**: robots and vehicles that learn how their own body moves, then plan with that knowledge,
live, on very little compute. Based in Switzerland 🇨🇭. Formerly in DeFi, where I co-founded
[FujiDAO](https://github.com/Fujicracy), the first on-chain borrowing aggregator.

## The approach 🧠

I follow the **world-model** line of thought championed by Yann LeCun: don't train a policy per task, learn the
machine.

```
   learn the machine, once                          then every task is just a cost
 ┌──────────────────────────────┐               ┌──────────────────────────────────────┐
 │ task-free motion, randomized │   frozen      │ a planner (MPPI) imagines hundreds   │
 │ bodies  ──►  a tiny world    │ ───────────►  │ of futures through the model, 20x a  │
 │ model (JEPA / physics-based) │   5-30k       │ second, and keeps the cheapest:      │
 │ no reward, no goal, no task  │   params      │ grasp, catch, orbit, fly in a forest │
 └──────────────────────────────┘               └──────────────────────────────────────┘
```

- **World models, not policies.** Small JEPA-style and physics-informed models (SkyJEPA, LeJEPA's SIGReg), trained
  without any task. A new behaviour is a cost function written in an afternoon, never a new training run.
- **Compute where it matters.** Models of 5-30k parameters that run on a laptop CPU. A compute meter on every
  control tick shows imagining the future takes ~98 % of it and vision under 1 %, so the eyes run only when they
  change the plan. Heavy jobs (datagen, training, studies) go to cloud GPUs for a few dollars each.
- **Robots that interact.** Each robot publishes its *aura*, the near future it is imagining, and the others plan
  around it. Hand-overs happen on the other body's word ("I hold it"), never on a clock. Same rules for two arms, an
  arm and a drone, or two drones in a forest.

## The pile of world models 🤖

One recipe, carried from vehicle to vehicle:

| machine | world model | what it does |
|---|---|---|
| 🚁 **Quadrotor** | [SkyJEPA, 5.6k params](https://huggingface.co/Ethgar/skyjepa) | clean-room reimplementation of SkyJEPA, flown by MPPI: courses, ball drops, relays, two drones through a forest |
| ✈️ **Fixed-wing plane** | 5.5k params, physics + learned correction | trained on a laptop in 18 min; orbits **2-3x tighter than physics alone** on randomized airframes in wind |
| 🦾 **Aerial manipulator** | physics + learned correction | a 0.55 kg drone carrying a robot-arm pinch: takes objects from an arm, grabs a ball it sees in stereo |
| 🧵 **Cable robot** | [Stringman, 28k params](https://huggingface.co/Ethgar/stringman-world-model) | one model for any room: clears real rooms it never saw in training |
| 🤏 **Robot arm (SO-101)** | twin + learned model | live MPPI grasps on the **real arm** (4/4), two arms handing objects over with stereo eyes |
| 🐕 **Quadruped (Unitree Go2)** | in training | walking as costs, live MPPI |
| 🧍 **Humanoid (Unitree G1)** | in progress | whole-body skills learned in simulation on cloud GPUs, energy measured on every rollout |

## Things I've learned building it 🔬

- **Small wins.** The plane's 5.5k-parameter model flies exactly like its 32k sibling: the physics carries the
  dynamics, the network only learns the correction.
- **Train the model on what the planner will try.** A drone model trained on smooth flight was better on paper and
  worse in the air: the planner picked exactly the jerky commands it had never seen. Adding planner-like probing to
  the data is what made the plane's model beat physics in closed loop.
- **A model flies only the body it learned.** Two "identical" 1.3 kg quads with a different rotor order are two
  different machines.
- **Feel what the model can't predict.** A tiny observer turns prediction error into a felt force (a payload, wind,
  a partner pulling) on top of any frozen model, without retraining it.
- **Consent between robots.** Two drones must never touch, except when stacking is the task: then they say so, and
  only that pair relaxes.

## Watch and read 🎥

- [Robot-lab notebook](https://huggingface.co/spaces/Ethgar/robot-lab-notebook): real grasps, videos, numbers
- [Robot-lab papers](https://huggingface.co/spaces/Ethgar/robot-lab-papers): one page per module, the whole atlas
- [Hugging Face](https://huggingface.co/Ethgar): models, datasets, collections

## Also building 🛠️

Products from before and alongside: **LaunchView** 🛰️ (a Flightradar for space), **EzBoost** 📣 (context-based Meta
ads for restaurants), **MenuBoost** 🧾 (whiteboard menus straight to the website), **Team Hours** ⏱️ (check-in made
simple), and **Beeond** 🐝 (bio-inspired RL for a robotic pollinator).

## How I work

Pick a problem worth solving, build the leanest thing that proves the idea, measure it in simulation and on real
hardware, and let the numbers decide. Claude Code and fleets of AI agents do much of the typing; the judgment stays
mine.

## Let's talk 🤝

If you work on world models, embodied AI, autonomous vehicles or multi-robot systems, I'd love to compare notes.
