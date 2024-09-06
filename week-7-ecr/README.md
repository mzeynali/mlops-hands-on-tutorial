
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

### Versioning data

Refer to the blog: [DVC Configuration](https://medium.com/@mzeynali01/week-3-how-to-use-dvc-for-machine-learning-model-management-c5b82b5dc9d0)

### Exporting model to ONNX

Once the model is trained, convert the model using the following command:

```
python convert_model_to_onnx.py
```

### Inference

#### Inference using standard pytorch

```
python inference.py
```

#### Inference using ONNX Runtime

```
python inference_onnx.py
```

## S3 & ECR

Follow the instructions mentioned in the [blog post](https://medium.com/@mzeynali01/week-7-automating-container-deployment-with-aws-ecr-and-github-actions-4a2f66f0268c) for creating S3 bucket and ECR repository. 

### Configuring dvc

```
dvc init (this has to be done at root folder)
dvc remote add -d model-store s3://models-dvc/trained_models/
```

### AWS credentials

Create the credentials as mentioned in the [blog post](https://medium.com/@mzeynali01/integrating-google-cloud-storage-and-aws-s3-with-dvc-0f014caf4e86)

**Do not share the secrets with others**

Set the ACCESS key and id values in environment variables.

```
export AWS_ACCESS_KEY_ID=<ACCESS KEY ID>
export AWS_SECRET_ACCESS_KEY=<ACCESS SECRET>
```

### Trained model in DVC

Sdd the trained model(onnx) to dvc using the following command:

```shell
cd dvcfiles
dvc add ../models/model.onnx --file trained_model.dvc
```

Push the model to remote storage

```shell
dvc push trained_model.dvc
```

### Docker

Install the docker using the [instructions here](https://docs.docker.com/engine/install/)

Build the image using the command

```shell
docker build -t mlops-hands-on-toturial:latest .
```

Then run the container using the command

```shell
docker run -p 8000:8000 --name inference_container mlops-hands-on-toturial:latest
```

(or)

Build and run the container using the command

```shell
docker-compose up
```

### Pushing the image to ECR

Follow the instructions mentioned in [blog post](https://medium.com/@mzeynali01/week-7-automating-container-deployment-with-aws-ecr-and-github-actions-4a2f66f0268c) for creating ECR repository.

- Authenticating docker client to ECR

```
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 246113150184.dkr.ecr.us-west-2.amazonaws.com
```

- Tagging the image

```
docker tag mlops-basics:latest 246113150184.dkr.ecr.us-west-2.amazonaws.com/mlops-hands-on-toturial:latest
```

- Pushing the image

```
docker push 246113150184.dkr.ecr.us-west-2.amazonaws.com/mlops-hands-on-toturial:latest
```

Refer to `.github/workflows/push_image_to_ecr.yaml` file for automatically creating the docker image with trained model and pushing it to ECR.


### Running notebooks

I am using [Jupyter lab](https://jupyter.org/install) to run the notebooks.

Since I am using a virtualenv, when I run the command `jupyter lab` it might or might not use the virtualenv.

To make sure to use the virutalenv, run the following commands before running `jupyter lab`

```
conda install ipykernel
python -m ipykernel install --user --name mlops
pip install ipywidgets
```