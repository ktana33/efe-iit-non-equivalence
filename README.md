# Non-equivalence of expected free energy and revised integrated information in noisy permutation networks

Reproducibility package for the manuscript by **Katsuaki Tanabe**.

This repository contains the numerical code and derived numerical data needed to check the manuscript's principal computational results.

## Contents

```text
.
├── README.md
├── LICENSE
├── requirements.txt
├── pyphi_config.yml
├── src/
│   ├── validate_pyphi_results.py
│   └── audit_directed_partition_mip.py
└── results/
    ├── intrinsic_information_benchmark.csv
    ├── two_node_eta_sweep.csv
    ├── three_node_all_permutations_eta075.csv
    ├── three_cycle_eta_sweep.csv
    ├── four_node_cycle_validation.csv
    ├── five_node_cycle_validation.csv
    └── directed_partition_mip_audit_n2_n8.csv
```

## Computational environment

The calculations reported in the manuscript used:

- Python 3.13.12
- NumPy 2.5.2
- pandas 3.0.5
- SciPy 1.18.1
- PyPhi 2.1.dev1707+g1f47a1e20
- PyPhi commit `1f47a1e20b6a27fd92c25ec4e8a1aa5e829f8198`

The exact PyPhi commit is pinned in `requirements.txt`. The IIT settings used in the calculations are recorded in `pyphi_config.yml`.

## Installation

A clean Python 3.13 environment is recommended.

```bash
python -m venv .venv
```

Activate the environment, then install the dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Installation from the pinned PyPhi Git commit requires Git and internet access.

## Reproducing the numerical checks

Run the scripts from the repository root.

### 1. PyPhi validation for n = 2--5

```bash
python src/validate_pyphi_results.py
```

This reproduces the numerical checks reported in Sec. VIII of the manuscript, including:

- the 2026 intrinsic differentiation/specification benchmark;
- the 101-point reciprocal two-node sweep and disconnected self-copy control;
- exhaustive evaluation of all six three-node permutation architectures at `eta = 0.75`;
- the 101-point single three-cycle sweep;
- selected four- and five-cycle checks and non-strongly-connected controls.

The script writes the corresponding CSV files to `results/`.

For the fully observed state-risk construction with uniform future-state preferences, the script uses

```text
G2 = n [1 - h2(eta)]
```

in bits. In this symmetric construction this has the same numerical value as `I(X;Y)`, whereas `H(Y|X) = n h2(eta)` is reported separately.

### 2. Directed-set-partition audit

```bash
python src/audit_directed_partition_mip.py
```

This independently enumerates all distinct directed-set cut matrices for `n = 2,...,8` and checks the minimum-information-partition result. The expected numbers of distinct cut matrices are

```text
3, 22, 150, 1061, 7896, 61888, 510313
```

for `n = 2,...,8`, respectively. For `n = 3,...,8`, the global optimum cuts two actual cycle edges and has

```text
M = floor(4 n^2 / 7).
```

The audit writes `results/directed_partition_mip_audit_n2_n8.csv`.

## Precomputed results

The CSV files under `results/` are compact versions of the numerical outputs used for the manuscript. They are included so that the reported values can be inspected without rerunning the calculations.

Representative values include:

- `n = 2`: `eta* = 0.7719595736`, `phi_s,max = 0.7468055944` ibits;
- `n = 3`: `eta* = 0.8270887188`, `phi_s,max = 0.8216580134` ibits;
- at `eta = 0.75`, the reciprocal two-cycle has `phi_s = 0.6580828133` ibits and `G2 = 0.3774437511` bits, while the disconnected self-copy control has `phi_s = 0`;
- at `eta = 0.75`, both single three-cycles have `phi_s = 0.4935621100` ibits, whereas all four multicycle three-node architectures have `phi_s = 0`;
- the maximum PyPhi-versus-analytic discrepancies are at floating-point precision for the reported checks.

## License

The code and associated files in this repository are released under the MIT License. See `LICENSE`.
