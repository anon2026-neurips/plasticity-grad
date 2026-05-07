# Loss of Plasticity in Language Models

Code for reproducing experiments on Random Seq2seq and Bigram Cipher tasks.

---

## Configuration

| Flag | Description |
|---|---|
| `--config` | Path to the config file containing hyperparameters (e.g., learning rate) |
| `--smooth` | Enable task sampling for smooth transitions |

---

## Random Seq2Seq

```bash
# Unregularized
python trainer.py --config train_config.json

# Task Sampling
python trainer.py --config train_config_smooth.json --smooth
```

### Baselines

```bash
python baselines.py --config train_config.json --baseline spectral_reg --reg_strength 0.001
python baselines.py --config train_config.json --baseline shrink_perturb --shrink 0.5 --perturb 0.01
python baselines.py --config train_config.json --baseline l2_reg --reg_strength 0.0000001
python baselines.py --config train_config.json --baseline redo
```

---

## Bigram Cipher

> **Note:** The keyword `cipher` must be included in the `exp_name` field of the config file to run this task.

```bash
# Unregularized
python trainer.py --config train_config_cipher.json

# Task Sampling
python trainer.py --config train_config_cipher_smooth.json --smooth
```

### Baselines

```bash
python baselines.py --config train_config_cipher_baselines.json --baseline spectral_reg --reg_strength 0.001
python baselines.py --config train_config_cipher_baselines.json --baseline shrink_perturb --shrink 0.5 --perturb 0.01
python baselines.py --config train_config_cipher_baselines.json --baseline l2_reg --reg_strength 0.0000001
python baselines.py --config train_config_cipher_baselines.json --baseline redo
```
