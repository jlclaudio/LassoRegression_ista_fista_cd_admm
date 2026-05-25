# Lasso and Sparse Compressed Sensing

Final project for the M.Sc. course "Machine Learning Numerical Analysis" at Politecnico di Milano. 
This repository explores the application of the Lasso model in Compressed Sensing (CS) problems, achieving precise reconstruction of sparse signals through $\ell_1$ regularization.

## Project Framework

The project independently implements four classic numerical optimization algorithms from scratch to solve the non-smooth Lasso objective function:
- **ISTA** (Iterative Shrinkage-Thresholding Algorithm)
- **FISTA** (Fast Iterative Shrinkage-Thresholding Algorithm with Nesterov momentum acceleration)
- **CD** (Coordinate Descent)
- **ADMM** (Alternating Direction Method of Multipliers)

These algorithms are evaluated under two core experimental scenarios:
1. **1D Sparse Signal Recovery**: Reconstructing a sparse 1D signal from undersampled, noisy Gaussian measurements.
2. **2D Medical Image Compressed Reconstruction (MRI Simulation)**: Reconstructing a 2D Shepp-Logan phantom from highly undersampled random Fourier measurements (simulating rapid MRI scanning). We implement **Variable Density Sampling** and use the **Discrete Cosine Transform (DCT)** as the sparsifying basis.

## How to Deploy and Run

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Install dependencies:**
   Ensure you have Python 3.8+ installed. You can install the required packages (e.g., using a virtual environment):
   ```bash
   pip install numpy scipy scikit-image scikit-learn matplotlib jupyter
   ```
   *(Note: Alternatively, check `requirements.md` if available for specific versioning).*

3. **Run the Notebook:**
   Start Jupyter Notebook or Jupyter Lab in the project directory:
   ```bash
   jupyter notebook Lasso_Compressed_Sensing_EN.ipynb
   ```
   Execute the cells sequentially to observe the mathematical derivations, dataset generation, algorithm execution, and resulting visualizations. Both an English (`_EN`) and a Chinese (`_CN`) version of the notebook are provided.

## Experimental Results

The comparative analysis across different solvers yields the following insights:

- **1D Signal Recovery**: All four algorithms accurately pinpoint the support set of the non-zero elements. ADMM converges in the fewest steps (<60), followed by FISTA (~150 steps). ISTA is the slowest among the gradient-based methods.
- **2D MRI Reconstruction**: 
  - Using a **60% Variable Density Fourier Sampling** mask (enforcing 100% sampling in the ultra-low frequency central region), all algorithms successfully reconstructed the Shepp-Logan image, surpassing an exceptional **Goodness of Fit ($R^2$) of 0.98**.
  - **FISTA** demonstrates the best overall performance, striking an optimal balance between computational efficiency (e.g., ~1.45s runtime) and convergence accuracy. It is highly recommended for large-scale $\ell_1$ optimization.
  - **ADMM** requires the fewest iterations but incurs a heavy computational overhead per step due to matrix inversion operations. 
  - **CD (Coordinate Descent)**, while theoretically sound, suffers from a severe performance bottleneck in Python due to large-scale, non-parallelizable nested `for` loops, making it the slowest solver in practice.

For more detailed mathematical derivations and evaluation graphs, please refer to `report.pdf`.
