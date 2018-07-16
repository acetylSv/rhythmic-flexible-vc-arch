## Rhythm-flexible Voice Conversion Without Parallel Data Using Cycle-GAN over Phoneme Posteriorgram Sequences

### Model Architectures
<img src=https://www.csie.ntu.edu.tw/~b02902099/arch_v2.png alt="drawing" width="500px"/>

* FC indicates fully-connected layer.
* N_phoneme equals to 70 in all experiments.
* [1] CBHG module: [Tacotron: Towards End-to-end Speech Synthesis](https://arxiv.org/pdf/1703.10135.pdf)
* [2] Bahdanau attention mechanism: [Neural Machine Translation by Jointly Learning To Align and Translate](https://arxiv.org/pdf/1409.0473.pdf)
* [3] [Parallel-data-free Vocie Conversion Using Cycle-consistent Adversarial Network](https://arxiv.org/pdf/1711.11293.pdf)

### Training Details
#### Optimizer and Learning Rate
* We chose ADAM optimizer for all of our neural network structures.
* The learning rate was set to be 0.0001.
#### For PPR
1. Trained with objective mentioned in section 3.1 for 100,000 batch steps.
2. Per frame accuracy is about 80% for VCTK and 75% for Librispeech.
#### For PPTS
1. Trained with objective mentioned in section 3.2 for 200,000 batch steps.
2. Generally, the loss began to converge at about 50,000 batch steps, but keep training was able to refine the quality of the output spectrograms. 
#### For UPPT
1. Pretrained the UPPT (generators) using the auto-encoder reconstruction loss for 20,000 batch steps with scheduled sampling to make sure the attention mechanism was good enough, 
2. Started the Cycle-GAN training with the objective in equation5 for another 60,000 batch steps.
* As GAN has been notoriously hard to train, we applied a different objective function, Wasserstein GAN with gradient penalty (WGAN-GP), to stabilize the training process of GAN for UPPT.
* We updated the generator once but the discriminator twice in each batch step when training GAN.
* The balancing parameters lambda_1, lambda_2 were set ot be 10 and 5.
