### Natural Language Inference

Keras implementation (tensorflow backend) of natural language inference

### Models

- [Decomposable Attention, EMNLP2016](https://arxiv.org/pdf/1606.01933v1.pdf)  
Parikh et al. A Decomposable Attention Model for Natural Language Inference.

- [InferSent, EMNLP2017](https://arxiv.org/pdf/1705.02364.pdf)  
Conneau et al. Supervised Learning of Universal Sentence Representations from Natural Language Inference Data.

- [ESIM, ACL2017](https://arxiv.org/pdf/1609.06038.pdf)  
Chen rt al. Enhanced LSTM for Natural Language Inference.

- Simases Bilstm  
using a bilstm based siamese architecture(two networks with the same structure and the same weight, each process one sentence in a pair) to model both premise and hypothesis.

- Simases CNN  
using a text-cnn based siamese architecture to model both premise and hypothesis.

- IA-CNN  
apply interaction attention (same as in esim model) to premise and hypothesis before feeding them into CNN network.

### Environment
- python==3.6.4
- keras==2.2.4
- nltk==3.2.5
- tensorflow=1.6.0
- fasttext
- glove_python

### Preparing Data

- NLI Data  
download `SNIL`, `MultiNLI`,  `MedNLI` data, put them in `raw_data/` dir:   
    1. [SNIL](https://nlp.stanford.edu/projects/snli/)  
    2. [MultiNLI](http://www.nyu.edu/projects/bowman/multinli/)  
    3. [MedNLI](https://jgc128.github.io/mednli/)  

- Pre-trained embeddings  
download pre-trained embeddings below, put them in `raw_data/word_embeddings` dir:  
    1. [glove_cc](http://nlp.stanford.edu/data/glove.840B.300d.zip)  
    2. [fasttext_wiki](https://dl.fbaipublicfiles.com/fasttext/vectors-english/wiki-news-300d-1M-subword.vec.zip), rename to `fasttext-wiki-news-300d-1M-subword.vec`  
    3. [fasttext_cc](https://dl.fbaipublicfiles.com/fasttext/vectors-english/crawl-300d-2M-subword.zip), rename to `fasttext-crawl-300d-2M-subword.vec`  

### Pre-processing
```
python3 preprocess.py
```

### Training
```
python3 train.py
```

### Performance

- SNIL

| model                      | batch_size | optimizer  | embedding   | train(paper)| train | dev(paper) | dev    | test(paper) | test  |train_time(1 TITAN X)|
|----------------------------|------------|------------|-------------|-------------|-------|------------|--------|-------------|-------|---------------------|
|decomposable                |   512      |   adam     |glove_cc_fix |   89.5      | 87.52 |-           |81.52   | 86.3        |81.19  |00:12:53             |
|decomposable(intra-sentence)|            |            |             |   90.5      |       |-           |        | 86.8        |       |                     |
|infersent(bilstm-max)       |   64       |   adam     |glove_cc_fix |   -	       | 90.54 |85.0        |85.43   | 84.5        |85.01  |01:28:26             |
|infersent(bilstm-mean)      |   64       |   adam     |glove_cc_fix |   -         | 87.33 |79.0        |83.62   | 78.2        |83.62  |01:14:32             |
|infersent(lstm)             |   64       |   adam     |glove_cc_fix |   -         | 92.09 |81.9        |84.20   | 80.7        |83.19  |00:53:43             |
|infersent(gru)              |   64       |   adam     |glove_cc_fix |   -         | 91.90 |82.4        |83.96   | 81.8        |83.30  |00:38:54             |
|indersent(bigru-last)       |   64       |   adam     |glove_cc_fix |   -         | 88.44 |81.3        |84.08   | 80.9        |83.64  |00:56:00             |
|infersent(bilstm-last)      |   64       |   adam     |glove_cc_fix |   -         | 89.75 |-           |84.27   | -           |83.63  |01:21:38             |
|infersent(inner-attention)  |   64       |   adam     |glove_cc_fix |   -         | 87.07 |82.3        |81.82   | 82.5        |82.23  |00:12:36             |
|infersent(hconv-net)        |   64       |   adam     |glove_cc_fix |   -         | 88.07 |83.7        |83.46   | 83.4        |83.23  |00:24:36             |
|esim                        |   32       |adam(0.0005)|glove_cc_tune|   92.6      | 90.81 | -          |87.55   | 88.0        |86.68  |11:03:31             |
|Siamese_BiLSTM              |   128      |   adam     |glove_cc_fix |   -         | -     | -          |83.47   | -           |83.22  |06:41:44             |
|Siamese_CNN                 |   128      |   adam     |glove_cc_tune|   -         | -     | -          |82.57   | -           |81.88  |00:33:51             |
|Siamese_IACNN               |            |            |             |   -         |

- MedNLI

| model                      | batch_size | optimizer  | embedding   | dev    | test  |train_time(1 TITAN X)|
|----------------------------|------------|------------|-------------|--------|-------|---------------------|
|decomposable(intra-sentence)|            |            |             |        |       |                     |
|decomposable                |   512      |   adam     |glove_cc_fix |81.52   |81.19  |00:12:53             |
|infersent(lstm)             |   64       |   adam     |glove_cc_fix |84.20   |83.19  |00:53:43             |
|infersent(gru)              |   64       |   adam     |glove_cc_fix |83.96   |83.30  |00:38:54             |
|infersent(bilstm-last)      |   64       |   adam     |glove_cc_fix |84.27   |83.63  |01:21:38             |
|indersent(bigru-last)       |   64       |   adam     |glove_cc_fix |84.08   |83.64  |00:56:00             |
|infersent(bilstm-max)       |   64       |   adam     |glove_cc_fix |85.43   |85.01  |01:28:26             |
|infersent(bilstm-mean)      |   64       |   adam     |glove_cc_fix |83.62   |83.62  |01:14:32             |
|infersent(inner-attention)  |   64       |   adam     |glove_cc_fix |81.82   |82.23  |00:12:36             |
|infersent(hconv-net)        |   64       |   adam     |glove_cc_fix |83.46   |83.23  |00:24:36             |
|esim                        |   32       |adam(0.0005)|glove_cc_tune|87.55   |86.68  |11:03:31             |
|Siamese_BiLSTM              |   128      |   adam     |glove_cc_fix |83.47   |83.22  |06:41:44             |
|Siamese_CNN                 |   128      |   adam     |glove_cc_tune|82.57   |81.88  |00:33:51             |
|Siamese_IACNN               |            |            |             |



### Reference

- Part of my code are based on [mednil](https://github.com/jgc128/mednli). Great work, thanks!
