# SO-ARM101 Isaac Lab Pick-and-Place

Research prototype for training a six-action SO-ARM101 pick-and-place policy with PPO in NVIDIA Isaac Lab. The task runs entirely in simulation and is not presented as physical-robot validation.

## What is included

- `pick_place_env.py` — 512 parallel environments, a 30-dimensional observation, randomized cube/goal positions, grasp/lift/place rewards, and episode resets.
- `train_pick_place.py` — a compact `rsl_rl` PPO runner and training configuration.
- `so101_robot_cfg.py` — Isaac Lab articulation and actuator configuration.
- `so101_flat.usd` — a converted SO-101 simulation asset with upstream attribution in `THIRD_PARTY_NOTICES.md`.

Training logs and checkpoints are intentionally excluded from the public repository. They are run artifacts, not source code, and the current repository does not claim a reproducible benchmark score.

## Task flow

```text
random cube + goal
        ↓
30-D observation → PPO policy → 6 joint targets
        ↑                         ↓
   reach / grasp / lift / place rewards
```

The observation combines joint state, end-effector position, cube position, goal position, relative vectors, distances, and cube height. Actions are mapped from `[-1, 1]` to the six joint limits.

## Run

Install a compatible NVIDIA Isaac Sim, Isaac Lab, PyTorch, and `rsl_rl` environment first. Then run from the repository root:

```bash
python train_pick_place.py
```

Outputs are written to `logs_pick_place/<timestamp>/`, which is ignored by Git.

## Status and limitations

- Simulation only; no sim-to-real result is claimed.
- Reward shaping and the grasp latch are experimental.
- The public snapshot does not include pretrained weights.
- Reproduction depends on compatible Isaac Lab and `rsl_rl` versions.

## License

Original Python code is under the MIT license. The converted SO-101 asset follows the upstream Apache-2.0 terms described in `THIRD_PARTY_NOTICES.md` and `LICENSES/Apache-2.0.txt`.
