# 🌍 ClimateGuard — End-to-End Disaster Probability Prediction System

**ClimateGuard** is a full-stack machine learning web application designed to forecast localized natural disaster probabilities for a specific **city** and **target year**. By combining historical meteorological and disaster datasets with supervised learning algorithms—specifically **Support Vector Machines (SVMs)** and **Extreme Gradient Boosting (XGBoost)**—the platform transforms raw tabular climate records into actionable risk probabilities via a **Flask** REST API and an interactive **React.js** dashboard, fully containerized with **Docker**.

## 📌 Table of Contents

1. [Project Overview & Motivation](#-project-overview--motivation)

2. [System Architecture & End-to-End Workflow](#-system-architecture--end-to-end-workflow)

3. [Machine Learning Pipeline & Methodology](#-machine-learning-pipeline--methodology)

4. [Tech Stack](#-tech-stack)

5. [API Endpoint Specification](#-api-endpoint-specification)

6. [Project Directory Structure](#-project-directory-structure)

7. [Getting Started (Docker & Local Setup)](#-getting-started-docker--local-setup)

8. [Roadmap & Future Enhancements](#-roadmap--future-enhancements)

## 🎯 Project Overview & Motivation

Urban planners, researchers, and risk analysts often struggle to translate historical climate datasets into localized, easy-to-interpret risk assessments. Traditional data science notebooks remain isolated from end users, while standard weather applications only offer short-term meteorological forecasts rather than long-term annual hazard probabilities.

**ClimateGuard bridges the gap between data science experimentation and production web engineering by:**

* Ingesting and preprocessing historical city-level climate and disaster records from structured `.csv` datasets using **Pandas** and **NumPy**.

* Benchmarking multiple supervised learning models (**SVMs** and **XGBoost**) in **Scikit-learn** to capture non-linear spatio-temporal relationships.

* Serving real-time probability inference through a lightweight **Flask** backend API.

* Providing an intuitive **React.js** user interface where users can select a target city and future year to visualize disaster probability scores.

* Packaging the entire application stack using **Docker** for reproducible, environment-agnostic deployment.

## 🔄 System Architecture & End-to-End Workflow

ClimateGuard operates across two distinct pipelines: an **Offline Training & Evaluation Pipeline** and an **Online Real-Time Inference Pipeline**.

```
+-----------------------------------------------------------------------------------+
|                        1. OFFLINE ML TRAINING PIPELINE                            |
+-----------------------------------------------------------------------------------+
  [Historical Climate CSV Dataset]
                 |
                 v
  [Data Cleaning & Preprocessing (Pandas / NumPy)]
  |-- Missing Value Imputation & Outlier Filtering
  |-- Categorical Encoding (City / Region Identifiers)
  |-- Feature Scaling & Temporal Feature Extraction (Year, Trends)
                 |
                 v
  [Model Training & Benchmarking (Scikit-learn / XGBoost)]
  |-- Support Vector Machines (SVM with RBF Kernel)
  |-- Extreme Gradient Boosting (XGBoost Classifier/Regressor)
                 |
                 v
  [Serialized Model Artifacts (.pkl / .joblib) & Encoders]

+-----------------------------------------------------------------------------------+
|                        2. ONLINE INFERENCE PIPELINE (DOCKERIZED)                  |
+-----------------------------------------------------------------------------------+
 [User Input on React.js Frontend] ---> (Selects Target City & Year)
                 |
                 v  HTTP POST /predict (JSON Payload)
  [Flask REST API Backend]
  |-- Input Validation & Feature Vector Transformation
  |-- Loads Pre-trained ML Model Artifact
  |-- Computes Disaster Probability Distribution
                 |
                 v  HTTP 200 OK (JSON Prediction Response)
  [React.js Interactive Dashboard]
  |-- Renders Probability Breakdown & Risk Severity
