# Efficient Personalized EEG-Based Action Recognition: Unveiling Minimal Training Requirements

### Abstract

This study investigates personalized EEG-based action recognition models, exploring various training methods, model architectures, and amounts of training data.
Drawing from data sourced from a previous study, we simulate scenarios akin to newly joined participants to assess model personalization performance across diverse experimental setups.
Our primary aim is to determine the minimal requirements of data amount and training iterations essential for obtaining a personalized model.
The analysis reveals that models incorporating Batch Normalization layers achieve superior accuracy when trained on a subset of available data, with only 20\% of the training data yielding competitive results.
Notably, this 20\% of data translates to approximately seven seconds of brain activity recordings for each action, a finding accentuating the potential for efficiency in data utilization.
Furthermore, we find that traditional fine-tuning and transfer learning techniques exhibit limitations in scenarios characterized by significant inter-individual variability in EEG signals.
These methods often necessitated higher numbers of training iterations and larger amounts of training data to achieve comparable results, indicating challenges in knowledge transferability between individuals.
Our findings underscore the importance of personalized modeling approaches tailored to individual participants, emphasizing efficient data utilization and model optimization.


# Running the code

All experiments are contained within the [Personalization.ipynb](Personalization.ipynb) notebook and the results are explored in [Results.ipynb](Results.ipynb) notebook.

Before running the training, you need to download data from the [google drive](https://drive.google.com/drive/folders/1aMRkk7fKuDgmLysBC4WdmcfZvLqYt3MO?usp=sharing) and place it in the `data` directory.


# Dependencies
The code utilizes the [tfkan](https://github.com/ZPZhou-lab/tfkan/) repository for KAN layers.