<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  
</head>
<body>

<h1>Vehicle Insurance Intelligence: End‑to‑End MLOps on AWS</h1>

<p>
This project implements an end-to-end <strong>MLOps</strong> workflow for a vehicle insurance machine learning system, focusing on premium and claim-risk prediction with local development, data engineering, model training, and AWS-based CI/CD deployment.
</p>

<h2>Overview</h2>
<p>
The repository demonstrates how to structure a production-grade Python ML project with modular components, environment management, data pipelines, experiment tracking, and cloud deployment for insurance analytics.
It uses MongoDB Atlas for storing policy and claim data, AWS services (S3, EC2, ECR, IAM) for model registry and deployment, and Docker plus GitHub Actions for automated continuous integration and delivery.
</p>

<ul>
  <li>End-to-end pipeline: from raw policy and claim records to live insurance premium and claim-risk predictions.</li>
  <li>Production-ready patterns: configuration-driven design, custom logging, exception handling, and modular components.</li>
  <li>Cloud-native deployment: containerized app deployed to AWS EC2 with an ECR-backed CI/CD pipeline.</li>
</ul>

<h2>Key Features</h2>
<ul>
  <li><strong>Project scaffolding</strong>: Auto-generated template using <code>template.py</code>, with proper Python packaging via <code>setup.py</code> and <code>pyproject.toml</code> to support local imports and reusable modules.</li>
  <li><strong>Environment &amp; dependencies</strong>: Reproducible environment using a dedicated Conda env (<code>vehicle</code>) and pinned dependencies through <code>requirements.txt</code>.</li>
  <li><strong>Insurance data lake on MongoDB Atlas</strong>: Cloud-hosted database for storing and serving raw policyholder, vehicle, and claim datasets, accessed securely via connection strings and environment variables.</li>
  <li><strong>Custom logging &amp; exception layer</strong>: Centralized logger and domain-specific exception classes integrated into the pipeline for observability and debuggability.</li>
  <li><strong>Modular data pipeline</strong>: Clear separation of Data Ingestion, Data Validation, Data Transformation, Model Trainer, Model Evaluation, and Model Pusher components tailored for insurance data.</li>
  <li><strong>Model registry on S3</strong>: Versioned premium/claim models stored in an S3 bucket (<code>my-model-mlopsproj</code>) with configurable S3 keys and thresholds for evaluation.</li>
  <li><strong>Prediction API</strong>: <code>app.py</code> exposes HTTP endpoints for online vehicle insurance premium and claim-risk inference, plus a training route for on-demand retraining.</li>
  <li><strong>Docker &amp; CI/CD</strong>: Dockerfile, GitHub Actions workflow, and self-hosted runner on EC2 enable automated build, push to ECR, and deployment.</li>
</ul>

<h2>Tech Stack</h2>
<ul>
  <li><strong>Domain</strong>: Vehicle insurance premium and claim-risk prediction.</li>
  <li><strong>Language</strong>: Python 3.10</li>
  <li><strong>Data storage</strong>: MongoDB Atlas (managed cloud MongoDB)</li>
  <li><strong>ML / Data</strong>: Typical Python ML ecosystem (e.g., pandas, scikit-learn, PyYAML, etc. – defined in <code>requirements.txt</code>)</li>
  <li><strong>Cloud</strong>: AWS (S3, EC2, ECR, IAM)</li>
  <li><strong>Orchestration &amp; CI/CD</strong>: GitHub Actions with self-hosted runner on EC2</li>
  <li><strong>Containerization</strong>: Docker images stored in Amazon ECR</li>
  <li><strong>Web framework</strong>: Python web app (e.g., Flask/FastAPI) exposed via <code>app.py</code> for prediction and training routes.</li>
</ul>

<h2>Repository Structure</h2>
<p>
The project follows a clean, MLOps-oriented layout to keep configuration, components, entities, and utilities organized.
</p>

<pre>
project-root/
├── template.py
├── setup.py
├── pyproject.toml
├── requirements.txt
├── notebook/
│   ├── mongoDB_demo.ipynb
│   └── EDA_and_Feature_Engineering.ipynb
├── src/
│   ├── constants/
│   │   └── __init__.py
│   ├── configuration/
│   │   ├── mongo_db_connections.py
│   │   └── aws_connection.py
│   ├── data_access/
│   │   └── proj1_data.py
│   ├── entity/
│   │   ├── config_entity.py
│   │   ├── artifact_entity.py
│   │   ├── estimator.py
│   │   └── s3_estimator.py
│   ├── components/
│   │   ├── data_ingestion.py
│   │   ├── data_validation.py
│   │   ├── data_transformation.py
│   │   ├── model_trainer.py
│   │   ├── model_evaluation.py
│   │   └── model_pusher.py
│   ├── aws_storage/
│   ├── pipeline/
│   │   ├── training_pipeline.py
│   │   └── prediction_pipeline.py
│   ├── utils/
│   │   └── main_utils.py
│   ├── logger.py
│   └── exception.py
├── app.py
├── static/
├── templates/
└── .github/
    └── workflows/
        └── aws.yaml
