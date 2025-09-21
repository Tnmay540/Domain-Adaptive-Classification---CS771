# Binary Classification — Project II (CS771)



## Introduction
This project investigates adaptive learning across evolving datasets using CIFAR‑10 subsets with distinct distribution characteristics, focusing on sequential training that preserves performance on earlier data while adapting to new distributions.  
Task 1 covers progressive learning on datasets with similar input distributions and evaluates each model on its corresponding held‑out set, aiming to minimize degradation on earlier datasets as the sequence proceeds.  
Task 2 introduces varying distributions, requiring training strategies that handle distribution shifts while maintaining robustness across all held‑out datasets encountered so far.

## Task 1

### Model and Approach
- ResNet50 is used as a feature extractor due to its residual architecture and proven capability to learn hierarchical features transferable to smaller datasets like CIFAR‑10, enabling deeper features and noise rejection for classification.  
- On top of the 2048‑dimensional ResNet50 feature space, a prototypical network maps features to a 128‑dimensional embedding, trained to reduce intra‑class variance and increase inter‑class separation using Euclidean distance to classify unseen examples.  
- No rehearsal of earlier datasets is performed; only the current model parameters are updated per dataset, and the approach achieves an average accuracy of about 60% across datasets in Task 1.

### Results (Accuracy Matrix: f1…f10 on D1…D10)
| Model | D1 | D2 | D3 | D4 | D5 | D6 | D7 | D8 | D9 | D10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| f1 | 0.6224 |  |  |  |  |  |  |  |  |  |
| f2 | 0.6308 | 0.6248 |  |  |  |  |  |  |  |  |
| f3 | 0.6304 | 0.6276 | 0.6096 |  |  |  |  |  |  |  |
| f4 | 0.6276 | 0.6160 | 0.6056 | 0.6244 |  |  |  |  |  |  |
| f5 | 0.6200 | 0.6196 | 0.6040 | 0.6212 | 0.6272 |  |  |  |  |  |
| f6 | 0.6144 | 0.6252 | 0.5960 | 0.6224 | 0.6204 | 0.6140 |  |  |  |  |
| f7 | 0.6080 | 0.6236 | 0.6024 | 0.6176 | 0.6196 | 0.6080 | 0.6072 |  |  |  |
| f8 | 0.6076 | 0.6240 | 0.5948 | 0.6144 | 0.6208 | 0.6052 | 0.6028 | 0.5956 |  |  |
| f9 | 0.6076 | 0.6168 | 0.5940 | 0.6084 | 0.6224 | 0.6084 | 0.6028 | 0.5976 | 0.5956 |  |
| f10 | 0.6036 | 0.6152 | 0.5956 | 0.5984 | 0.6164 | 0.6008 | 0.5980 | 0.5952 | 0.5904 | 0.6072 |

## Task 2

### Approach
- Task 2 extends sequential learning to datasets from differing distributions and explores domain adaptation by aligning embeddings from the new dataset with those from the previously trained model (e.g., via MMD) and a prototype consistency regularizer.  
- Empirically, lower accuracies were observed on the new evaluation sets under these shifts, and the best overall results across the evaluation were obtained using the prototypical network trained on ResNet features.  
- Evaluation remains cumulative across held‑out splits, reporting performance as new datasets are introduced and the model is adapted sequentially.

