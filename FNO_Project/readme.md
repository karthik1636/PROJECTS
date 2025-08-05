# FNO-Gravity: A Surrogate Model for Self-Gravitating Systems

This project explores the use of **Fourier Neural Operators (FNOs)** to build a surrogate model for the evolution of a self-gravitating fluid under periodic boundary conditions. The work is directly inspired by the research in [_"Modeling Turbulent and Self-Gravitating Fluids with Fourier Neural Operators"_](https://arxiv.org/abs/2507.23662) (Poletti, Offner, et al., arXiv:2507.23662).

The primary goal was to validate the FNO's ability to learn complex, non-linear physical dynamics and test its stability in long-term, autoregressive predictions. A secondary goal was to investigate the model's performance when trained on **incomplete data**, mirroring a key challenge in real-world astrophysical observations.

---

## 🚀 Project Overview

The project was executed in three main phases:

### 1. Physics-Informed Data Generation
A simplified **3D gravity simulation** was created using the **Poisson equation in Fourier space**. This served as the *ground truth* data generator, modeling how initial, smooth density fluctuations evolve over a single time step.

### 2. Surrogate Model Training
A powerful **3D Fourier Neural Operator (FNO)** was trained to learn the mapping from an initial density field to its future state as dictated by the gravity simulation.

### 3. Validation & Advanced Testing

- **Single-Step Prediction:** Evaluating accuracy on a test set.
- **Autoregressive Rollout:** A long-term stability test where the model's output is fed back as its own input for 50 consecutive steps.
- **Incomplete Data Challenge:** A final, advanced test where the model was trained on inputs with 50% of the data randomly removed to see if it could infer the missing information and predict the correct evolution.

---

## 🔍 Key Findings & Insights

### ✅ 1. High-Fidelity Learning of Single-Step Physics

The FNO demonstrated an exceptional ability to learn the one-step gravity transformation. The model achieved a **near-zero Test Loss of 0.000003**, indicating near-perfect replication of the ground-truth physics for a single time step.

<img width="1190" height="418" alt="image" src="https://github.com/user-attachments/assets/8189e6d1-4f70-44e0-b2ae-34ff3401ab62" />


The visual comparison shows that the FNO's output is virtually indistinguishable from the true simulation.

---

### 🔁 2. Confirmed Long-Term Stability in Autoregressive Rollouts

This was the project's most significant success. When tasked with predicting **50 consecutive steps**, the FNO remained stable and physically plausible. It successfully simulated the gravitational collapse of initial fluctuations into a well-defined, **cosmic web-like structure** without exploding or vanishing.

This confirms that the model’s tiny per-step errors did **not catastrophically accumulate**, making it a robust tool for long-term simulation.

<img width="1190" height="418" alt="image" src="https://github.com/user-attachments/assets/7b9c4225-c00d-4a39-9cb3-fc73f5d610c4" />


---

### 🧩 3. Realistic Performance on the Incomplete Data Challenge

When trained on data with **50% of the input voxels missing**, the FNO still managed to learn the general evolution of the system.

- **A Harder Problem:** The training process showed this task was significantly more difficult, as the model had to both *infer the missing data* and *predict its physical evolution*.
- **Smoother Predictions:** The model's output was smoother and less detailed compared to the full-data experiment. This is an expected and realistic outcome, as the model produces a best guess from partial information.

<img width="1190" height="418" alt="image" src="https://github.com/user-attachments/assets/e02c33e2-a089-4a62-bfdb-96319bab9e47" />


Running long-term prediction using the trained FNO...
Evolution complete. Visualizing initial state vs. state after 50 steps.

<img width="1190" height="418" alt="image" src="https://github.com/user-attachments/assets/629d9da1-b88a-47ac-be7b-c55ad09e235f" />


#### Alignment with Research:
This outcome **directly validates** the research finding that prediction accuracy is *"significantly degraded"* when models are given incomplete information.

---

### A Note on Spectral Bias (Over-smoothing)

A key finding, even in the complete data experiment, is the slight visual difference between the ground truth "Target" and the "FNO Output". While the model's loss is nearly zero, the output appears smoother.

This is a well-known characteristic of Fourier Neural Operators called **spectral bias**. Here's what it means in the context of this project:

* **The Model's Shortcut:** To achieve the lowest possible error, the powerful FNO model found the most efficient "shortcut" was to perfectly learn the large-scale (low-frequency) structures while aggressively filtering out the fine-grained, small-scale (high-frequency) details.

* **Visual vs. Mathematical Accuracy:** This results in an output that is mathematically almost identical to the target (hence the `0.000003` loss) but visually smoother.

* **Alignment with Research:** This is not a failure but an expected and insightful outcome. The reference paper also notes this behavior, observing that their models' power spectra decay faster than the ground truth at high wave numbers, which is a direct result of this over-smoothing effect.

Crucially, the long-term autoregressive rollout proved that these minor high-frequency discrepancies were not significant enough to destabilize the simulation, validating the FNO as a robust surrogate model for the core physics.

---

## 🧠 Conclusion

This project successfully demonstrates that **Fourier Neural Operators** are a powerful and effective tool for building **stable surrogate models** of self-gravitating systems.

- FNOs can learn complex physical laws with high fidelity.
- They remain **stable** during long-term, autoregressive simulations.
- Even with incomplete data, FNOs can make *reasonable physical inferences*, although with some degradation in accuracy.

This work lays a strong foundation for future research into **AI-driven scientific simulation**, especially in astrophysics and cosmology.
