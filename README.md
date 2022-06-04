# Kaggle_BirdCLEF_2022
Bird species classification from sound recordings.  This year the competition was only scored on 21 specific birds of interest, located in Hawaii, where all the sound recordings were from.  Results format was true or false for each bird, for each 5 second time segment.  The recordings had no geo-reference, but some birds are endemic to specific islands, some are only found near the ocean, some inland, so there was the potential to do some interesting post processing with this competition.

I made good progress compared to last year with this, but was still a little disappointed with the performance on the private dataset (84% of the data).  I went backwards compared to one of my earlier models, which had less augmentation, and only one fold, when my final submissions ensembled 5 folds.

My two post-processing ideas in the inference notebook have some kind of bug, so are essentially untested. I should look into this further if the comp is similar next year. They produced scores around 50%, so completely random results.  They are currently not implemented.

Ideas for next time from high placed entries:
Over-train for all birds to a fixed number of epochs.  Then re-train on the small subset used for the competition.
Train different models for birds with larger datasets, and smaller datasets.
