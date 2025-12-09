🧠 Deep Reinforcement Learning for Personalized Radiotherapy Beam Orientation Optimization

This repository accompanies our research work introducing a Deep Reinforcement Learning (DRL) framework for patient-specific beam orientation optimization (BOO) in radiotherapy.
The method learns to select clinically meaningful gantry angles directly from anatomical geometry, without requiring repeated forward Monte Carlo dose evaluations.

📌 Overview
🎯 Problem

Selecting optimal beam orientations is critical for achieving high-quality radiotherapy plans.
Conventional BOO techniques (equiangular templates, heuristics, combinatorial solvers) are:

Not personalized to anatomy

Computationally infeasible at scale

Insensitive to voxel-level geometry

🚀 Proposed Solution

We formulate BOO as a sequential decision process and train a Deep Q-Network (DQN) to:

Read patient CT + anatomical structures

Predict 5 optimal gantry angles

Accumulate a pseudo-physical dose surrogate over time

Balance PTV coverage and OAR avoidance

The system produces patient-adaptive beam sets in <1 second.

📁 Repository Structure
Beam-Angle-Optimization-in-Radiotherapy-Using-Deep-Reinforcement-Learning/
│
├── configs/
│   └── experiments.json        # Experiment configuration, hyperparameters
│
├── figures/
│   ├── strong/                 # Highly successful cases
│   ├── median/                 # Typical performance
│   ├── failure/                # Failure/weak cases
│   └── anomaly/                # Anatomical outliers
│
├── models/
│   └── best_dqn_model.pt       # Best performing trained checkpoint
│
├── results/
│   ├── summary_results.md      # Human-readable summary
│   └── test_results.csv        # Full numerical evaluation (100 patients)
│
├── utils/
│   └── repro.py                # Reproducibility utilities (seeds, env setup)
│
├── baselines.py                # Equiangular, heuristic, random baselines
├── eval_main.py                # Evaluation script producing figs+metrics
├── train.py                    # Full DQN training pipeline
├── requirements.txt            # Python dependencies
└── README.md                   # You are here

⚙️ Installation
git clone https://github.com/krishdef7/Beam-Angle-Optimization-in-Radiotherapy-Using-Deep-Reinforcement-Learning-
cd Beam-Angle-Optimization-in-Radiotherapy-Using-Deep-Reinforcement-Learning-
pip install -r requirements.txt

🧬 Dataset

We use the OpenKBP head-and-neck dataset, consisting of:

CT volumes

PTV structures

OARs: spinal cord, brainstem, bilateral parotids, mandible

Split

Train:   200
Valid:    40
Test:    100

🧠 Model Summary

Input: 8 channels (CT, PTV, 5 OAR masks, evolving dose map)

Action space: 36 angles (0–350° at 10° resolution)

Sequential selection: 5 beams

Architecture: 5-layer CNN + FC + masking

Exploration: ε-greedy + warm start heuristic

Replay buffer: 3000 transitions

Training time: ~3.5 hours (CPU only)

🌡️ Dose Surrogate (No Monte Carlo Required)

To avoid physics simulations during RL:

A ray-traced geometric field is generated per beam

Blurred with Gaussian kernel to approximate scatter

Accumulated over timesteps

This enables fast and stable RL without a dose engine.

🧪 Results — 100 Patient Evaluation
Method	Coverage	D95
DQN (Ours)	0.8059	0.2405
Equiangular	0.6867	0.1207
Heuristic	0.6397	0.0949
RandomMean	0.5883	0.0554

Highlights

+11.9% absolute increase in PTV coverage vs equiangular

Almost 2× improvement in D95

Learned policy clearly outperforms heuristic and random approaches

🔬 Representative Qualitative Examples

(Figures available under /figures/)

High-dose regions remain concentrated within the PTV

OARs remain sparsely irradiated

DVHs reflect better target coverage while avoiding critical structures

Sample metrics:

Patient	Coverage	D95
#8	100.0%	1.0000
#14	90.3%	0.3203
#56	89.1%	0.3596
#81	97.8%	0.6691
#90	100.0%	1.0000
🔄 Reproducing Results
To Evaluate the Model
python eval_main.py


Outputs:

results/test_results.csv

DVHs & dose maps in figures/

Terminal metrics

To Train from Scratch
python train.py

📚 Baselines Included

Equiangular beams

Geometry-driven heuristic beams

Random non-repeating beams

All evaluated under identical dose surrogate for fairness.

🧩 Key Technical Contributions

✔️ Formulation of BOO as a sequential RL problem

✔️ Use of voxel-level anatomy as input

✔️ Dose surrogate enabling fast learning without Monte Carlo

✔️ Action masking to prevent repeated beams

✔️ Warm start exploration to stabilize early training

✔️ High performance generalizing over 100 real CT geometries

🏥 Clinical Perspective

Higher D95 → higher local tumor control likelihood

Avoiding spinal cord/brainstem → reduced severe toxicity risk

Model outputs plans in <1 second, making it practical for:

Adaptive planning

Online replanning

Planning quality assurance

📌 Limitations (Honest and Realistic)

Surrogate dose ≠ full Monte Carlo accuracy

2D axial slice (currently)

Model trained only on head-and-neck cases

The method is research-prototype — not clinical.

🌱 Future Work

3D volumetric DQN/UNet encoders

Fast GPU Monte Carlo integration

Neural surrogate dose engines

Multi-objective RL (Pareto optimality)

Robustness to anatomical changes (online use)

📝 Citation

If you use this repository, please cite:

Deep Reinforcement Learning for Personalized Radiotherapy Beam Orientation Optimization:
Voxel-Level Policy Learning and Surrogate Dose Modeling.
Krish Garg, IIT Roorkee, 2025.

🙏 Acknowledgements

OpenKBP dataset contributors

IIT Roorkee – Department of Engineering Physics

No external funding was used.