### Results (f11…f20 on D1…D10)
| Model | D1 | D2 | D3 | D4 | D5 | D6 | D7 | D8 | D9 | D10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| f11 | 0.5840 | 0.5868 | 0.5832 | 0.5756 | 0.5960 | 0.5836 | 0.5740 | 0.5796 | 0.5744 | 0.5668 |
| f12 | 0.5712 | 0.5724 | 0.5656 | 0.5612 | 0.5816 | 0.5652 | 0.5564 | 0.5724 | 0.5560 | 0.5552 |
| f13 | 0.5672 | 0.5688 | 0.5616 | 0.5532 | 0.5748 | 0.5660 | 0.5524 | 0.5688 | 0.5536 | 0.5552 |
| f14 | 0.5668 | 0.5668 | 0.5604 | 0.5472 | 0.5748 | 0.5620 | 0.5492 | 0.5632 | 0.5464 | 0.5468 |
| f15 | 0.5640 | 0.5604 | 0.5588 | 0.5532 | 0.5660 | 0.5648 | 0.5456 | 0.5612 | 0.5488 | 0.5520 |
| f16 | 0.5512 | 0.5580 | 0.5388 | 0.5416 | 0.5552 | 0.5492 | 0.5372 | 0.5524 | 0.5408 | 0.5448 |
| f17 | 0.5444 | 0.5576 | 0.5500 | 0.5364 | 0.5556 | 0.5428 | 0.5288 | 0.5460 | 0.5416 | 0.5372 |
| f18 | 0.5356 | 0.5476 | 0.5400 | 0.5220 | 0.5404 | 0.5348 | 0.5236 | 0.5340 | 0.5292 | 0.5320 |
| f19 | 0.5356 | 0.5424 | 0.5376 | 0.5224 | 0.5372 | 0.5416 | 0.5208 | 0.5304 | 0.5232 | 0.5264 |
| f20 | 0.5300 | 0.5464 | 0.5436 | 0.5200 | 0.5356 | 0.5344 | 0.5232 | 0.5284 | 0.5280 | 0.5256 |

### Results (f11…f20 on D11…D20)
| Model | D11 | D12 | D13 | D14 | D15 | D16 | D17 | D18 | D19 | D20 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| f11 | 0.5228 | - | - | - | - | - | - | - | - | - |
| f12 | 0.5020 | 0.3668 | - | - | - | - | - | - | - | - |
| f13 | 0.4984 | 0.3644 | 0.4780 | - | - | - | - | - | - | - |
| f14 | 0.4980 | 0.3584 | 0.4716 | 0.5012 | - | - | - | - | - | - |
| f15 | 0.4944 | 0.3592 | 0.4688 | 0.4868 | 0.5688 | - | - | - | - | - |
| f16 | 0.4824 | 0.3716 | 0.4736 | 0.4820 | 0.5596 | 0.4504 | - | - | - | - |
| f17 | 0.4948 | 0.3704 | 0.4684 | 0.4828 | 0.5504 | 0.4332 | 0.4440 | - | - | - |
| f18 | 0.4740 | 0.3728 | 0.4684 | 0.4732 | 0.5424 | 0.4400 | 0.4404 | 0.4420 | - | - |
| f19 | 0.4620 | 0.3724 | 0.4664 | 0.4700 | 0.5384 | 0.4264 | 0.4396 | 0.4312 | 0.4736 | - |
| f20 | 0.4652 | 0.3708 | 0.4552 | 0.4700 | 0.5412 | 0.4188 | 0.4380 | 0.4300 | 0.4656 | 0.4788 |

## Related Work (Problem 2)
The LDAuCID approach addresses lifelong domain adaptation by consolidating internal distributions from prior tasks and aligning new task data to these distributions while mitigating catastrophic forgetting through reuse of informative past examples.  
This method demonstrates strong performance on standard benchmarks, enabling models to learn new tasks under shifting data while retaining prior knowledge, making it practical for real‑world non‑stationary settings.

## Acknowledgments
Gratitude is expressed to Professor Piyush Rai for guidance throughout the project and to the open‑source Python community and machine learning researchers whose tools and ideas supported this work.

## References
1. Rostami, M. (2021). “Lifelong Domain Adaptation via Consolidated Internal Distribution.” NeurIPS 34, 11172‑11183.  
2. He, K., Zhang, X., Ren, S., Sun, J. (2016). “Deep Residual Learning for Image Recognition.” CVPR, 770‑778.  
3. Bishop, C. M. (2006). “Pattern Recognition and Machine Learning.” Springer, 1st Edition.  
4. Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., & Duchesnay, E. (2011). “Scikit‑learn: Machine Learning in Python.” JMLR 12, 2825‑2830.  
5. Van Rossum, G., & Drake, F.L. (2009). Python 3 Reference Manual. CreateSpace.
