# Autocoding Injury Narratives with BERT
This repo contains code for training an ensemble of BERT models to autocode injury narratives. 

## Model
BERT stands for Bidirectional Encoder Representations from Transformers. One of the newer large-scale contextual language models, it's a good baseline for a wide variety of downstream NLP tasks. To learn more about how the base model is trained, check out the paper on [arXiv](https://arxiv.org/abs/1810.04805). To see how folks from Google implemented it in TensorFlow, check out the original [repo](https://github.com/google-research/bert) on GitHub, which we've also included here (but not updated in while, so you may want to pull down a fresh copy).

## Task
Our task here was to classify free-text injury narratives using [OIICS](https://wwwn.cdc.gov/wisards/oiics/Trees/MultiTree.aspx?Year=2012) event codes. The project was for a Kaggle-like competition internal to CDC, which you can read about [here](https://www.cdc.gov/od/science/technology/innovation/innovationfund.htm). In our data, there were 47 classifiable event codes distributed across 7 categories:

  1. Violence and other injuries by persons and animals
  2. Transportation incidents
  3. Fires and explosions
  4. Falls, slips, and trips
  5. Exposure to harmful substances or environments
  6. Contact with objects and equipment
  7. Overexertion and bodily reaction

Events not belonging to one of these categories were marked with with code 99, making for a grand total of 48 event codes. Since each narrative only receives a single code, we formulated the problem as one of multiclass classification.

## Code
The main scripts are the two ```.bat``` files. To fine-tune the base BERT checkpoint on your own data, train the model with ```train.bat```, and then update the ckeckpoint number in the call to ```bert\run_classifer.py``` in ```test.bat``` to reflect the last checkpoint saved during training. To use our fine-tuned checkpoints to get predictions on your own data, simply run ```test.bat```, leaving the checkpoint number the same. In both cases, you'll want to have run ```src\preprocessing.py``` on your raw text files, which should have the structure outlined in the Data section below. 

## Data
To download a copy of the data directory we reference in our code, including the small base BERT model we fine-tuned to classify the narratives, head [here](https://www.dropbox.com/s/10iu4rslh6pre81/injury_autocoding.zip?dl=1). Once you've upzipped the file, you'll see a directory with a BERT folder, two CSV files with information about the injury codes, and a few empty folders for holding the individual model checkpoints that go into our final ensemble. The next step is will be to put your raw text files in the new directory so that ```src\preprocessing.py``` has something to work with. Your files should be in ```.csv``` format, with the following names and columns:

  1. ```train.csv```: 'id', 'text', 'event'
  2. ```test.csv```: 'id', 'text'

If your narratives don't already have an 'id' column with unique record identifiers, our script will generate one during the preprocessing steps/
