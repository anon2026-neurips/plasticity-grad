# MNIST Benchmarks

Code for reproducing experiments on the Random MNIST benchmark.

---

## Running Experiments

```bash
# Unregularized (abrupt transitions)
python mnist_abrupt.py --config random_mnist_config.json

# Unregularized (smooth interpolation)
python mnist_gradual.py --config random_mnist_config_smooth.json
```

### Baselines

Set the `"baseline"` field in `random_mnist_config.json` to `spectral_reg`, `l2`, `redo`, or `shrink_perturb`, then run:

```bash
python baselines.py --config random_mnist_config.json
```
