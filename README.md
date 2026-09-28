# AIGC-5500 Lab 2 - Making Training Work

**Student:** Daniel Alejandro Castillo Martin  
**Student ID:** N10019284  
**Course:** Advanced Deep Learning (AIGC-5500-0NA)  
**Professor:** Hossein Pourmodheji  

Diagnose why the provided eight-layer `BrokenNet` fails to train on a synthetic binary classification task. The project reproduces the failure, measures gradients and activations, and compares initialization and normalization changes in controlled training stages.

## Files

```text
lab-02/
├── docs/
│   ├── AIGC5500_Lab02_Making_Training_Work.pdf
│   └── AIGC5500_Week02_Learning_Module.pdf
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

2. Install the dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Run the unchanged starter script from the repository root:

   ```bash
   python scripts/lab02_starter.py
   ```

4. Open `daniel_alejandro_castillo_martin_LAB2.ipynb` from the repository root and **Run All** cells. Select the Python environment created above as the notebook kernel.

The notebook reproduces the baseline, reports the gradient and activation diagnosis, trains each experimental stage, and plots their loss curves together.

## Baseline and diagnosis

The unchanged starter trains `BrokenNet` for 50 epochs with plain SGD and a learning rate of 0.1. Its loss decreases only from **0.7037** to **0.689301**, remaining near the reference value of approximately **0.693**.

The activation check shows that nonzero outputs fall from **6.71%** in hidden layer 1 to **0.12%** in layer 2. Hidden layer 3 is the first to output zeros across all 256 training examples; subsequent hidden layers also output zeros. The output weight receives no measurable gradient, while the output bias receives a mean absolute gradient of approximately **0.08847** before training.

The starter initializes every hidden bias to **-2.0**. Most pre-activations become negative, and ReLU maps them to zero. Once the hidden activations collapse, the output cannot use the input features. Updating its bias can slightly reduce the loss, but it cannot learn the classification boundary.

## Experimental stages

All runs use the same 256-example synthetic dataset, eight hidden layers of width 32, ReLU activations, plain SGD with a learning rate of 0.1, and 50 epochs. Each run creates a fresh model with `torch.manual_seed(0)`.

| Stage | Hidden-layer initialization | Normalization | Final training loss |
|---|---|---|---:|
| 0: Baseline | Starter weights; biases of −2.0 | None | 0.689301 |
| 1: Initialization | Kaiming normal weights; zero biases | None | 0.363229 |
| 2: Normalization | Starter weights; biases of −2.0 | BatchNorm before each ReLU | 0.243709 |
| 3: Initialization and Normalization | Kaiming normal weights; zero biases | BatchNorm before each ReLU | **0.227591** |

Stages 1 and 2 each change one category relative to Stage 0. Stage 3 adds corrected initialization to Stage 2. The notebook reports a separate loss curve for each run and displays all four curves together.

## Conclusion

The baseline network ended at a loss of **0.689301** after 50 epochs. Its hidden activations collapsed to zero by layer 3, preventing signals from reaching the output. The output bias could still update, explaining the small decrease in baseline loss.

Corrected initialization reduced final loss to **0.363229**, while batch normalization alone reduced it to **0.243709**. Normalization therefore made the largest isolated contribution under the fixed training settings: a reduction of **0.445592** relative to the baseline.

Combining both changes produced the lowest loss, **0.227591**. This improved on normalization alone by **0.016118**, showing that corrected initialization added a smaller benefit once normalization was already present.