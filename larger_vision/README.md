# Loss of Plasticity in Larger Vision Benchmarks

Code for reproducing experiments on CIFAR-10 and Tiny ImageNet.

---

## Running Experiments

```bash
# Unregularized (abrupt transitions)
python train_abrupt.py --config random_cifar_config.json        # CIFAR-10
python train_abrupt.py --config random_imagenet_config.json     # Tiny ImageNet

# Interpolation
python train_gradual.py --config random_cifar_config_smooth.json      # CIFAR-10
python train_gradual.py --config random_imagenet_config_smooth.json   # Tiny ImageNet
```

### Baselines

Set the `"baseline"` field in the corresponding abrupt config file to `spectral_reg`, `l2`, `redo`, or `shrink_perturb`, then run:

```bash
python baselines.py --config random_cifar_config.json       # CIFAR-10
python baselines.py --config random_imagenet_config.json    # Tiny ImageNet
```
