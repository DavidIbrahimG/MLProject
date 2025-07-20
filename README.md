# Student Performance Prediction Project

## Project Overview

This project focuses on predicting **student performance** based on various factors such as study time, parental education level, lunch type, test preparation course, and individual subject scores. The pipeline follows an end-to-end **machine learning lifecycle** that includes **data ingestion**, **exploratory data analysis (EDA)**, **data transformation**, **model training**, and **model deployment**. The final model was deployed on **AWS EC2**, **AWS Elastic Beanstalk**, and **Azure Web App** using **Docker** and **ECR**.

### **Key Features and Goals:**

- **Data Ingestion**: Data is pulled from a CSV file and cleaned.
- **Exploratory Data Analysis (EDA)**: Performed to understand the distribution and relationships of data.
- **Preprocessing**: One-hot encoding of categorical variables and scaling of numerical features.
- **Model Training**: Several machine learning models were evaluated, and the best-performing model was selected.
- **Prediction and API Deployment**: The model was deployed using **Flask** to make real-time predictions.
- **Cloud Deployment**: The final model and application were deployed to **AWS EC2**, **AWS Elastic Beanstalk**, **Azure Web App**, and **AWS ECR**.

---

## **Project Workflow**

### 1. **Data Collection and Ingestion**

The data was initially collected in a CSV format, containing various student performance metrics, such as:
- `gender`, `race_ethnicity`, `parental_level_of_education`, `lunch`, `test_preparation_course`
- `math_score`, `reading_score`, `writing_score`

This data was **ingested**, cleaned, and prepared for model training.

### 2. **Exploratory Data Analysis (EDA)**

The data was thoroughly explored using **Jupyter Notebooks** to:
- Identify correlations between features.
- Analyze the distribution of numerical and categorical features.
- Handle missing values, if any, and check for outliers.

### 3. **Data Transformation and Feature Engineering**

To prepare the data for training:
- **One-Hot Encoding** was applied to categorical features (`gender`, `lunch`, etc.).
- **Scaling** was applied to numerical features (`math_score`, `reading_score`, `writing_score`) using `StandardScaler`.

### 4. **Model Training and Evaluation**

<img width="571" height="448" alt="Screenshot 2025-07-20 at 13 23 56" src="https://github.com/user-attachments/assets/2444ea3e-1df5-492e-b571-3269f548a0b5" />


Several machine learning models were trained and evaluated using **cross-validation** and metrics like **accuracy**, **precision**, **recall**, and **F1-score**. The models included:
- **Logistic Regression**
- **Random Forest**
- **Gradient Boosting**
- **XGBoost**

The best-performing model was selected based on evaluation metrics and was fine-tuned using **hyperparameter optimization**.

### 5. **Flask API for Predictions**

Once the model was trained and evaluated:
- A **Flask** web application was created to expose the model as an **API**.
- The API receives incoming data (such as study time, parental education, etc.) and returns predicted scores for students.
- The model was serialized and stored using **Pickle** for easy access and loading in production.

---

## **Deployment on Cloud**

### 1. **AWS Deployment (EC2, ECR, Elastic Beanstalk)**

The application was deployed to **AWS EC2**, **AWS Elastic Beanstalk**, and **AWS ECR** using **Docker**:

#### **Steps Taken**:
1. **Dockerize the Application**:
   - The Flask app was packaged into a **Docker container**.
   - A **Dockerfile** was created to define the environment and dependencies for the app.
   
2. **Push Docker Image to AWS ECR**:
   - The Docker image was pushed to **AWS Elastic Container Registry (ECR)**.
   - This allowed the image to be stored and accessed from AWS.

3. **Deploy to AWS EC2**:
   - The Docker image was pulled from **ECR** and deployed to **AWS EC2** to host the Flask API.
   - The application was configured to be accessible via a public IP and port.

4. **Deploy to AWS Elastic Beanstalk**:
   - The application was deployed to **AWS Elastic Beanstalk**, which handles the scaling, monitoring, and management of the web application in the cloud.
   - Elastic Beanstalk made it easier to manage the environment and handle automatic scaling based on traffic.

---

### 2. **Azure Deployment (Azure Web App)**

The same application was also deployed to **Azure Web App** using **Docker**:

#### **Steps Taken**:
1. **Dockerize the Application**:
   - The Flask app was containerized using Docker.
   - The Docker image was pushed to **Azure Container Registry (ACR)**.

2. **Deploy to Azure Web App**:
   - The Docker image was pulled from **Azure Container Registry (ACR)** and deployed to **Azure Web App**.
   - Azure handled the deployment, scaling, and management of the app.
   - The app is now hosted and accessible via a custom URL provided by Azure.

---

## **Technologies Used**

- **Python**: Main programming language for the pipeline.
- **Pandas**: Data manipulation and analysis.
- **Scikit-learn**: For training and evaluating models.
- **Flask**: Framework for serving the model via API.
- **Docker**: Containerization of the app for consistent deployments.
- **AWS EC2**: Cloud infrastructure to host the app.
- **AWS Elastic Beanstalk**: Managed service for deploying the application at scale.
- **AWS ECR**: Storage for Docker images.
- **Azure Web App**: Managed platform to host the Flask application.
- **MLflow**: For tracking models, parameters, and experiments.
- **GitHub Actions**: Continuous integration and deployment (CI/CD).

---

## **Deployment Instructions**

### 1. **AWS EC2 Deployment (Using ECR & Beanstalk)**

1. **Create the Docker Image**:
   - Build the Docker image:
     ```bash
     docker build -t flask-app .
     ```

2. **Push Docker Image to AWS ECR**:
   - Authenticate and push the image:
     ```bash
     aws ecr create-repository --repository-name flask-app
     docker tag flask-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/flask-app:latest
     docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/flask-app:latest
     ```

3. **Deploy to AWS EC2**:
   - SSH into your EC2 instance and pull the Docker image:
     ```bash
     docker pull <aws_account_id>.dkr.ecr.<region>.amazonaws.com/flask-app:latest
     docker run -d -p 5000:5000 flask-app
     ```

4. **Deploy to AWS Elastic Beanstalk**:
   - Create an Elastic Beanstalk environment and deploy your Docker container.

### 2. **Azure Web App Deployment**

1. **Push Docker Image to Azure Container Registry**:
   - Create an ACR instance and push the Docker image:
     ```bash
     az acr create --name myacr --resource-group myresourcegroup --sku Basic
     az acr login --name myacr
     docker tag flask-app:latest myacr.azurecr.io/flask-app:latest
     docker push myacr.azurecr.io/flask-app:latest
     ```

2. **Deploy to Azure Web App**:
   - Create a web app in Azure and configure it to pull the image from ACR:
     ```bash
     az webapp create --resource-group myresourcegroup --plan myAppServicePlan --name myflaskapp --deployment-container-image-name myacr.azurecr.io/flask-app:latest
     ```

---


