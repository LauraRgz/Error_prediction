# Predicting Temporal 2Q Gate Error Rate Variability in Superconducting Quantum Processors

## Table of contents
* [Introduction](#introduction)
* [Models Implemented](#models-implemented)
* [Inputs and Outputs](#inputs-and-outputs)
* [Data](#data)
* [Evaluation Metric](#evaluation-metric)
* [How to Use the Models](#how-to-use-the-models)
  * [Steps to Run Any Model](#steps-to-run-any-model)
  * [Unified Notebook](#unified-notebook)

## Introduction
This repository contains five different predictive models implemented in Python to predict the variability of **two-qubit (2Q) gate error rates** across calibration cycles in quantum systems.

## Models Implemented

This repository includes implementations of the following models:

| Model | Description |
|-------|-------------|
| **FFT (Fast Fourier Transform)** | Frequency-domain approach for identifying periodic components in error rate time series. |
| **ARIMA (AutoRegressive Integrated Moving Average)** | Classic statistical method for univariate time series forecasting. |
| **LSTM (Long Short-Term Memory)** | A type of recurrent neural network well-suited for sequential data. |
| **RF (Random Forest)** | An ensemble learning method based on decision trees, useful for capturing nonlinear dependencies. |
| **CNN (Convolutional Neural Network)** | Adapted for time series, CNNs can capture local temporal patterns effectively. |

### Inputs and outputs
- **Input**: Time series of the calibration cycles to predict.
- **Output**: Predicted error rates for the next calibration cycles.

## Data

The dataset used for training and evaluating the models consists of two-qubit (2Q) gate error rates extracted from calibration data collected once daily over a period of nine months.

- **Duration**: 9 months  
- **Frequency**: Daily calibrations  
- **Data type**: 2Q gate error rates per qubit pair  

The dataset was split chronologically:

- **Training Set**: First **8 months** of data  
- **Evaluation Set**: Final **1 month** of data  

## Evaluation Metric

To assess model performance, we use the **Root Mean Square Error (RMSE)**, a standard metric for regression tasks that penalizes large deviations between predicted and actual values.

## How to Use the Models

Each model in this repository is implemented in its own Jupyter notebook (e.g., `arima_estimator.ipynb`, `LSTM_estimator.ipynb`, etc.). The workflow for using these models is consistent across all implementations.

### Steps to Run Any Model:

1. **Open the Notebook**  
   Navigate to the model's directory and open the corresponding `.ipynb` file.

2. **Prepare the Data**  
   The notebook includes code to load and preprocess the 2Q gate error rate time series. Make sure your dataset is correctly placed or update the path as needed.

3. **Train the Model**  
   Each notebook contains a section to train the model using data from the first 8 months.

4. **Generate Predictions**  
   The model forecasts error rates for the 9th month (the evaluation set).

5. **Evaluate Performance**  
   Predictions are evaluated using **RMSE** to quantify model accuracy.

6. **Visualize Results**  
   Notebooks include plots comparing actual vs. predicted values for easier interpretation.

#### Example: CNN Model

##### Main Steps:
- Load and preprocess the 2Q gate error rate time series.
- Prepare sequences of historical calibration data.
- Use a 1D CNN model defined in a utility function to train and forecast.
- Evaluate using RMSE.
- Plot predicted vs. actual error rates for visual comparison.

##### Sample Code

```python
# Run the CNN model for qubit pair '0-14'

cnn_estimator_past(
    data=df,                 # DataFrame with calibration data
    N_remove=30,             # Number of days to remove from data
    N_estimate=30,           # Number of days to estimate
    link='0-14',             # Target qubit pair
    normalize_data=True,
    epochs=100,
    save_figures=False,
    save_path=None,
    show_figs=True
)
```

The results can be:

- **Visualized:** Comparison plots of predicted vs. actual error rates can be shown in the notebook.

- **Saved:** A structured dictionary can be generated and optionally saved for further analysis.

Example graph visualization:
![Alt text](Results\cnn_0-14.png)

Example output dictionary:
```python
result = {
    "0-14": {
        "Normalized_data": ...,
        "Complete_dates": [...],
        "Complete_estimated_data": [...],

        "Past_dates": [...],
        "Last_date": ...,
        "Past_data": [...],

        "Future_dates": [...],
        "Future_estimated_data": [...],

        "Train_dates": [...],
        "Last_train_date": ...,
        "Train_data": [...],
        "Train_estimated_data": [...],

        "Train_estimation_error": [...],
        "Train_mean_estimation_error": ...,
        "Train_std_estimation_error": ...,
        "Train_rmse_estimation_error": ...,

        "Test_dates": [...],
        "Test_data": [...],
        "Test_estimated_data": [...],

        "Test_estimation_error": [...],
        "Test_mean_estimation_error": ...,
        "Test_std_estimation_error": ...,
        "Test_rmse_estimation_error": ...
    }
}
```

### Unified Notebook

In addition to individual model notebooks, we provide a single Jupyter notebook that contains **all five models**: `estimator.ipynb`.

This file includes:

- Data loading and preprocessing
- Model definitions for:
  - FFT
  - ARIMA
  - LSTM
  - RF
  - CNN
- Training and forecasting for each model
- RMSE evaluation on the 9th month
- Comparative plots and analysis

#### How to Use

1. Open `estimator.ipynb`.
2. Run all cells sequentially.
3. Review performance metrics and visualizations for each model.
4. Modify or extend any section to experiment with parameters, time windows, or different qubit pairs.
