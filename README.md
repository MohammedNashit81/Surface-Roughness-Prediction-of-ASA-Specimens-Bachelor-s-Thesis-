# Surface Roughness Prediction of 3D-Printed Specimens

A Machine Learning regression project that predicts the surface roughness of a 3D-printed specimen from its printing parameters.

![image alt]()

## Project Overview

Surface quality is an important factor in additive manufacturing. This project uses experimental printing data to estimate surface roughness from selected process parameters.

The task is formulated as **supervised regression**:

- **Input:** 3D-printing parameters
- **Target:** Measured surface roughness (a continuous numerical value)

The notebook compares multiple regression algorithms and evaluates their predictions using standard regression metrics. It also includes a Gradio interface for entering printing parameters and viewing a prediction.

## Input Features

The dataset used in the notebook contains the following columns:

| Feature | Description |
|---|---|
| Build Angle | Build orientation angle |
| Infill Angle | Infill pattern angle |
| Layer Thickness (mm) | Thickness of each printed layer |
| Printing Speed (mm/s) | Printing speed |
| Nozzle Temperature | Printing nozzle temperature |

**Target column:** `Surface Roughness`

The dataset also contains a `Sample` identifier, which is excluded from the model features in the interface section.

> The experimental dataset (`Dataset_Project (1).xlsx`) is not included in this repository by default. Add it only if you have permission to share it, and update the notebook's file path accordingly.

## Machine Learning Models

The notebook evaluates these regression models:

- Gradient Boosting Regressor
- XGBoost Regressor
- AdaBoost Regressor
- Random Forest Regressor
- Extra Trees Regressor
- K-Nearest Neighbors (KNN) Regressor
- Support Vector Regression (SVR)

The notebook evaluates models with 80:20 and 70:30 train-test splits for the model comparisons. It also plots actual versus predicted surface roughness values.

## Evaluation Metrics

Regression performance is measured using:

- **RMSE (Root Mean Squared Error):** Penalizes larger prediction errors more strongly.
- **MAE (Mean Absolute Error):** Average absolute difference between actual and predicted values.
- **MAPE (Mean Absolute Percentage Error):** Average absolute prediction error expressed as a percentage.

Lower values indicate better performance for these error metrics.

## Results

### Ensemble model comparison — 80:20 split

| Model | RMSE | MAE | MAPE |
|---|---:|---:|---:|
| Gradient Boosting | 0.5819 | 0.4476 | 15.22% |
| XGBoost | 0.6918 | 0.4799 | 13.61% |
| **AdaBoost** | **0.4488** | **0.3610** | **10.67%** |
| Random Forest | 0.5133 | 0.3870 | 11.22% |
| Extra Trees | 0.4895 | 0.3902 | 12.14% |

In this comparison, AdaBoost achieved the lowest RMSE, MAE, and MAPE.

### Comparison with traditional regression models — 80:20 split

| Model | RMSE | MAE | MAPE |
|---|---:|---:|---:|
| **AdaBoost** | **0.4488** | **0.3610** | **10.67%** |
| KNN | 0.6335 | 0.5395 | 15.91% |
| SVR | 0.5520 | 0.5058 | 15.56% |

These results are from the notebook's recorded evaluation outputs. Performance may vary with the dataset, split, and model configuration.

## Prediction Interface

The notebook includes a **Gradio** interface. Users can enter:

1. Build angle
2. Infill angle
3. Layer thickness
4. Printing speed
5. Nozzle temperature

The interface returns a predicted surface roughness value.

**Implementation note:** The comparison section identifies AdaBoost as the best model in its 80:20 ensemble comparison. The Gradio implementation in the notebook separately trains and saves a Decision Tree Regressor (`max_depth=5`) with a `StandardScaler`. The interface therefore does not currently use the AdaBoost comparison model.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- XGBoost
- Gradio
- Joblib
- Jupyter Notebook / Google Colab

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib scikit-learn xgboost gradio joblib openpyxl
```

### 3. Add the dataset

Place the Excel dataset in the project directory, then update the path in the notebook:

```python
file_path = "Dataset_Project (1).xlsx"
```

### 4. Run the notebook

Open `Surface_Roughness_Prediction_of_ASA_Specimens.ipynb` in Jupyter Notebook or Google Colab and run the cells.

The notebook's Gradio section trains the Decision Tree interface model and launches the application.

## Project Workflow

1. Load experimental data from Excel.
2. Separate input features and the target variable.
3. Split the data into training and testing sets.
4. Train and compare regression algorithms.
5. Calculate RMSE, MAE, and MAPE.
6. Visualize actual and predicted values.
7. Train a model for the Gradio prediction interface.
8. Save the interface model and scaler using Joblib.
