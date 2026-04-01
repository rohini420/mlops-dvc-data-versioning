# 🍷 DVC Data Versioning — Wine Quality Dataset

> A hands-on experiment implementing Git + DVC workflows for scalable, reproducible data versioning.

![DVC](https://img.shields.io/badge/DVC-945DD6?style=for-the-badge&logo=dvc&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![S3](https://img.shields.io/badge/Amazon_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)

---

## 📌 Overview

This project demonstrates a complete **Data Version Control (DVC)** workflow using the Wine Quality dataset. The core idea: let **Git track code**, let **DVC track data** — keeping repositories lightweight while maintaining full reproducibility across dataset versions.

---

## 🗂️ Project Structure

```
project/
├── data/
│   ├── WineQT.csv          # Dataset (tracked by DVC, ignored by Git)
│   └── WineQT.csv.dvc      # DVC metadata pointer (tracked by Git)
├── .dvc/
│   └── config              # DVC remote storage config
├── .gitignore              # Auto-updated by DVC to exclude raw data
└── README.md
```

---

## 🚀 Workflow Walkthrough

### 1. Environment Setup

Install DVC with S3 support:

```bash
pip install dvc[s3]
dvc init
```

---

### 2. Track Dataset with DVC

Add the dataset to DVC — this creates a `.dvc` metadata file and excludes the raw data from Git automatically:

```bash
dvc add data/WineQT.csv
```

---

### 3. Commit Metadata to Git

Only the lightweight `.dvc` pointer file goes into Git:

```bash
git add data/WineQT.csv.dvc .gitignore
git commit -m "track initial dataset version"
```

---

### 4. Push Data to Remote Storage

Configure a remote (local path, S3, GCS, etc.) and push:

```bash
dvc remote add -d myremote s3://your-bucket/path
dvc push
```

---

### 5. Update Dataset Version

Simulate a new dataset version, re-track, and push:

```bash
# (modify data/WineQT.csv)
dvc add data/WineQT.csv
git add data/WineQT.csv.dvc
git commit -m "updated dataset version"
dvc push
```

---

### 6. Reproduce Any Historical Version

Use Git to navigate commits and DVC to restore the corresponding data:

```bash
# View version history
git log --oneline

# Switch to an older dataset version
git checkout <commit_id>
dvc pull

# Verify the rollback
tail data/WineQT.csv

# Return to latest
git checkout main
dvc pull
```

---

## 🧠 Key Concepts

| Concept | Tool | Role |
|---|---|---|
| Code versioning | Git | Tracks scripts, configs, `.dvc` files |
| Data versioning | DVC | Tracks dataset changes via metadata |
| Remote storage | S3 / Local | Stores actual large files outside Git |
| Reproducibility | Git + DVC | Restore any dataset state from history |

---

## ✅ Key Outcomes

- **Separation of concerns** — Git stays lean; data lives in dedicated storage
- **Efficient storage** — Large files never bloat the Git repository
- **Full version history** — Every dataset state is recoverable
- **Team-friendly** — Collaborators pull exact data versions with `dvc pull`

---

## 💡 One-Line Summary

> Implemented a DVC-based data versioning workflow to track dataset changes, store data in remote storage, and reproduce historical dataset versions using Git and DVC integration.

---

## 📚 References

- [DVC Documentation](https://dvc.org/doc)
- [Wine Quality Dataset — UCI ML Repository](https://archive.ics.uci.edu/ml/datasets/wine+quality)
