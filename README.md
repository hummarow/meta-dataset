This repository contains accompanying code for the article introducing
Meta-Dataset, [arxiv.org/abs/1903.03096](https://arxiv.org/abs/1903.03096).
It also contains accompanying code and checkpoints for CrossTransformers,
[https://arxiv.org/abs/2007.11498](https://arxiv.org/abs/2007.11498), a
follow-up work which improves performance.

This code is provided here in order to give more details on the implementation
of the data-providing pipeline, our back-bones and models, as well as the
experimental setting.

See below for [user instructions](#user-instructions), including how to:

1.  [install](#installation) the software,
2.  [download and convert](#downloading-and-converting-datasets) the data, and
3.  [train](#training) implemented models.

See this
[introduction notebook](https://github.com/google-research/meta-dataset/blob/main/Intro_to_Metadataset.ipynb)
for a demonstration of how to sample data from the pipeline (episodes or
batches).

In order to run the experiments described in the first version of the arXiv
article, [arxiv.org/abs/1903.03096v1](https://arxiv.org/abs/1903.03096v1),
please use the instructions, code, and configuration files at version
[arxiv_v1](https://github.com/google-research/meta-dataset/tree/arxiv_v1) of
this repository.

We are currently working on updating the instructions, code, and configuration
files to reproduce the results in the second version of the article,
[arxiv.org/abs/1903.03096v2](https://arxiv.org/abs/1903.03096v2). You can follow
the progess in branch
[arxiv_v2_dev](https://github.com/google-research/meta-dataset/tree/arxiv_v2_dev)
of this repository.

This is not an officially supported Google product.

## Meta-Dataset: A Dataset of Datasets for Learning to Learn from Few Examples

_Eleni Triantafillou, Tyler Zhu, Vincent Dumoulin, Pascal Lamblin, Utku Evci,
Kelvin Xu, Ross Goroshin, Carles Gelada, Kevin Swersky, Pierre-Antoine Manzagol,
Hugo Larochelle_

Few-shot classification refers to learning a classifier for new classes given
only a few examples. While a plethora of models have emerged to tackle it, we
find the procedure and datasets that are used to assess their progress lacking.
To address this limitation, we propose Meta-Dataset: a new benchmark for
training and evaluating models that is large-scale, consists of diverse
datasets, and presents more realistic tasks. We experiment with popular
baselines and meta-learners on Meta-Dataset, along with a competitive method
that we propose. We analyze performance as a function of various characteristics
of test tasks and examine the models' ability to leverage diverse training
sources for improving their generalization. We also propose a new set of
baselines for quantifying the benefit of meta-learning in Meta-Dataset. Our
extensive experimentation has uncovered important research challenges and we
hope to inspire work in these directions.

## CrossTransformers: spatially-aware few-shot transfer

_Carl Doersch, Ankush Gupta, Andrew Zisserman_

This is a Transformer-based neural network architecture which can find coarse
spatial correspondence between the query and the support images, and then infer
class membership by computing distances between spatially-corresponding
features.  The paper also introduces SimCLR episodes, which are episodes that
require SimCLR-style instance recognition, and therefore encourage features
which capture more than just the training-set categories.  This algorithm is
SOTA on Meta-Dataset (train-on-ILSVRC) as of NeurIPS 2020.

Configuration files for CrossTransformers with and without SimCLR episodes (CTX
and CTX+SimCLR Eps from the paper) can be found in
`learn/gin/default/crosstransformer*`.  We also have pretrained checkpoints for
these two configurations:
[CTX](https://storage.googleapis.com/dm_crosstransformer/ctx.zip),
and
[CTX+SimCLR Eps](https://storage.googleapis.com/dm_crosstransformer/ctx_simclreps.zip),
as well as
[CTX+SimCLR Eps+BOHB Aug](https://storage.googleapis.com/dm_crosstransformer/ctx_simclreps_bohbaug.zip).
Note that these were retrained from the versions reported in the paper, but
their performance should be on-par.  The network structure is the same for all
three models, and so they can be loaded using either of the CrossTransformer
config files.

# Leaderboard (in progress)

The tables below were generated by
[this notebook](https://github.com/google-research/meta-dataset/blob/main/Leaderboard.ipynb).

## Adding a new model to the leaderboard

1.  Gather accuracy results and 95% confidence intervals, as well as the number
    of episodes used for the CI (minimum 600).
2.  If you were affected by
    [#54](https://github.com/google-research/meta-dataset/issues/54),
    make sure the evaluation on Traffic Sign is done on shuffled samples. We
    encourage you to re-train your best model (or at least perform validation
    again) as well.
3.  Create an
    [issue](https://github.com/google-research/meta-dataset/issues/new),
    with the name of the model, results, as well as the article to cite or any
    other relevant information to include, and label it "leaderboard".
    Alternatively, submit a PR with an update to the notebook.

<!-- Beginning of content generated by `Leaderboard.ipynb` -->

## Training on ImageNet only

Method                     |Avg rank                   |ILSVRC (test)              |Omniglot                   |Aircraft                   |Birds                      |Textures                   |QuickDraw                  |Fungi                      |VGG Flower                 |Traffic signs              |MSCOCO                     
---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------
k-NN [[1]]                 |9.7                        |41.03±1.01&nbsp;(10)       |37.07±1.15&nbsp;(11)       |46.81±0.89&nbsp;(10)       |50.13±1.00&nbsp;(10.5)     |66.36±0.75&nbsp;(8)        |32.06±1.08&nbsp;(11)       |36.16±1.02&nbsp;(8)        |83.10±0.68&nbsp;(8)        |44.59±1.19&nbsp;(10)       |30.38±0.99&nbsp;(10.5)     
Finetune [[1]]             |6.15                       |45.78±1.10&nbsp;(8)        |60.85±1.58&nbsp;(6.5)      |68.69±1.26&nbsp;(2)        |57.31±1.26&nbsp;(9)        |69.05±0.90&nbsp;(4.5)      |42.60±1.17&nbsp;(8.5)      |38.20±1.02&nbsp;(7)        |85.51±0.68&nbsp;(6)        |66.79±1.31&nbsp;(2)        |34.86±0.97&nbsp;(8)        
MatchingNet [[1]]          |8.65                       |45.00±1.10&nbsp;(8)        |52.27±1.28&nbsp;(9)        |48.97±0.93&nbsp;(9)        |62.21±0.95&nbsp;(7.5)      |64.15±0.85&nbsp;(10)       |42.87±1.09&nbsp;(8.5)      |33.97±1.00&nbsp;(9)        |80.13±0.71&nbsp;(10)       |47.80±1.14&nbsp;(7.5)      |34.99±1.00&nbsp;(8)        
ProtoNet [[1]]             |6.45                       |50.50±1.08&nbsp;(4.5)      |59.98±1.35&nbsp;(6.5)      |53.10±1.00&nbsp;(7.5)      |68.79±1.01&nbsp;(5.5)      |66.56±0.83&nbsp;(8)        |48.96±1.08&nbsp;(6)        |39.71±1.11&nbsp;(6)        |85.27±0.77&nbsp;(6)        |47.12±1.10&nbsp;(9)        |41.00±1.10&nbsp;(5.5)      
fo-MAML [[1]]              |7.45                       |45.51±1.11&nbsp;(8)        |55.55±1.54&nbsp;(8)        |56.24±1.11&nbsp;(5.5)      |63.61±1.06&nbsp;(7.5)      |68.04±0.81&nbsp;(4.5)      |43.96±1.29&nbsp;(8.5)      |32.10±1.10&nbsp;(10)       |81.74±0.83&nbsp;(9)        |50.93±1.51&nbsp;(5.5)      |35.30±1.23&nbsp;(8)        
RelationNet [[1]]          |10.55                      |34.69±1.01&nbsp;(11)       |45.35±1.36&nbsp;(10)       |40.73±0.83&nbsp;(11)       |49.51±1.05&nbsp;(10.5)     |52.97±0.69&nbsp;(11)       |43.30±1.08&nbsp;(8.5)      |30.55±1.04&nbsp;(11)       |68.76±0.83&nbsp;(11)       |33.67±1.05&nbsp;(11)       |29.15±1.01&nbsp;(10.5)     
fo-Proto-MAML [[1]]        |5.2                        |49.53±1.05&nbsp;(6)        |63.37±1.33&nbsp;(4.5)      |55.95±0.99&nbsp;(5.5)      |68.66±0.96&nbsp;(5.5)      |66.49±0.83&nbsp;(8)        |51.52±1.00&nbsp;(4.5)      |39.96±1.14&nbsp;(3.5)      |87.15±0.69&nbsp;(3)        |48.83±1.09&nbsp;(7.5)      |43.74±1.12&nbsp;(4)        
ALFA+fo-Proto-MAML [[3]]   |3.25                       |52.80±1.11&nbsp;(2.5)      |61.87±1.51&nbsp;(4.5)      |63.43±1.10&nbsp;(3)        |69.75±1.05&nbsp;(3.5)      |70.78±0.88&nbsp;(2)        |59.17±1.16&nbsp;(2)        |41.49±1.17&nbsp;(3.5)      |85.96±0.77&nbsp;(6)        |60.78±1.29&nbsp;(3)        |48.11±1.14&nbsp;(2.5)      
ProtoNet (large) [[4]]     |3.45                       |53.69±1.07&nbsp;(2.5)      |68.50±1.27&nbsp;(2.5)      |58.04±0.96&nbsp;(4)        |74.07±0.92&nbsp;(2)        |68.76±0.77&nbsp;(4.5)      |53.30±1.06&nbsp;(3)        |40.73±1.15&nbsp;(3.5)      |86.96±0.73&nbsp;(3)        |58.11±1.05&nbsp;(4)        |41.70±1.08&nbsp;(5.5)      
CTX [[4]]                  |**1**                      |**62.76**±0.99&nbsp;(1)    |**82.21**±1.00&nbsp;(1)    |**79.49**±0.89&nbsp;(1)    |**80.63**±0.88&nbsp;(1)    |**75.57**±0.64&nbsp;(1)    |**72.68**±0.82&nbsp;(1)    |**51.58**±1.11&nbsp;(1)    |**95.34**±0.37&nbsp;(1)    |**82.65**±0.76&nbsp;(1)    |**59.90**±1.02&nbsp;(1)    
BOHB [[5]]                 |4.15                       |51.92±1.05&nbsp;(4.5)      |67.57±1.21&nbsp;(2.5)      |54.12±0.90&nbsp;(7.5)      |70.69±0.90&nbsp;(3.5)      |68.34±0.76&nbsp;(4.5)      |50.33±1.04&nbsp;(4.5)      |41.38±1.12&nbsp;(3.5)      |87.34±0.59&nbsp;(3)        |51.80±1.04&nbsp;(5.5)      |48.03±0.99&nbsp;(2.5)      

## Training on all datasets

Method                     |Avg rank                   |ILSVRC (test)              |Omniglot                   |Aircraft                   |Birds                      |Textures                   |QuickDraw                  |Fungi                      |VGG Flower                 |Traffic signs              |MSCOCO                     
---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------
k-NN [[1]]                 |5.95                       |38.55±0.94&nbsp;(5.5)      |74.60±1.08&nbsp;(7)        |64.98±0.82&nbsp;(8)        |66.35±0.92&nbsp;(3.5)      |63.58±0.79&nbsp;(5)        |44.88±1.05&nbsp;(8)        |37.12±1.06&nbsp;(4.5)      |83.47±0.61&nbsp;(5)        |40.11±1.10&nbsp;(7)        |29.55±0.96&nbsp;(6)        
Finetune [[1]]             |4.4                        |43.08±1.08&nbsp;(3.5)      |71.11±1.37&nbsp;(8)        |72.03±1.07&nbsp;(4.5)      |59.82±1.15&nbsp;(6)        |**69.14**±0.85&nbsp;(1.5)  |47.05±1.16&nbsp;(7)        |38.16±1.04&nbsp;(4.5)      |85.28±0.69&nbsp;(4)        |**66.74**±1.23&nbsp;(1)    |35.17±1.08&nbsp;(4)        
MatchingNet [[1]]          |5.8                        |36.08±1.00&nbsp;(7)        |78.25±1.01&nbsp;(5.5)      |69.17±0.96&nbsp;(6.5)      |56.40±1.00&nbsp;(7)        |61.80±0.74&nbsp;(6)        |60.81±1.03&nbsp;(4.5)      |33.70±1.04&nbsp;(7)        |81.90±0.72&nbsp;(6)        |55.57±1.08&nbsp;(2.5)      |28.79±0.96&nbsp;(6)        
ProtoNet [[1]]             |3.7                        |44.50±1.05&nbsp;(3.5)      |79.56±1.12&nbsp;(5.5)      |71.14±0.86&nbsp;(4.5)      |67.01±1.02&nbsp;(3.5)      |65.18±0.84&nbsp;(3.5)      |64.88±0.89&nbsp;(3)        |40.26±1.13&nbsp;(3)        |86.85±0.71&nbsp;(3)        |46.48±1.00&nbsp;(5)        |39.87±1.06&nbsp;(2.5)      
fo-MAML [[1]]              |5.15                       |37.83±1.01&nbsp;(5.5)      |83.92±0.95&nbsp;(3.5)      |76.41±0.69&nbsp;(2)        |62.43±1.08&nbsp;(5)        |64.16±0.83&nbsp;(3.5)      |59.73±1.10&nbsp;(6)        |33.54±1.11&nbsp;(7)        |79.94±0.84&nbsp;(7)        |42.91±1.31&nbsp;(6)        |29.37±1.08&nbsp;(6)        
RelationNet [[1]]          |6.8                        |30.89±0.93&nbsp;(8)        |86.57±0.79&nbsp;(2)        |69.71±0.83&nbsp;(6.5)      |54.14±0.99&nbsp;(8)        |56.56±0.73&nbsp;(8)        |61.75±0.97&nbsp;(4.5)      |32.56±1.08&nbsp;(7)        |76.08±0.76&nbsp;(8)        |37.48±0.93&nbsp;(8)        |27.41±0.89&nbsp;(8)        
fo-Proto-MAML [[1]]        |2.25                       |46.52±1.05&nbsp;(2)        |82.69±0.97&nbsp;(3.5)      |75.23±0.76&nbsp;(3)        |69.88±1.02&nbsp;(2)        |**68.25**±0.81&nbsp;(1.5)  |66.84±0.94&nbsp;(2)        |41.99±1.17&nbsp;(2)        |**88.72**±0.67&nbsp;(1.5)  |52.42±1.08&nbsp;(4)        |**41.74**±1.13&nbsp;(1)    
CNAPs [[2]]                |**1.95**                   |**50.80**±1.10&nbsp;(1)    |**91.70**±0.50&nbsp;(1)    |**83.70**±0.60&nbsp;(1)    |**73.60**±0.90&nbsp;(1)    |59.50±0.70&nbsp;(7)        |**74.70**±0.80&nbsp;(1)    |**50.20**±1.10&nbsp;(1)    |**88.90**±0.50&nbsp;(1.5)  |56.50±1.10&nbsp;(2.5)      |39.40±1.00&nbsp;(2.5)      

## References

[1]: #1-triantafillou-et-al-2020
[2]: #2-requeima-et-al-2019
[3]: #3-baik-et-al-2020
[4]: #4-doersch-et-al-2020
[5]: #5-saikia-et-al-2020

###### \[1\] Triantafillou et al. (2020)

Eleni Triantafillou, Tyler Zhu, Vincent Dumoulin, Pascal Lamblin, Utku Evci, Kelvin Xu, Ross Goroshin, Carles Gelada, Kevin Swersky, Pierre-Antoine Manzagol, Hugo Larochelle; [_Meta-Dataset: A Dataset of Datasets for Learning to Learn from Few Examples_](https://arxiv.org/abs/1903.03096); ICLR 2020.


###### \[2\] Requeima et al. (2019)

James Requeima, Jonathan Gordon, John Bronskill, Sebastian Nowozin, Richard E. Turner; [_Fast and Flexible Multi-Task Classification Using Conditional Neural Adaptive Processes_](https://arxiv.org/abs/1906.07697); NeurIPS 2019.


###### \[3\] Baik et al. (2020)

Sungyong Baik, Myungsub Choi, Janghoon Choi, Heewon Kim, Kyoung Mu Lee; [_Meta-Learning with Adaptive Hyperparameters_](https://papers.nips.cc/paper/2020/hash/ee89223a2b625b5152132ed77abbcc79-Abstract.html); NeurIPS 2020.


###### \[4\] Doersch et al. (2020)

Carl Doersch, Ankush Gupta, Andrew Zisserman; [_CrossTransformers: spatially-aware few-shot transfer_](https://arxiv.org/abs/2007.11498); NeurIPS 2020.


###### \[5\] Saikia et al. (2020)

Tonmoy Saikia, Thomas Brox, Cordelia Schmid; [_Optimized Generic Feature Learning for Few-shot Classification across Domains_](https://arxiv.org/abs/2001.07926); arXiv 2020.


<!-- End of content generated by `Leaderboard.ipynb` -->

# User instructions

## Installation

Meta-Dataset is generally compatible with Python 2 and Python 3, but some parts
of the code may require Python 3. The code works with TensorFlow 2, although it
makes extensive use of `tf.compat.v1` internally.

-   We recommend you follow
    [these instructions](https://www.tensorflow.org/install/pip) to install
    TensorFlow.
-   A list of packages to install is available in `requirements.txt`, you can
    install them using `pip`.
-   Clone the `meta-dataset` repository. Most command lines start with `python
    -m meta_dataset.<something>`, and should be typed from within that clone
    (where a `meta_dataset` Python module should be visible).
-   To reproduce the CrossTransformers training, you will need data augmentation
    code from [simclr](https://github.com/google-research/simclr), which is
    autimatically downloaded by `setup.py`.

## Downloading and converting datasets

Meta-Dataset uses several established datasets, that are available from
different sources. You can find below a summary of these datasets, as well as
instructions to download them and convert them into a common format.

For brevity of the command line examples, we assume the following environment
variables are defined:

-   `$DATASRC`: root of where the original data is downloaded and potentially
    extracted from compressed files. This directory does not need to be
    available after the data conversion is done.
-   `$SPLITS`: directory where `*_splits.json` files will be created, one per
    dataset. For instance, `$SPLITS/fungi_splits.json` contains information
    about which classes are part of the meta-training, meta-validation, and
    meta-test set. These files are only used during the dataset conversion
    phase, but can help troubleshooting later. To re-use the
    [canonical splits](https://github.com/google-research/meta-dataset/tree/main/meta_dataset/dataset_conversion/splits)
    instead of re-generating them, you can make it point to
    `meta_dataset/dataset_conversion` in your checkout.
-   `$RECORDS`: root directory that will contain the converted datasets (one per
    sub-directory). This directory needs to be available during training and
    evaluation.

### Dataset summary

Dataset (other names)                                                                                                                        | Number of classes (train/valid/test)    | Size on disk                 | Conversion time
-------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ---------------------------- | ---------------
ilsvrc\_2012 (ImageNet, ILSVRC) \[[instructions](doc/dataset_conversion.md#ilsvrc_2012)\]                                                  | 1000 (712/158/130, hierarchical)        | \~140 GiB                    | 5 to 13 hours
omniglot \[[instructions](doc/dataset_conversion.md#omniglot)\]                                                                            | 1623 (883/81/659, by alphabet: 25/5/20) | \~60 MiB                     | few seconds
aircraft (FGVC-Aircraft) \[[instructions](doc/dataset_conversion.md#aircraft)\]                                                            | 100 (70/15/15)                          | \~470 MiB (2.6 GiB download) | 5 to 10 minutes
cu\_birds (Birds, CUB-200-2011) \[[instructions](doc/dataset_conversion.md#cu_birds)\]                                                     | 200 (140/30/30)                         | \~1.1 GiB                    | \~1 minute
dtd (Describable Textures, DTD) \[[instructions](doc/dataset_conversion.md#dtd)\]                                                          | 47 (33/7/7)                             | \~600 MiB                    | few seconds
quickdraw (Quick, Draw!) \[[instructions](doc/dataset_conversion.md#quickdraw)\]                                                           | 345 (241/52/52)                         | \~50 GiB                     | 3 to 4 hours
fungi (FGVCx Fungi) \[[instructions](doc/dataset_conversion.md#fungi)\]                                                                    | 1394 (994/200/200)                      | \~13 GiB                     | 5 to 15 minutes
vgg\_flower (VGG Flower) \[[instructions](doc/dataset_conversion.md#vgg_flower)\]                                                          | 102 (71/15/16)                          | \~330 MiB                    | \~1 minute
traffic\_sign (Traffic Signs, German Traffic Sign Recognition Benchmark, GTSRB) \[[instructions](doc/dataset_conversion.md#traffic_sign)\] | 43 (0/0/43, test only)                  | \~50 MiB (263 MiB download)  | \~1 minute
mscoco (Common Objects in Context, COCO) \[[instructions](doc/dataset_conversion.md#mscoco)\]                                              | 80 (0/40/40, validation and test only)  | \~5.3 GiB (18 GiB download)  | 4 hours
*Total (All datasets)*                                                                                                                       | *4934 (3144/598/1192)*                  | *\~210 GiB*                  | *12 to 24 hours*

## Training

Experiments are defined via [gin](google/gin-config) configuration files, that
are under `meta_dataset/learn/gin/`:

-   `setups/` contain generic setups for classes of experiment, for instance
    which datasets to use (`imagenet` or `all`), parameters for sampling the
    number of ways and shots of episodes.
-   `models/` define settings for different meta-learning algorithms (baselines,
    prototypical networks, MAML...)
-   `default/` contains files that each correspond to one experiment, mostly
    defining a setup and a model, with default values for training
    hyperparameters.
-   `best/` contains files with values for training hyperparameters that
    achieved the best performance during hyperparameter search.

There are three main architectures, also called "backbones" (or "embedding
networks"): `four_layer_convnet` (sometimes `convnet` for short), `resnet`, and
`wide_resnet`. These architectures can be used by all baselines and episodic
models. Another backbone, `relationnet_convnet` (similar to `four_layer_convnet`
but without pooling on the last layer), is only used by RelationNet (and
baseline, for pre-training purposes).  CrossTransformers use a larger backbone
`resnet34`, which is similar to `resnet` but with more layers.

### Reproducing results

See [Reproducing best results](doc/reproducing_best_results.md) for
instructions to launch training experiments with the values of hyperparameters
that were selected in the paper. The hyperparameters (including the backbone,
whether to train from scratch or from pre-trained weights, and the number of
training updates) were selected using only the validation classes of the ILSVRC
2012 dataset for all experiments. Even when training on "all" datasets, the
validation classes of the other datasets were not used.
