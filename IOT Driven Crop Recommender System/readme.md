# IoT-Based Crop Recommender System

## Project Description
<p align="justify">
This project aims to develop an IoT-based crop recommender system to assist farmers in making informed decisions about crop selection. The system leverages sensor data, machine learning algorithms, and cloud computing to provide real-time recommendations for suitable crops based on specific agricultural conditions.
</p>

---

## Problem Statement
<p align="justify">
Farmers often face challenges in selecting the most appropriate crops due to limited knowledge about soil conditions and environmental factors. This can lead to suboptimal crop choices, reduced yields, and financial losses.
</p>

---

## Proposed Solution
<p align="justify">
The proposed solution is an Android app that acts as a virtual agronomist for farmers. The app collects data on soil composition (nitrogen, potassium, phosphorus levels) and environmental factors (temperature, humidity, pH, rainfall) using IoT sensors. This data is then processed using machine learning algorithms to predict the most suitable crops for a specific area.
</p>

---

## System Architecture
The system consists of the following components:
- **IoT Sensors:** Collect data on soil composition and environmental factors.  
- **Raspberry Pi:** Acts as a data acquisition and processing unit.  
- **Cloud Platform:** Stores and processes data, runs machine learning models, and provides recommendations.  
- **Android App:** User interface for farmers to access recommendations.  

---

## Data Collection and Processing
- **Sensor Data:** Sensors collect soil and environmental data.  
- **Data Transmission:** Data is sent to the cloud platform via Wi-Fi or cellular networks.  
- **Data Storage:** Data is securely stored in the cloud for analysis.  

### Sensor Details
- Soil Moisture Sensor  
- Soil pH Sensor  
- Soil Nutrient Sensor (N, P, K levels)  
- Temperature Sensor  
- Humidity Sensor  
- Light Sensor  
- Rainfall Sensor  

---

## Machine Learning Models
Several machine learning models were implemented and evaluated:
- **k-Nearest Neighbors (k-NN)**  
- **Support Vector Classifier (SVC)**  
- **Logistic Regression**  
- **Random Forest**  
- **Gradient Boosting**  
- **LightGBM**

---

## Model Evaluation and Selection
Performance was evaluated using **accuracy, precision, recall, and F1-score**:

| Model               | Accuracy | Precision | Recall | F1-Score |
|----------------------|----------|-----------|--------|----------|
| k-NN                | 0.98     | 0.98      | 0.98   | 0.98     |
| SVC                 | 0.99     | 0.99      | 0.99   | 0.99     |
| Logistic Regression | 0.98     | 0.99      | 0.98   | 0.98     |
| Random Forest       | 0.99     | 0.99      | 0.99   | 0.99     |
| Gradient Boosting   | 0.99     | 0.99      | 0.99   | 0.99     |
| LightGBM            | 0.99     | 0.99      | 0.99   | 0.99     |

<p align="justify">
Based on these results, **LightGBM** was selected as the best-performing model, achieving superior performance across all key metrics.
</p>

---

## Crop Recommendations
<p align="justify">
The system provides real-time recommendations for suitable crops based on the collected data and the trained LightGBM model. Recommendations are delivered via the Android app.
</p>

🔗 [View the Crop Recommender App](https://croprecommender.anvil.app/)  
📓 Explore the full implementation in the following Colab notebook:  
🔗 [Google Colab Notebook](https://colab.research.google.com/drive/1S74GQ0mPJ7lsJaxWqShPSXyjwYYaUbUw)

---

## Future Work
- Integrate real-time sensor data collection.  
- Expand geographical coverage.  
- Incorporate crop disease prediction.  
- Develop mobile applications.  

---

## Expected Outcomes
- Empowers farmers with **data-driven decision making**.  
- Optimizes **resource utilization**.  
- Improves **crop yields and livelihoods**.  
- Contributes to **sustainable agriculture practices**.  
