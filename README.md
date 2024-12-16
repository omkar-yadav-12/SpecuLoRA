# SpecuLoRA

There are 3 main jupyter notebooks in our git repository. Below is a description of what each of these notebooks accomplishes.

1) `Router.ipynb` is the notebook that is used to train the feed forward neural network used to dynamically choose the correct LoRA adapter.
2) `Adapter.ipynb` is an example notebook of how we trained our medical and python LoRA adapters. This notebook namely looks at the medical dataset/adapter
3) `Inference.ipynb` is our main notebook that runs our pipeline end to end. This runs inference on a list of queries and outputs analysis such as the throughput with/without LoRA adapters

## Installation / Set Up 

We have included a `requirements.txt` file that can be used to download all relevant packages needed to run our notebooks. We recommend the use of a virtual environment if the notebooks are going to be run locally. Note all of the experiments we ran were hosted on google colab so package management was handled for us. Follow the steps listed below for package installation:

1) Create a virtual environment: `python3 -m venv env`
2) Activate the virtual environment:
   1)  Mac/Linux: `source env/bin/activate`
   2)  Windows: `venv\Scripts\activate`
4) Install relevant packages: `pip3 install -r requirements.txt`
5) If any missing packages pop up simply install the package by running `pip3 install <PACKAGE_NAME>`

The other main component needed to run our code is a Hugging Face account. All of the pretrained models that we use are loaded in from Hugging Face so it is required that the user has their own account and user access token. More can be read about user access tokens here: https://huggingface.co/docs/hub/en/security-tokens. Once you have located your user access token, simply paste it at the beginning of the `Adapter.ipynb` and `Inference.ipynb` notebooks. The HUGGING_FACE_TOKEN has purposefully been left blank in both of these notebooks to be filled in by the user. 

## Training

There are two training notebooks that we used to create our pipeline. This section will summarize these notebooks at a high level. 

### `Router.ipynb`

As discussed in the paper, the router is a simple feed forward neural network that is used to deduce which router the given query should be sent to. This notebook is straightforward and easy to adapt to your specific needs. The general logic flow of the notebook is listed below:

1) Load in datasets that the LoRA adapters are trained on
2) Extract the questions and answers from the datasets
3) Run a bag of words vectorizer to transform the text input into numeric values
4) Create a dataset class used in the data loader
5) Train the model
6) Evaluate the model
7) Save the model and vectorizer

The router notebook can be run end to end assuming the package installation steps were run without any other interventions.

### `Adapter.ipynb`

This notebook serves as an example of how to train a LoRA adapter on your specific task or domain. As noted in the file, we test this specifically on the medical dataset and create a medical adapter. The general logic flow of the notebook is listed below:

1) Define all constants needed for datasets and Hugging Face
2) Load in dataset
3) Perform data preparation to extract the questions and answers
4) Log in to Hugging Face
5) Define LoRA configurations
6) Train the adapter
7) Save the adapter

The adapter training notebook can be run end to end with package installation and the integration of the Hugging Face token. 

## Inference

Finally, we have the inference notebook which displays our whole end to end system. This notebook serves as an example use case of the pipeline we built. The notebooks takes in a list of queries that are specific to the LoRA adapters the user trained and performs analysis on the difference between the SpecuLoRA framework and normal speculative decoding. The general logic flow of the notebook is listed below:

1) Define all constants needed for datasets and Hugging Face
2) Load in dataset(s)
3) Load in models and router
4) Perform LoRA inference by defining and running the following functions:
   1) Function to determine the correct adapter given the router and the input
   2) Function to run speculative decoding
   3) Function to process and analyze multiple queries
5) Perform non LoRA inference by defining and running the following functions:
   1) Function to run speculative decoding
   2) Function to process and analyze multiple queries

The inference notebook can be run end to end with package installation and the integration of the Hugging Face token. 

## Summary

We have provided three notebooks that define the SpecuLoRA pipeline that is discussed in our final report. Note we condensed the code found in these notebooks to make them more readable and easy to run. There were many failed experiments that we purposefully omitted to ensure that our repository contained only essential and executable code. Please note that we ran all of our experiments on Colab Pro which enabled us to have compute resources that were required to load in an 8B parameter model. Running these notebooks on a non GPU device is not feasible especially if the user is interested in training their own adapters. With these considerations, it is also possible to sub out the models and adapters we used to ones that are much smaller if the user is interested in running the pipeline with less compute requirements. 
