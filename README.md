# Latent CLAP Loss for Better Foley Sound Synthesis

Audio samples for the paper "_[Latent CLAP Loss for Better Foley Sound Synthesis](arxiv link here)_".

## Abstract

Foley sound generation, the art of creating audio for multimedia, has recently seen notable advancements through text-conditioned latent diffusion models. These systems use multimodal text-audio representation models, such as Contrastive Language-Audio Pretraining (CLAP), whose objective is to map corresponding audio and text prompts into a joint embedding space. AudioLDM, a text-to-audio model, was the winner of the DCASE2023 task 7 Foley sound synthesis challenge. The winning system fine-tuned the model for specific audio classes and applied a post-filtering method using CLAP similarity scores between output audio and input text at inference time, requiring the generation of extra samples, thus reducing data generation efficiency. We introduce a new loss term to enhance Foley sound generation in AudioLDM without post-filtering. This loss term uses a new module based on the CLAP model—Latent CLAP encoder—to align the latent diffusion output with real audio in a shared CLAP embedding space. Our experiments demonstrate that our method effectively reduces the Fréchet Audio Distance (FAD) score of the generated audio and eliminates the need for post-filtering, thus enhancing generation efficiency.

## Organization

Audio files are organized using the following folder structure:
```
audios
└── model_name
    └── Class_name
           ├── 01.wav
           ├── 02.wav
           └── 03.wav
 ```
#### `model_name`
Corresponds to the model name from the paper.
+ `baseline` -> baseline (LDM + Tuning) [1]
+ `GT` -> Ground Truth audios (Ground Truth)
+ `new` -> Proposed model (LDM + Tuning + Latent)


Each folder contains three 16kHz `.wav` files.

## Subjective Evaluation

the proposed method was compared to audio generated by the baseline model involving the tuning layer (LDM + Tuning)~\cite{yuan2023latent} and ground truth samples in a series of 7 online surveys. Each survey focused on a specific sound class, included 15 audio samples from each of the three models and was completed by 40 listeners. Respondents listened to one audio clip at a time and rated their quality and category fit on a scale from 0 to 10, where 0 indicates the lowest possible quality or fit to category, and 10 indicates the highest. Audio clips were presented in randomized order.

**Audio Quality Evaluation**

This table presents the class-specific audio quality ratings.

| Model                  | Dog Bark | Footstep | Gun Shot | Keyboard | Motor Vehicle | Rain  | Sneeze Cough | Average |
|------------------------|----------|----------|----------|----------|---------------|-------|--------------|---------|
| GT                     | 4.92     | 5.76     | 5.00     | 4.71     | 6.32          | 5.71  | 6.44         | 5.55    |
| LDM + Tuning           | 6.54     | 6.75     | 5.18     | 5.24     | 6.56          | 5.94  | 5.82         | 6.00    |
| LDM + Tuning + Latent  | 6.84     | 7.15     | 5.77     | 5.83     | 6.82          | 6.21  | 6.21         | 6.40    |

**Class Relevance Evaluation**

The table below shows the class-specific class relevance ratings.

| Model                  | Dog Bark | Footstep | Gun Shot | Keyboard | Motor Vehicle | Rain  | Sneeze Cough | Average |
|------------------------|----------|----------|----------|----------|---------------|-------|--------------|---------|
| GT                     | 5.74     | 5.66     | 5.74     | 5.48     | 6.06          | 6.32  | 7.19         | 6.03    |
| LDM + Tuning           | 7.53     | 6.64     | 5.93     | 5.48     | 6.32          | 6.85  | 7.07         | 6.55    |
| LDM + Tuning + Latent  | 7.76     | 7.30     | 6.37     | 6.36     | 6.80          | 7.24  | 7.40         | 7.03    |



**Summary of Averages**

The summary table provides an overall average rating for quality and relevance.

| Condition              | GT  | LDM + Tuning | LDM + Tuning + Latent |
|------------------------|-----|--------------|-----------------------|
| Fit                    | 6.03| 6.55         | 7.03                  |
| Quality                | 5.55| 6.00         | 6.40                  |




The average subjective ratings reveal that the Latent CLAP loss model outscored the baseline model and ground truth samples in both audio quality and category fitness. A mixed-design ANOVA showed that the main effect of model was significant for both audio quality ($F$(1.9,509.6) = 115.3, $p$ $<$ .001, $\eta_{p}^{2}$ = .3) and category fit ($F$(1.9,522) = 156.6, $p$ $<$ .001, $\eta_{p}^{2}$ = .36), while post-hoc paired t-tests with Bonferroni corrections further support the finding that the proposed model was rated significantly higher than both the baseline model and ground truth samples across both items ($p$ $<$ .001 for all comparisons). 

The lower ratings for the ground truth samples is unexpected and could be attributed to the fact that real-world recordings often contain extraneous noises or recording artifacts. Our generated audio presents cleaner sounds with features that are more distinctly aligned with the target class, offering a potentially clearer representation of the intended sound event, which may contribute to higher scores in human evaluations.
