# Impact of SMILES Notational Inconsistencies on Chemical Language Models Trained via Molecular Translation

This repository contains the official implementation of the paper:

> **Impact of SMILES Notational Inconsistencies on Chemical Language Models Trained via Molecular Translation**
> Yosuke Kikuchi, Yasuhiro Yoshikai, Shumpei Nemoto, Ayako Furuhama, Takashi Yamada, Hiroyuki Kusuhara, and **Tadahaya Mizuno**†
> *Preprint / Submitted, 2025.*
> [[Project Page]](https://github.com/mizuno-group/NotationalInconsistency) | [[Manuscript]](https://arxiv.org/abs/2505.07139)

---

## Abstract

Chemical Language Models (CLMs) are increasingly used for molecular modeling, yet their reliability is undermined by inconsistencies in the SMILES notation.
Even “canonical” SMILES differ across toolkits, and stereochemical annotations are frequently incomplete, but the consequences for model behavior remain unclear.

We systematically assess these effects through a literature survey, dataset analyses, and controlled modeling experiments.
Our findings show that:

* nearly half of CLM studies omit canonicalization details,
* public benchmarks contain redundant encodings and missing stereochemistry,
* structural comprehension tasks are impaired,
* property prediction tasks appear deceptively robust due to feature selection, and
* notational artifacts can spuriously inflate benchmark performance.

This repository provides the training and analysis pipeline used in the study, from PubChem preprocessing to Transformer–VAE model training and downstream evaluation.

---

## Installation

We recommend using **Python ≥3.10** and a clean virtual environment.

### From GitHub (latest version)

```bash
pip install git+https://github.com/mizuno-group/NotationalInconsistency.git
```

---

## Directory Structure

```
NotationalInconsistency/
├── src/
│   └── notate/                 # main package
│       └── __init__.py
├── scripts/
│   ├── 1_preprocess.py         # data preprocessing and SMILES canonicalization
│   ├── 2_tokenize.py           # tokenizer and vocabulary builder
│   └── 3_train.py              # training pipeline (Transformer + VAE)
├── data/                       
├── notebooks/                  # example notebooks for analysis and visualization
│   └── figure.ipynb
├── pyproject.toml
├── LICENSE
└── README.md
```

---

## Requirements

All dependencies are listed in `pyproject.toml`.
Main requirements include:

* Python ≥3.10
* PyTorch ≥1.8
* RDKit ≥2024.03
* scikit-learn ≥1.6
* numpy, pandas, tqdm, optuna, xgboost, addict

To reproduce the paper’s results:

```bash
git clone https://github.com/mizuno-group/NotationalInconsistency.git
cd NotationalInconsistency
pip install -e .
```

---

If the installation fails, please try upgrading your packaging tools first:
```bash
python3 -m pip install -U pip setuptools wheel
```

---


## Quick Start

### 1. Preprocess PubChem data

```bash
python scripts/1_preprocess.py
```

This script reads `Pubchem_chunk_*.csv` and generates:

* `Pubchem_chunk_pro_*.csv` (canonical + randomized SMILES pairs)

### 2. Tokenize SMILES

```bash
python scripts/2_tokenize.py
```

This creates `.pkl` tokenized datasets for canonical and randomized SMILES.

### 3. Train the Model

```bash
python scripts/3_train.py
```

* Uses Transformer + Variational Autoencoder (VAE)
* Supports multi-epoch training with automatic dataset switching
* Outputs logs, checkpoints, and validation metrics to `./result/`

---

## Key Scripts

| Script           | Description                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------- |
| **`1_preprocess.py`** | Canonicalizes and randomizes SMILES from PubChem, generating paired datasets.                |
| **`2_tokenize.py`**     | Defines `VocabularyTokenizer` and converts SMILES strings into token indices for training.   |
| **`3_train.py`**   | Main training script integrating data loading, model construction, hooks, and checkpointing. |

Each component is modular and can be reused for other SMILES-based CLM studies.

---

## Citation

If you find this work useful, please cite:

```bibtex
@article{Kikuchi2025NotationalInconsistency,
  title   = {Impact of SMILES Notational Inconsistencies on Chemical Language Models Trained via Molecular Translation},
  author  = {Yosuke Kikuchi and Yasuhiro Yoshikai and Shumpei Nemoto and Ayako Furuhama and Takashi Yamada and Hiroyuki Kusuhara and Tadahaya Mizuno},
  year    = {2025},
  journal = {Preprint / Submitted},
  url     = {https://github.com/mizuno-group/NotationalInconsistency}
}
```

---

## License

This project is licensed under the **MIT License**.
See the `LICENSE` file for details.

---

## Authors

* **[Yosuke Kikuchi](https://github.com/KikuchiY16)**
* **[Tadahaya Mizuno](https://github.com/tadahayamiz)**

---

## Contact

For questions or collaborations:

* Tadahaya Mizuno — `tadahaya[at]gmail.com` (lead contact)

---

## Acknowledgements

This work was supported by:

* **AMED** (JP22mk0101250h, 23ak0101199h0001)
* **MHLW** (21KD2005, 24KD2004)

We thank all contributors of open datasets including **PubChem**, **MoleculeNet**, and **Therapeutics Data Commons**.

---

## Related Links

* [RDKit Documentation](https://www.rdkit.org/)
* [MoleculeNet Benchmark](https://moleculenet.org/)
* [Therapeutics Data Commons](https://tdcommons.ai/)
