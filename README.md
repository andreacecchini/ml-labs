# Machine Learning Labs (`ml-lab`)

Repository condiviso per i laboratori settimanali di Machine Learning (`lab1/`, `lab2/`, ..., `labX/`).
Tutti i laboratori condividono un unico ambiente Python 3.12 gestito con [`uv`](https://docs.astral.sh/uv/) nella root del repository (`.venv/`), così da non dover riconfigurare le dipendenze ogni settimana.

## Struttura del repository

```text
ml-lab/
├── pyproject.toml       # Dipendenze condivise e vincolo Python 3.12
├── uv.lock              # Lockfile riproducibile delle versioni installate
├── .python-version      # Versione Python fissata a 3.12
├── .vscode/
│   └── settings.json    # Interprete .venv e root dei notebook sulla cartella labX/
├── .venv/               # Ambiente virtuale locale (ignorato da git)
└── labX/                # Cartella settimanale (notebook, script, dataset, ecc.)
```

## Utilizzo quotidiano

### 1. Avviare JupyterLab
Dalla root del repository:

```bash
uv run jupyter lab
```

Apri il notebook nella relativa cartella `labX/` e seleziona il kernel **`Python (ml-lab)`** se richiesto.

### 2. Usare VS Code
Apri la cartella root `ml-lab` in VS Code:
- `.vscode/settings.json` seleziona automaticamente `${workspaceFolder}/.venv/bin/python` come interprete Python.
- `jupyter.notebookFileRoot` è impostato a `${fileDirname}`: la working directory dei notebook rimane la cartella `labX/` del notebook stesso, così import locali (es. `import ml_utilities`) e percorsi relativi ai dataset funzionano subito senza modifiche.

### 3. Eseguire script Python
Dalla root (oppure da dentro una cartella `labX/`):

```bash
uv run python labX/script.py
```

In alternativa, puoi attivare il virtual environment nel terminale:

```bash
source .venv/bin/activate
python labX/script.py
```

## Gestione delle dipendenze

- **Aggiungere una nuova libreria** (aggiorna `pyproject.toml`, `uv.lock` e `.venv/`):
  ```bash
  uv add <pacchetto>
  ```
- **Ricreare l'ambiente esatto** su una nuova macchina o dopo un `git pull`:
  ```bash
  uv sync
  ```
- **Registrare il kernel Jupyter** (già configurato dentro `.venv/`, utile solo se ricrei `.venv` da zero):
  ```bash
  uv run python -m ipykernel install --sys-prefix --name ml-lab --display-name "Python (ml-lab)"
  ```

## Accelerazione hardware su Apple Silicon

- **PyTorch (`torch` / `torchvision`)**: supporta nativamente la GPU Apple Silicon tramite backend MPS (`torch.device("mps")`).
- **TensorFlow / Keras**: installati nella versione stabile più recente con esecuzione nativa su CPU Apple Silicon (`arm64`) per mantenere il pieno supporto alle versioni correnti senza conflitti con il plugin legacy `tensorflow-metal`.
