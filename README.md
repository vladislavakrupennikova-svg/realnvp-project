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

Steps with peictures:

Examples from dataset:
<img width="1029" height="160" alt="image" src="https://github.com/user-attachments/assets/370b5446-3495-49a2-9238-5d9e61885649" />

Training metrics:
Loss (NLL): measures how well the model fits the data (lower is better)
avg_logdet: tracks how the transformation scales space (stability & invertibility)
Bits-per-dim (BPD): normalized likelihood, standard metric for generative models (lower is better)
<img width="1078" height="334" alt="image" src="https://github.com/user-attachments/assets/4013c14b-ace5-4229-badc-464d3446222b" />

Reconstruction: x -> z - >x_rec, MSE, visualization
<img width="687" height="459" alt="image" src="https://github.com/user-attachments/assets/6d74de64-8885-44f3-bd5e-786b9d898600" />

MSE: Mean Squared Error between original and reconstructed inputs (first 16 samples)
Value ~ 3.4e-15:
- essentially zero error - near-perfect reconstruction
- confirms invertibility of the model (no information loss)

Projection of the latent space. Shows the distribution of points in the latent space by the first two coordinates.
<img width="540" height="479" alt="image" src="https://github.com/user-attachments/assets/33de2b4d-4be9-4764-9884-178d51c8ca2d" />

A series of images of intermediate values between two endpoints shows how the noise becomes a picture.
<img width="638" height="79" alt="image" src="https://github.com/user-attachments/assets/7524ccbe-3589-472c-9932-9d87bb193e21" />

Generation of new layers. We take random noise, invert it, and generate new images.
<img width="439" height="254" alt="image" src="https://github.com/user-attachments/assets/1aaf4d37-9917-4a18-9801-11cf90bc4978" />

Histogram and statistics check the logdet distribution for model stability
<img width="560" height="348" alt="image" src="https://github.com/user-attachments/assets/0b824dd9-c026-4605-9b53-63eaca4de028" />

Generating images

<img width="473" height="465" alt="image" src="https://github.com/user-attachments/assets/e8f5c282-e4c7-4d2d-a8bc-916fb53c7993" />
