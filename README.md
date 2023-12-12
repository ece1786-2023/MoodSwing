# MoodSwing

Introduction

The goal of our project is to accurately classify song lyrics into emotions such as happy, sad, angry,
and relaxed. The motivation behind our goal is to understand the emotional impact of songs on
listeners and consider the possibility of using song data to analyze the mental state/health of people.
When achieved, this could be used to enhance music recommendation systems to satisfy the mood
of the user. We believe this to be an important and refreshing improvement in the industry of music
applications with the growth of NLP, opening considerable potential for mood-based
recommendation system that analyses lyrical information.

Data

For lyrics mood classification, our initial step involved utilizing the MoodyLyricQ dataset. This
dataset is a carefully curated collection comprising 2000 song titles and associated artist names,
spanning various genres. Each song is tagged with a single mood from four categories - Happy, Sad,
Angry, and Relaxed. Notably, these 2000 songs are evenly distributed, ensuring class balance across
the four mood tags. However, it's important to highlight that this dataset lacks the lyrics of the songs
due to copyright restrictions. To address this limitation, we turned to the Genius APIs' search feature,
allowing us to retrieve song lyrics by using the song title and artist name.

Architecture 

The analysis done in mood classification of lyrics was achieved by using two models. Our first
architecture is based on GPT2 model. The work was then progressed to
a larger model architecture based on GPT3.5.

GPT2 Architecture:

Our GPT-2 pretrained model implementation begins by structuring the model architecture with a
dataframe containing Song ID, preprocessed lyrics, mood labels, and corresponding encoded
numerical labels. The dataset is then split into 80% training and 20% validation sets, processed
through the GPT-2 tokenizer, and fed into the GPT2ForSeqeunceClassification pretrained model
from Hugging Face, accompanied by numerical labels. Training involves Cross Entropy Loss with
an Adam optimizer, utilizing a StepLR scheduler for learning rate optimization (initial rate: 5e-5,
gamma: 0.9). A batch size of 4, 20 epochs, and optimization techniques like gradient accumulation
and mixed precision are employed. Post-training, the model undergoes evaluation using specified
metrics.


![lyrical_arc](https://github.com/ece1786-2023/MoodSwing/assets/114831340/4a4b8842-3bca-4923-8168-2509e8e90032)


GPT3.5 Architecture:

The GPT3.5 model architecture mandates a specific input data format, which 
includes a system prompt in addition to lyrical text and mood labels. We initiate the process by
creating a prompt array that organizes the data accordingly. After formatting, the data is divided into
training, validation, and test datasets. Subsequently, we generate the necessary train, validation, and
test jsonl files essential for GPT3.5 model finetuning on OpenAI. Following file creation, the data is
uploaded, and a finetuning job for the GPT3.5 model is initiated with default hyperparameters. Once
the finetuned model is available, we assess its performance using a comprehensive test dataset and
specific metrics.

![gpt3 5](https://github.com/ece1786-2023/MoodSwing/assets/114831340/1a88b6ed-5ae9-49b4-8310-8fed1da44de2)

Interesting Findings:

The quantitative and qualitative analysis helped us explore certain interesting findings in the way
our system worked. The explanation for why we believe we observe such trends is hypothesized
below after analyzing the results deeply.

1. BERT shows better results than GPT2.
BERT may outperform GPT-2 in mood classification of lyrics due to its bidirectional context
understanding. Understanding the relationships between words in both directions helps
capture nuanced sentiment and context in lyrics. Additionally, BERT's token-level
representations allow for a detailed analysis of individual word sentiments, which is crucial
for accurately classifying the overall mood of lyrics where specific words heavily influence
the mood. The fine-grained contextual understanding provided by BERT makes it well-suited
for tasks like mood classification in text.

2. Minimal Pre-processing shows better performance.
For tasks like mood classification in song lyrics, minimal pre-processing without stemming
or lemmatization preserves detail and better addresses the complexity of understanding subtle
emotional nuances unique to creative content. Song lyrics may have a vocabulary that
standard rules of pre-processing fail to capture effectively.

3. Class Relaxed consistently was misclassified.
The concept of being "relaxed" could be more nuanced and challenging to define with
specific features. If "relaxed" instances share subtle similarities with "happy," the model may
struggle to differentiate between them.
This also shows the model might place a higher emphasis on valence (extent to which an
emotion is positive or negative), it might prioritize features related to positive valence, which
is common in both "relaxed" and "happy" instances. Emphasizing valence more might lead
the model to downplay arousal (intensity of the associated emotional state) differences. Since
valence is not sufficient to distinguish between "relaxed" and "happy," the model may default
to predicting the more prevalent class, "happy," when faced with similar valence patterns.

![image](https://github.com/ece1786-2023/MoodSwing/assets/114831340/aee26e92-4ef7-41f3-9bcf-059f6354ce3c)
