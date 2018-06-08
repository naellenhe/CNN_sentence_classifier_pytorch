Reference to [Convolutional Neural Networks for Sentence Classification](https://arxiv.org/pdf/1408.5882v2.pdf) and implemented with PyTorch. 


6/6/2018: 
Test Accuracy of the model on the test dataset: 81.83 %

### Dataset: Movie review
http://www.cs.cornell.edu/people/pabo/movie-review-data/
sentence polarity dataset v1.0 (includes sentence polarity dataset README v1.0: 5331 positive and 5331 negative processed sentences / snippets. Introduced in Pang/Lee ACL 2005. Released July 2005.


### Input data size:
sentence to vector (64, 300)


### Model:

```
Network
Input ->
Conv -> ReLU -> MaxPool |
Conv -> ReLU -> MaxPool | -> concat
Conv -> ReLU -> MaxPool |
Fully Connected Layer(Logits -> Softmax) -> Labels
```

```
Convolutional layer formula:
- Filter 1(Kernel) size K = 3 => (3 x 300)
- P(same padding) P = (3-1)/2=1
- S(stride) S = 1
- in_channels = 1
- out_channels (int) – Number of channels produced by the convolution = 100
Pooling layer formula:
- K
```

```
*Filter dimensions*:
Conv1 (W_conv, (100, 1, 3, 300))
Conv1 (b_conv, (100,))
Conv2 (W_conv, (100, 1, 4, 300))
Conv2 (b_conv, (100,))
Conv3 (W_conv, (100, 1, 5, 300))
Conv3 (b_conv, (100,))

*Layer input dimensions*:
- Input image(64, 300) 

----------------------------------------------------------------------
|  Conv1  (100, 3, 300)   Conv2  (100, 4, 300)   Conv3  (100, 5, 300) |
|  MaxPool (100, 62, 1)   MaxPool (100, 61, 1)   MaxPool (100, 60, 1) |
-----------------------------Concat ----------------------------------

- Concatenated (100, 1, 1) + (100, 1, 1) + (100, 1, 1) => (300, 1, 1) 

- Fully Connected Layer(Logits (100, 1) -> Logits (2, 1) -> Softmax) -> Labels
```

### Test memo:

#### Model 1
|mode/vec|dataset size(train/test)|#epoch|      model  |  optimizer | parameters |lr| accuracy  |
|-----|-----------------------|-------|--------------|------------|------------|--|------------|
|-nonstatic -rand|9572/1100|20|(conv- relu - pool), (linear)x2 |torch.optim.Adam|-|0.01| 77.15%|
|-nonstatic -rand|9572/1100|20|(conv- relu - pool), (linear-relu)x2 |torch.optim.Adam|Kaiming He|0.01| 77.52%|
|-nonstatic -rand|9572/1100|20|(conv- relu - pool), (linear-relu)x2 |torch.optim.Adam|Kaiming He|0.001| 80.27%|
|-nonstatic -word2vec|9572/1100|20|(conv- relu - pool), (linear-relu)x2 |torch.optim.Adam|Kaiming He|0.001| 81.83%|

