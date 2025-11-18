# Air Quality Prediction Pipeline with Hopsworks

This project builds a complete machine-learning system for forecasting air quality (PM2.5) using weather data and historical air-quality measurements.  
The system includes feature pipelines, model training, batch inference, dashboards, and accuracy monitoring — using **Hopsworks** and **GitHub Pages**, and automated with **GitHub Actions**

---

## **1. Backfill Feature Pipeline**

The first step is a pipeline that:

- Downloads **historical weather data** (ideally >1 year).
- Loads a CSV file containing **historical air-quality data** (e.g., downloaded from https://aqicn.org).
- Cleans, processes, and registers the data in **Hopsworks** as **two Feature Groups**:
  - `weather_features`
  - `air_quality_features`

This step is performed by the file mlfs-book/notebooks/airquality/1_air_quality_feature_backfill.ipynb.

---

## **2. Daily Feature Pipeline**

The second step:

1. Downloads **yesterday’s weather** and **air-quality measurements**.
2. Downloads **weather forecasts** for the next 7 days.
3. Updates the corresponding Feature Groups in Hopsworks.

This ensures your feature store is always fresh and ready for training/inference.

This step is performed by the file mlfs-book/notebooks/airquality/2_air_quality_feature_pipeline.ipynb.

This file is ran once daily using GitHub Actions
---

## **3. Training Pipeline**

The third steps performs training workflow that:

1. Selects and joins features via a **Feature View**.
2. Loads training data automatically from the Hopsworks Feature Store.
3. Trains a **regression or classification model** to predict **PM2.5** air-quality values.
4. Registers the trained model to the **Hopsworks Model Registry**.

Models can be trained on a schedule or on-demand.

This step is performed by the file mlfs-book/notebooks/airquality/3_air_quality_training_pipeline.ipynb.
---

## **4. Batch Inference Pipeline + Dashboard**

The fourth step is a inference pipeline that:

- Downloads the trained model from Hopsworks.
- Loads the latest and forecasted feature data.
- Predicts **air quality for the next 7–10 days** for a selected sensor.
- Generates a dashboard visualizing:
  - Predicted PM2.5 values
  - Weather context
  - Time-series forecast horizon

This dashboard is deployed using GitHub Pages(https://ololpol.github.io/mlfs-book/)

This step is performed by the file mlfs-book/notebooks/airquality/4_air_quality_batch_inference.ipynb.

This file is ran once daily using GitHub Actions

---

## 📌 **5. Monitoring + Hindcast Evaluation**

Using GitHub Pages, the results of the daily inferences can be tracked. Two plots are presented:

- A plot of predicted future air quality predictions.
- A hindcast plot showing the accuracy of previous predictions vs outcomes.


---

# Grade ‘C’ Extension Task

## **6. Add Lagged Air-Quality Features**

The model is how enhanced by creating a new feature group with 3 additional features

- `pm25_lag1`
- `pm25_lag2`
- `pm25_lag3`

Theese features represent lagged air-quality one, two or three days back. A new model is trained using theese additional features, and plots are generated using predictions from this model as well.


# mlfs-book
O'Reilly book - Building Machine Learning Systems with a feature store: batch, real-time, and LLMs


## ML System Examples


[Dashboards for Example ML Systems](https://featurestorebook.github.io/mlfs-book/)


# Run Air Quality Tutorial

See [tutorial instructions here](https://docs.google.com/document/d/1YXfM1_rpo1-jM-lYyb1HpbV9EJPN6i1u6h2rhdPduNE/edit?usp=sharing)
    
# Create a conda or virtual environment for your project before you install the requirements
    pip install -r requirements.txt


##  Run pipelines with make commands

    make aq-backfill
    make aq-features
    make aq-train
    make aq-inference
    make aq-clean

or 
    make aq-all



## Feldera


mkdir -p /tmp/c.app.hopsworks.ai
ln -s  /tmp/c.app.hopsworks.ai ~/hopsworks
docker run -p 8080:8080 \
  -v ~/hopsworks:/tmp/c.app.hopsworks.ai \
  --tty --rm -it ghcr.io/feldera/pipeline-manager:latest


## Introduction to ML
I wrote a brief introduction to machine learning [here](./introduction_to_supervised_ml.pdf)
