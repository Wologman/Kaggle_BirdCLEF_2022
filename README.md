# Kaggle_BirdCLEF_2022

Predict Bird species from sound recordings - [Competition web page](https://www.kaggle.com/competitions/birdclef-2022)

This year the competition was only scored on 21 specific birds of interest, located in Hawaii, where all the sound recordings were from.  Results format was true or false for each bird, for each 5 second time segment.  The recordings had no geo-reference, but some birds are endemic to specific islands, some are only found near the ocean, some inland, so there was the potential to do some interesting post processing with this competition.

I made good progress compared to last year with this, but was still a little disappointed with the performance on the private dataset (84% of the data).  I went backwards compared to one of my earlier models, which had less augmentation, and only one fold, when my final submissions ensembled 5 folds.  I should have thought more carefully about the key differences from the previous year, and made larger changes to the training and inference notebooks.  It made a big difference only searching for the 21 scored birds out of a much larger training set.  And the evaluation metric was quite different also.

A key mistake was probably to base the CV scheme on all the birds in the training dataset, since the leaderboard was only calculated on the 21 birds of interest.  My CV scheme was not corresponding well to the leaderboard.

My two post-processing ideas in the inference notebook have some kind of bug, so are essentially untested. I should look into this further if the comp is similar next year. They produced scores around 50%, so completely random results.  They are currently not implemented.  The 11th placed solution had success with the idea of biasing the scores in other time segments when there was a high probability (>90%) of a particular bird in that segment.  So I should perservere with this concept next time.

Ideas for next time from high placed entries:   
[1st place](https://www.kaggle.com/competitions/birdclef-2022/discussion/327047)  
[3rd place](https://www.kaggle.com/competitions/birdclef-2022/discussion/327193)  
[4th place](https://www.kaggle.com/competitions/birdclef-2022/discussion/326987)

It was interesting to note that with these better models, some high scoring notebooks benefited from much much higher thresholds.  So important to focus on improving the model before fussing too much over the threshold!


* Pre-train for all birds to a fixed number of epochs (improves generalisation as the training is on a much larger dataset) on 2021 & 2022 datasets.  Then fine tune on the small subset used for the competition (in primary or secondary label), with a more specific CV metric.

* Post process: audio was slided by some offset (in their case 1.5 seconds forward and backward) and then aggregated as follows: p = 0.5*p0 + 0.25*pr + 0.25*pl   [7th place solution](https://www.kaggle.com/competitions/birdclef-2022/discussion/326979)

* Finetuning with weighted sampling (w~N^0.5)

* Mixup Augmentation + pink, brown, or gaussian noise, external bird-free noise clips

* window size 1024

*  Train different models for birds with larger datasets, and smaller datasets.

* BCE loss for bird classes with more than 10 samples, BCE with folcal-loss, for birds with < 10 samples

* Other high scoring backbones included: tf_efficientnet_b3_ns, eca_nfnet_10, ResNeXt50, MiT-B2 transformer, seresnext26t_32x4d, resnet34, resnest50,  tf_efficientnetv2s

* Ensemble several models, one of them being birdnet (Winning entry)

* Constant Q-transform instead of mel-scale spectrogram  [7th place solution](https://www.kaggle.com/competitions/birdclef-2022/discussion/326973)

* Use long and short clipwise predictions with an AND rule, 5 seconds and 15 seconds, or use a 15 second clip for the whole model, whilst applying the head only to a centred 5 second chunk.

* Use different thresholds for different birds, to cope with the large class imbalance.
