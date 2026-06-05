# stack_bowls PI0.5 Fine-Tuning

This folder contains a ready-to-use `pi05_base` fine-tuning setup for the local
`stack_bowls` dataset at:

`/home/xingrui/lueq/simulator/data/stack_bowls`

What is included:

- `train_episodes.json`: deterministic 90% train split
- `val_episodes.json`: deterministic 10% validation holdout
- `pi05_base_finetune.json`: LeRobot training config with:
  - `policy.path = "lerobot/pi05_base"`
  - `rename_map` from `stack_bowls` camera keys to PI0.5 base camera keys
  - training restricted to the train split

The split was created with:

- total episodes: `1047`
- seed: `42`
- split ratio: `90 / 10`

Run:

```bash
cd /home/xingrui/lueq/simulator/lerobot
python -m lerobot.scripts.lerobot_train \
  --config_path examples/training/stack_bowls/pi05_base_finetune.json
```

Camera rename mapping used for `pi05_base`:

```json
{
  "observation.global_image": "observation.images.base_0_rgb",
  "observation.left_wrist_image": "observation.images.left_wrist_0_rgb",
  "observation.right_wrist_image": "observation.images.right_wrist_0_rgb"
}
```

Notes:

- This config disables sim eval during training with `eval_freq = 0`.
- The validation split is a holdout list only. LeRobot's stock trainer does not
  run offline validation-loss sweeps on a second dataset split by default.
- If you later want to try `lerobot/pi05_libero_base`, you should use a
  different `rename_map` because that checkpoint expects different camera names.
