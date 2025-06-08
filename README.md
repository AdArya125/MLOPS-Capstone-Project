

# Sentiment Analysis - MLOps Capstone Project

This repository demonstrates a full-scale **MLOps pipeline** for a **Sentiment Analysis** model trained on the IMDB movie review dataset. It combines best practices from software engineering and machine learning to create a robust, reproducible, and deployable ML system.

---

## Tools & Technologies Used

| Category            | Tools/Services Used                                            |
| ------------------- | -------------------------------------------------------------- |
| Version Control     | Git, GitHub                                                    |
| Virtual Env         | Conda                                                          |
| Experiment Tracking | MLFlow (via Dagshub)                                           |
| Data Versioning     | DVC                                                            |
| Model Serving       | Flask API                                                      |
| CI/CD               | GitHub Actions                                                 |
| Containerization    | Docker, DockerHub                                              |
| Cloud Storage       | AWS S3 (for DVC remote), AWS ECR (for container image storage) |

---

## Setup Instructions

  ### 1. Initial Setup
  
  ```bash
  git clone https://github.com/AdArya125/MLOPS-Capstone-Project.git && cd MLOPS-Capstone-Project
  conda create -n atlas python=3.10 -y
  conda activate atlas
  pip install cookiecutter dagshub mlflow dvc dvc[s3] awscli flask pipreqs
  ```
  
  ### 2. Project Bootstrapping
  
  ```bash
  cookiecutter -c v1 https://github.com/drivendata/cookiecutter-data-science
  mv src/models src/model
  ```

---

## Experiment Tracking with Dagshub & MLFlow

1. Create a Dagshub repo and connect your GitHub repo.
2. Copy the MLFlow tracking URI and auth token.
3. Run experiment notebooks and track metrics via MLFlow.

```python
from dagshub import dagshub_logger
import mlflow

mlflow.set_tracking_uri("https://dagshub.com/<user>/<repo>.mlflow")
mlflow.set_experiment("sentiment-analysis")

with mlflow.start_run():
    ...
```

---

## DVC Pipeline Setup

```bash
dvc init
mkdir local_s3
dvc remote add -d mylocal local_s3
```

* Implement components inside `src/`:

  * `logger.py`
  * `data_ingestion.py`
  * `data_preprocessing.py`
  * `feature_engineering.py`
  * `model_building.py`
  * `model_evaluation.py`
  * `register_model.py`

* Add `dvc.yaml` and `params.yaml`.

```bash
dvc repro
dvc status
git add .
git commit -m "DVC and MLFlow tracking setup"
git push
```

---

## Cloud Storage with AWS S3

```bash
aws configure
dvc remote add -d myremote s3://<your-bucket-name>
```

Ensure your IAM user has `AmazonS3FullAccess`.

---

## Flask API for Inference

```bash
cd flask_app
pip install flask
python app.py
```

---

## CI/CD Pipeline (GitHub Actions)

* Add `.github/workflows/ci.yaml`
* Generate Dagshub token and save it in GitHub secrets as `CAPSTONE_TEST`
* Add AWS-related secrets:

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
AWS_ACCOUNT_ID
ECR_REPOSITORY
```

---

## Docker Setup

```bash
pipreqs flask_app --force
docker build -t capstone-app:latest .
docker run -p 8888:5000 -e CAPSTONE_TEST=your_token capstone-app:latest
```

(Optional):

```bash
docker push youruser/capstone-app:latest
```

---

## AWS ECR Deployment (Optional)

1. Create ECR repo in AWS.
2. Ensure IAM user has `AmazonEC2ContainerRegistryFullAccess`.
3. Add ECR build/push steps to CI pipeline.

---
## Testing

Use the `tests/` and `scripts/` directories for unit tests.

```bash
pytest tests/
```


## Acknowledgements

* [DrivenData's Cookiecutter Template](https://github.com/drivendata/cookiecutter-data-science)
* [Dagshub](https://dagshub.com/)
* [AWS S3 & ECR](https://aws.amazon.com/)
* [DVC](https://dvc.org/)
* [MLFlow](https://mlflow.org/)

---

## Contact

**Aditya Arya**
📧 [adarya125@gmail.com](mailto:adarya125@gmail.com)
🌐 [LinkedIn](https://www.linkedin.com/in/adarya125/)
🔗 [GitHub](https://github.com/AdArya125)

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

