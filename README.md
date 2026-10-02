Inspired by non-equilibrium thermodynamics, diffusion models have achieved stateof-the-art performance in generative modeling. However, their iterative sampling
nature results in high inference latency, which is typically required to maintain
image quality. While recent efforts in distillation techniques have improved sample
quality with fewer steps, they discard intermediate trajectory steps. By discarding
intermediate trajectory steps, these methods lose structural information, resulting
in significant discretization errors. To mitigate this issue, we propose a novel
framework, B-DENSE, that leverages multi-branch trajectory alignment. We train
the student model using branches that simultaneously map to the entire sequence
of the teacher’s target timesteps. We modify the student architecture to output
K−fold expanded channels. Each channel subset corresponds to a specific branch
representing a discrete intermediate step in the teacher’s trajectory. By enforcing
intermediate trajectory alignment, the student model learns to navigate the solution
space from the earliest stages of training, leading to better image generation quality
than the baseline distillation frameworks.

The results of our experiments are shown below 



<img width="618" height="499" alt="image" src="https://github.com/user-attachments/assets/c7b274e9-3994-4402-875c-79e21c8fbb08" />


In our framework, if the teacher model T performs N steps
that we want to distill into a single student step, we re-
configure the final layer in the student model S to output
N*C channels, where C is the number of channels in the
output image. These output channels are then conceptually
reshaped into N distinct ’branches’, each producing a full-
resolution tensor of shape [C,H,W]. N is set to 2 for our
experiments
Each branch is responsible for predicting one of the teacher’s
intermediate denoised images.The BranchDistillation loss is
then calculated as the sum of the reconstruction losses be-
tween each student branch prediction and its corresponding
teacher state.We use MSE loss. This has the effect of essen-
tially absorbing knowledge from all the timesteps used to
denoise , instead of just mapping the concerned outputs.