</pre>

<h2>Local Setup &amp; Installation</h2>

<h3>Project Template &amp; Packaging</h3>
<ol>
  <li>Run the project template generator:
    <pre>python template.py</pre>
  </li>
  <li>Configure local package imports in <code>setup.py</code> and <code>pyproject.toml</code> so that <code>src</code> modules can be imported as installable packages.</li>
</ol>

<h3>Create Virtual Environment</h3>
<ol>
  <li>Create and activate the Conda environment:
    <pre>
conda create -n vehicle python=3.10 -y
conda activate vehicle
    </pre>
  </li>
  <li>Update <code>requirements.txt</code> with all required packages.</li>
  <li>Install dependencies:
    <pre>pip install -r requirements.txt</pre>
  </li>
  <li>Verify local package installation:
    <pre>pip list</pre>
  </li>
</ol>

<h3>MongoDB Atlas Setup</h3>
<ol>
  <li>Sign up or log in to MongoDB Atlas and create a new project, then create an M0 cluster with default settings.</li>
  <li>Create a database user (username and password) and whitelist <code>0.0.0.0/0</code> in Network Access to allow external connections.</li>
  <li>From the “Connect” → “Drivers” section, select Driver: Python, Version: 3.6 or later, and copy the connection string (replace <code>&lt;password&gt;</code>).</li>
  <li>Create a <code>notebook</code> folder, add the dataset, and use <code>mongoDB_demo.ipynb</code> (kernel: <code>vehicle</code>) to push data into MongoDB.</li>
  <li>Validate data in MongoDB Atlas via Database → Browse Collections.</li>
</ol>

<h3>Logging, Exceptions, and Notebooks</h3>
<ol>
  <li>Implement and configure the central logger in <code>logger.py</code> and test it in <code>demo.py</code>.</li>
  <li>Implement custom exception handling in <code>exception.py</code> and test via <code>demo.py</code>.</li>
  <li>Use the EDA and Feature Engineering notebook to understand insurance data distributions and engineer relevant risk and pricing features.</li>
</ol>

<h2>ML Pipeline Components</h2>

<h3>Data Ingestion</h3>
<ol>
  <li>Declare all required constants in <code>constants/__init__.py</code>.</li>
  <li>Implement MongoDB connection logic in <code>configuration/mongo_db_connections.py</code>.</li>
  <li>In <code>data_access/proj1_data.py</code>, implement functions to:
    <ul>
      <li>Connect to MongoDB.</li>
      <li>Fetch policy and claim data as key–value documents.</li>
      <li>Convert data into a pandas DataFrame.</li>
    </ul>
  </li>
  <li>Define <code>DataIngestionConfig</code> in <code>entity/config_entity.py</code> and <code>DataIngestionArtifact</code> in <code>entity/artifact_entity.py</code>.</li>
  <li>Implement the <code>DataIngestion</code> component in <code>components/data_ingestion.py</code> and integrate it into the training pipeline.</li>
</ol>

<h3>Environment Variables</h3>
<p>
Set the MongoDB URL as an environment variable for secure configuration on both Unix-like shells and Windows.
</p>
<pre>
# Bash
export MONGODB_URL="mongodb+srv://&lt;username&gt;:&lt;password&gt;@&lt;cluster&gt;/&lt;db&gt;"

# PowerShell
$env:MONGODB_URL = "mongodb+srv://&lt;username&gt;:&lt;password&gt;@&lt;cluster&gt;/&lt;db&gt;"

# Windows GUI
Name:  MONGODB_URL
Value: &lt;connection-url&gt;
</pre>

<h3>Data Validation, Transformation, and Model Trainer</h3>
<ol>
  <li>Complete <code>utils/main_utils.py</code> and <code>config/schema.yaml</code> with full dataset schema and validation rules.</li>
  <li>Implement Data Validation component mirroring the Data Ingestion structure (config, artifact, component, and pipeline wiring).</li>
  <li>Implement Data Transformation component, including feature engineering, preprocessing, and estimator serialization in <code>entity/estimator.py</code>.</li>
  <li>Implement Model Trainer, defining training configuration and model training logic in <code>components/model_trainer.py</code> and related entities.</li>
</ol>

<h2>Model Evaluation, Registry &amp; AWS Integration</h2>

<h3>AWS Credentials &amp; S3 Configuration</h3>
<ol>
  <li>Log in to the AWS console (region <code>us-east-1</code>) and create an IAM user (for example, <code>firstproj</code>) with <code>AdministratorAccess</code>.</li>
  <li>Create access keys, download the CSV, and export:
    <pre>
export AWS_ACCESS_KEY_ID="&lt;ACCESS_KEY_ID&gt;"
export AWS_SECRET_ACCESS_KEY="&lt;SECRET_ACCESS_KEY&gt;"
    </pre>
  </li>
  <li>Update <code>constants/__init__.py</code> with:
    <pre>
MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
MODEL_BUCKET_NAME = "my-model-mlopsproj"
MODEL_PUSHER_S3_KEY = "model-registry"
    </pre>
  </li>
  <li>Create an S3 bucket named <code>my-model-mlopsproj</code> in <code>us-east-1</code> and configure public access settings as required.</li>
  <li>Implement AWS S3 connectivity in <code>configuration/aws_connection.py</code> and S3 helper logic in <code>src/aws_storage</code> plus <code>entity/s3_estimator.py</code> for pushing and pulling models.</li>
</ol>

<h3>Model Evaluation &amp; Model Pusher</h3>
<ol>
  <li>Implement Model Evaluation component to:
    <ul>
      <li>Compare the new insurance model with the previously deployed model stored in S3.</li>
      <li>Use <code>MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE</code> to decide whether to promote the new model.</li>
    </ul>
  </li>
  <li>Implement Model Pusher component to:
    <ul>
      <li>Upload the approved model to S3 under <code>MODEL_PUSHER_S3_KEY</code>.</li>
      <li>Update model registry references used by the prediction pipeline.</li>
    </ul>
  </li>
</ol>

<h2>Prediction Service</h2>
<ol>
  <li>Implement the Prediction Pipeline in <code>pipeline/prediction_pipeline.py</code> to load the latest approved model and preprocessing objects from S3 or local artifacts.</li>
  <li>Configure <code>app.py</code> to expose:
    <ul>
      <li>Root or <code>/predict</code> route for real-time vehicle insurance premium and claim-risk predictions.</li>
      <li><code>/training</code> route to trigger model training from the browser or via HTTP tools.</li>
    </ul>
  </li>
  <li>Add <code>static</code> and <code>templates</code> directories for HTML templates and assets.</li>
</ol>

<h2>Dockerization &amp; CI/CD on AWS</h2>

<h3>Docker &amp; GitHub Actions</h3>
<ol>
  <li>Create a production-ready <code>Dockerfile</code> and <code>.dockerignore</code> to containerize the application.</li>
  <li>Configure <code>.github/workflows/aws.yaml</code> to:
    <ul>
      <li>Build the Docker image on each push to the main branch.</li>
      <li>Push the image to Amazon ECR.</li>
      <li>Trigger deployment on the EC2 self-hosted runner.</li>
    </ul>
  </li>
  <li>Create a dedicated IAM user (for example, <code>usvisa-user</code>) for CI/CD with permissions for ECR and deployment operations.</li>
</ol>

<h3>ECR &amp; EC2 Setup</h3>
<ol>
  <li>Create an ECR repository (for example, <code>vehicleproj</code>) in <code>us-east-1</code> and keep the repository URI for the CI/CD workflow.</li>
  <li>Launch an EC2 Ubuntu instance (for example, <code>vehicledata-machine</code>, t2.medium, 30 GB storage), enable HTTP/HTTPS, and connect via EC2 Instance Connect.</li>
  <li>Install Docker on EC2:
    <pre>
sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
    </pre>
  </li>
</ol>

<h3>Self-hosted Runner &amp; GitHub Secrets</h3>
<ol>
  <li>From GitHub → Settings → Actions → Runners, create a new self-hosted runner (Linux) and run the provided install and configure commands on the EC2 instance.</li>
  <li>Start the runner with <code>./run.sh</code> and verify its status as idle in GitHub.</li>
  <li>Add the following GitHub repository secrets:
    <ul>
      <li><code>AWS_ACCESS_KEY_ID</code></li>
      <li><code>AWS_SECRET_ACCESS_KEY</code></li>
      <li><code>AWS_DEFAULT_REGION</code> (for example, <code>us-east-1</code>)</li>
      <li><code>ECR_REPO</code> (ECR repository URI)</li>
    </ul>
  </li>
  <li>On the next commit and push, the CI/CD pipeline builds, pushes, and deploys the updated container image automatically.</li>
</ol>

<h2>Running the Application</h2>
<ol>
  <li>On the EC2 instance, ensure the container is running and listening on the configured internal port (for example, 5080 inside the container).</li>
  <li>In the EC2 Security Group, add an inbound rule:
    <ul>
      <li>Type: Custom TCP</li>
      <li>Port range: 5080</li>
      <li>Source: <code>0.0.0.0/0</code></li>
    </ul>
  </li>
  <li>Access the app in a browser:
    <pre>http://&lt;EC2-PUBLIC-IP&gt;:5080</pre>
  </li>
  <li>Use the training route (for example, <code>/training</code>) to trigger model training jobs from the deployed service if configured.</li>
</ol>

<h2>Best Practices &amp; Notes</h2>
<ul>
  <li>Never commit secrets (MongoDB connection strings, AWS keys) to version control; always use environment variables or secret managers.</li>
  <li>Update <code>.gitignore</code> to exclude folders like <code>artifact</code>, virtual environments, and any local data dumps.</li>
  <li>Continuously refine documentation, notebooks, and schema definitions as the project evolves to keep everything aligned with the production system.</li>
</ul>

</body>
</html>
