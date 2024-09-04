
**Note: The purpose of the project to explore the libraries and learn how to use them. Not to build a SOTA model.**

## Requirements:

This project uses Python 3.11

Create a virtual env with the following command:

```
conda create --name mlops python=3.11
conda activate mlops
```

Install the requirements:

```
pip install -r requirements.txt
```

## Running

### Training

After installing the requirements, in order to train the model simply run:

```
python train.py
```

### Monitoring

Once the training is completed in the end of the logs you will see something like:

```
wandb: Synced 5 W&B file(s), 4 media file(s), 3 artifact file(s) and 0 other file(s)
wandb:
wandb: Synced proud-mountain-77: https://wandb.ai/mzeynali/mlops%20basic%20tutorial/runs/5mz1dkmc
```

Follow the link to see the wandb dashboard which contains all the plots.

### Inference

After training, update the model checkpoint path in the code and run

```
python inference.py
```

### Versioning data

Refer to the blog: [DVC Configuration]https://medium.com/@mzeynali01/week-3-how-to-use-dvc-for-machine-learning-model-management-c5b82b5dc9d0)

### Running notebooks

I am using [Jupyter lab](https://jupyter.org/install) to run the notebooks.

Since I am using a virtualenv, when I run the command `jupyter lab` it might or might not use the virtualenv.

To make sure to use the virutalenv, run the following commands before running `jupyter lab`

```
conda install ipykernel
python -m ipykernel install --user --name mlops
pip install ipywidgets
```