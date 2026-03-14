# realnvp-project
RealNVP for MNIST: Invertible Flow-Based Generative Modeling

RealNVP (Real-valued Non-Volume Preserving transformation) is a reversible flow-based model that enables:
1) Construction of a bijective transformation between data (x) (e.g., images) and latent variables (z) with a known prior distribution (typically Gaussian).
2) Exact computation of the data log-likelihood, facilitated by the tractable Jacobian determinant.

Key Features:
1) The model comprises affine coupling layers, where a subset of features remains "frozen" (via a mask), while the complementary subset undergoes scale and shift transformations conditioned on the frozen features via a neural network. This design ensures invertibility and efficient Jacobian computation.
2) Layers alternate masks to ensure all features are eventually transformed.
3) It supports new data generation, interpolation in the latent space, and visualization of the image synthesis process from noise.

Program Functionality:
1) Data Loading and Preprocessing:
   Loads MNIST dataset and downsamples images.
   Flattens each image into a fixed-dimensionality vector (dim = img_size * img_size).
2) RealNVP Model Construction:
   Specifies number of coupling layers.
   Alternates masks across layers to ensure all features are processed.
   Each layer employs a small neural network (STNet) to predict scale s (scale) and t (translation).
3) Model training
   Forward pass: (x -> z).
   Computes negative log-likelihood (NLL): NLL = -(log p(z) + log |det J|)where J is the Jacobian of the transformation.
   Loss Function Explanation:
   - Likelihood measures how probable observed data is under the model; higher likelihood indicates better data fit.
   - Log-likelihood converts products of probabilities to sums of log-probabilities for numerical stability.
   - Negative log-likelihood is minimized during training (equivalent to maximizing likelihood).
   log p(z): Log-probability of latent vector z under the base distribution (first term).
   z: Latent representation of input x after forward transformation.
   p(z): Density under base distribution (typically standard Gaussian).
   - Updates weights via gradient descent.
 4) Training Visualization:
    Loss (NLL): Model fit to data.
    Average logdet: Mean log-Jacobian determinant (critical for training stability).
    Bits per dimension (BPD): Standard metric for generative model quality.
 5) Invertibility Verification:
    Forward: (x -> z).
    Inverse: (z -> x_rec).
    Compares original and reconstructed images - computes MSE.
 6) New Image Generation:
    Samples latent vector (z ~ N(0, I)).
    Applies inverse transformation to generate novel images.
 7) Latent Space Interpolation:
    Linearly interpolates between two latent points.
    Inverse mapping produces smooth transitions between images.
 8) Stepwise Inversion and GIF Animation:
    Visualizes progressive transformation of noise to image through each RealNVP layer.

Examples from dataset:
