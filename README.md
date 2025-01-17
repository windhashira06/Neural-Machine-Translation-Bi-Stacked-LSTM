# [Tensorflow] Neural Machine Translation 
<p align="center">
  <img src="https://user-images.githubusercontent.com/121651344/222886855-6f8bf43c-4dae-49cf-95f0-4cb57e739c81.jpg" alt="translation">
</p>

## Introduction 
Machine Translation is an important task in Natural Language Processing. Machine Translation has a lot of potentials, it helps to solve communication problems that occur in business, tourism, education, and health,...                                  
For NLP enthusiasts like me, Machine Translation is a very interesting topic, so I decided to undertake this task to find out what's happening inside it. In this task, I use Neural Machine Translation.
## Datasets
I use the [IWSLT'15 English-Vietnamese](https://github.com/windhashira06/NMT-with-Seq2Seq/tree/main/Neural-Machine-Translation/Dataset) dataset. This is a small dataset consisting of 133K pairs of train sentences, more than 1500 pairs of test sentences in 2012, and more than 1300 pairs of test sentences in 2013. In my opinion, this dataset is not high quality.
## Model
This source code was written by me based on <a href="https://nlp.stanford.edu/pubs/luong-manning-iwslt15.pdf">Stanford Neural Machine Translation System for Language Domains spoken language</a> paper.                                                           
The model I use is the Seq2Seq model combined with 1 layer of Attention, about the Attention mechanisms, I choose <a href="https://machinelearningmastery.com/the-luong-attention-mechanism/">Luong's Attention</a>. Model details:                                      
1. [Encoder](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/Model/encoder_block.py): I use 2-layer LSTMs of 512 units with bidirectional (i.e., 1 bidirectional layer for the encoder) and with dropout keep_prob of 0.8. The 2-layers are combined by the 'sum' method.
2. [Decoder](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/Model/decoder_block.py): I use 2-layer LSTMs of 512 units stacked (i.e., the output of the previous layer is the input of the next layer) and with dropout keep_prob of 0.8.
3. [Attention layer](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/Model/attention_layer.py): There are many Attention mechanisms, but I use Luong's Attention because it has many advantages such as easy to implement, easy to understand and adjust, and scalability for different types of Encoder.
## Processing steps
1. Pre-processing: these processing steps I have done in [vocabulary.py](https://github.com/windhashira06/NMT-with-Seq2Seq/tree/main/Neural-Machine-Translation/vocabulary.py), [language.py](https://github.com/windhashira06/NMT-with-Seq2Seq/tree/main/Neural-Machine-Translation/language.py), [lang_utils](https://github.com/windhashira06/NMT-with-Seq2Seq/tree/main/Neural-Machine-Translation/lang_utils.py), [dataloader.py](https://github.com/windhashira06/NMT-with-Seq2Seq/tree/main/Neural-Machine-Translation/dataloader.py).
2. Post-processing: these processing steps I have done in [inference.py](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/inference.py).
## Training detail
I train the model in 20 epochs with an initial learning rate is 0.0008. After 10 epochs, halve the learning rate after each epoch. The optimizer used here is Adam, and the max norm value used to clip the gradient is 1.0. The training files consist of  [loss.py](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/loss.py), [custom_lr.py](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/custom_lr.py), [train_utils.py](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/train_utils.py), [train.py](https://github.com/windhashira06/NMT-with-Seq2Seq/blob/main/Neural-Machine-Translation/train.py).

![Untitled](https://user-images.githubusercontent.com/121651344/222891175-a4443ef6-5a68-4f14-a75d-5e4219b2045d.gif)
## Result
To evaluate an NMT model, I use the [BLEU score](https://en.wikipedia.org/wiki/BLEU). Here is the quality evaluation scale of NMT based on the BLEU score:

|BLEU Score | Interpretation |
| -----------| -----------|
| <10 | Almost useless |
| 10-19 | Hard to get the gist |
| 20-29 | The gist is clear, but has significant grammatical errors |
| 30-40 | Understandable to good translations |
| 40-50 | High quality translations |
| 50-60 | Very high quality, adequate, and fluent translations |
| > 60 | Quality often better than human |

My model's BLEU score in the 2012 test set and 2013 test set is **22.21** and **24.15**.         
## Several factors affect model quality
- Data quality is not high: as mentioned, the Stanford dataset is not high quality because of several reasons: the number of sentences in the dataset is small, the translation quality is not uniform,...                             
- Language complexity: Vietnamese is a complex language because it has many unique and complex features in terms of vocabulary usage, grammar, and sentence structure.
- Computer resources are limited: Low GPU RAM. Therefore, during the preprocessing step, longer sentences than the initialized max_len must be truncated, leading to the loss of information and coherence within the sentence.
- Model architecture: the Seq2Seq model often has difficulties in handling long-distance relationships between words in sentences, is limited in handling too long sentences, and challenges in matching input and output,...
## Why do I choose Seq2Seq?
Currently, many models have given very good results in Machine Translation tasks such as [Transformer](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)), [GPT-2](https://en.wikipedia.org/wiki/GPT-2), [GPT-3](https://en.wikipedia.org/wiki/GPT-3), and [T5](https://github.com/google-research/text-to-text-transfer-transformer). But for an NLP enthusiast like me, understanding and practicing old models like Seq2Seq will greatly improve my knowledge in this task. So this is the first model I choose to solve the Machine Translation task.   


Thank you a lot for the finding! 😊







