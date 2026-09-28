# AIGC-5500 Lab 2 - Making Training Work

**Student:** Daniel Alejandro Castillo Martin  
**Student ID:** N10019284  
**Course:** Advanced Deep Learning (AIGC-5500-0NA)  
**Professor:** Hossein Pourmodheji  

Eight-layer `BrokenNet` on a synthetic binary classification task: reproduce the training failure, inspect gradients and activations, then test initialization and normalization changes in controlled stages.

## Files

```text
lab-02/
├── docs/ (assignment instructions and Week 2 learning module)
├── scripts/
│   └── lab02_starter.py (unchanged starter code)
├── daniel_alejandro_castillo_martin_LAB2.ipynb (submission notebook)
├── requirements.txt
├── README.md
└── .gitignore
```

## Run

1. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

   On Windows PowerShell, activate it with:

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Run the unchanged starter script from the repository root:

   ```bash
   python scripts/lab02_starter.py
   ```

4. Open `daniel_alejandro_castillo_martin_LAB2.ipynb` and **Run All** cells.

## Baseline and diagnosis

| Measurement | Result |
|---|---:|
| Baseline loss, epoch 0 | 0.7037 |
| Baseline loss, epoch 49 | 0.6893 |
| First hidden layer with 0% nonzero activations | [fill from notebook] |
| First hidden-layer mean absolute weight gradient | [fill from notebook] |
| Output-layer mean absolute weight gradient | [fill from notebook] |

The baseline loss remains close to approximately 0.693 after 50 epochs. [Add one or two sentences explaining the measured gradient and activation pattern.]

## Stages and results

| Stage | Change from previous stage | Final loss |
|---|---|---:|
| 0 | Unmodified `BrokenNet` | 0.6893 |

Initialization, normalization, and final comparison stages will be added in the next commit.