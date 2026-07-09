# Squash 'Em All 🚗🧟

A 2D top-down Unity game built to demonstrate core AI techniques for interactive agents: an AI-controlled car pursues zombies that react dynamically to threat, using finite state machines, raycasting-based perception, and predictive targeting.

Built for **CS4096 – Artificial Intelligence for Games**, MSc in Artificial Intelligence and Machine Learning, University of Limerick.

🎥 [Watch the demo](https://youtu.be/Frh88IB_e70)
📁 [Project files (Drive)](https://drive.google.com/drive/folders/1oaP8uKYl93D5eaLnqj0f8j-XZMR-s_mi?usp=drive_link)

---

## Overview

The zombies wander, flee, hide, and are destroyed when caught, transitioning based on environmental stimuli like car proximity and collisions. The AI car doesn't just chase the zombie's current position — it estimates where the zombie *will* be and steers to intercept, using obstacle-avoidance raycasts to navigate the environment along the way.

## AI Techniques

### Finite State Machine (Zombie behaviour)
Zombies transition between **Wander → Flee → Hide → Death** states based on triggers such as car detection, proximity to obstacles, or collision. Cooldown timers and smoothed transitions were added to prevent state oscillation ("jitter") near detection thresholds.

### Raycasting (Car perception & obstacle avoidance)
The car projects forward, left, and right rays, using `LayerMask`s to distinguish zombies from environmental obstacles. Ray collisions drive steering decisions — continue, steer away, or trigger a back-off routine if the car gets stuck. Debug gizmos were used throughout development to visualise ray behaviour and tune the avoidance logic.

### Predictive Targeting (Pursuit)
Rather than chasing a zombie's current position, the car:
1. Estimates the zombie's velocity vector
2. Projects a future position (`velocity × lead time`)
3. Sets that predicted position as the steering target
4. Adjusts trajectory to intercept

Lead time is clamped and damped to prevent overshoot when zombies change direction abruptly.

## Architecture

Code is split by single-responsibility principle for clarity and easier debugging/tuning via Unity's Inspector:

| Script | Responsibility |
|---|---|
| `ZombieAI` | FSM logic (wander / flee / hide / death) |
| `AICarController` | Raycasting perception + predictive-targeting pursuit |
| `ZombieHit` | Collision handling and death effects |

```
ZombieAI (FSM Logic) ←──detection── AICarController (Raycasting & Predictive Targeting)
       ↑                                        ↑
   state change                            collision trigger
       └──────────────── ZombieHit ─────────────┘
```

## Known Limitations & Future Work

- No group/coordinated zombie behaviour — each agent reacts independently
- Car avoidance is reactive rather than strategic; no true pathfinding
- Occasional jitter remains in densely cluttered environments
- Only partial audio integration (continuous zombie groan implemented; state-based sound cues incomplete)

Natural extensions: A\* or navmesh-based pathfinding for the car, learning-based (e.g. RL) adaptive zombie behaviour, and richer multimodal audio feedback tied to FSM state.

## Tech Stack

- **Engine:** Unity (2D)
- **Language:** C#
- **AI Techniques:** Finite State Machines, Raycasting, Predictive/Interception Targeting

## References

- Millington, I. & Funge, J. (2009). *Artificial Intelligence for Games*.
- Rabin, S. (2015). *Game AI Pro*.
- Isaacs, R. & Barron (1985). Pursuit-evasion models in differential game theory.

---

**Author:** Govind Pathak — [LinkedIn](https://linkedin.com/in/pathak-govind) · [GitHub](https://github.com/Pathak-G)
