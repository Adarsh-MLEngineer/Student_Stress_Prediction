 ## Student Stress Predictor: Machine Learning GUI Application 

This is a **Python desktop application** built with `tkinter` that uses a pre-trained **Logistic Regression** model to predict a student's stress level based on a series of self-reported academic, lifestyle, and health factors.

It allows users to input their current status (rated on a 1-5 scale) and instantly receive a stress level prediction, along with a visual comparison of their inputs against the dataset's average student.

-----

##  Key Features

  * **Stress Level Prediction:** Uses a machine learning model to classify the student's stress into one of three categories:
      * **0: Low Stress** (No Depression)
      * **1: Medium Stress** (At Risk)
      * **2: High Stress** (Depression)
  * **Dynamic Input Form:** Generates a GUI form based on the features present in the input CSV file.
  * **Input Validation:** Ensures all user inputs are valid integers between 1 and 5.
  * **Visual Comparison Chart:** Displays a bar chart comparing the user's input scores against the average scores of students in the training dataset.
  * **Model Persistence:** The Logistic Regression model is trained once and saved as a `pickle` file (`student_stress_model.pkl`) for faster loading on subsequent runs.
  * **Data Preprocessing:** Automatically handles feature scaling and target variable transformation (mapping raw stress scores to 0, 1, or 2).

-----

## Prerequisites

To run this application, you need to have **Python** installed along with the following libraries:

  * `pandas`
  * `tkinter` (usually built-in with Python)
  * `matplotlib`
  * `scikit-learn` (`sklearn`)
  * `numpy`
  * `pickle`

You can install the required Python libraries using `pip`:

```bash
pip install pandas matplotlib scikit-learn numpy
```

-----

## Usage

### 1\. Prepare the Data

You need a **CSV file** containing student stress data. This file must include features used for prediction and a target column related to stress levels (e.g., `'stress levels'`).

**Note:** The code is set up to specifically look for a column containing the substring `'stress levels'` (case-insensitive) as the target variable.

### 2\. Run the Application

Execute the Python script:

```bash
python your_script_name.py
```

*(Replace `your_script_name.py` with the actual file name)*

### 3\. Load the Dataset

1.  Upon execution, a file dialog will open, prompting you to **select your Student Stress CSV File**.
2.  The application will automatically train the model (if `student_stress_model.pkl` doesn't exist) or load the existing model.

### 4\. Input Your Scores

1.  A window titled **"Student Stress Predictor"** will appear.
2.  Enter your ratings for each feature on a scale of **1 to 5**.
      * **Quality Features** (e.g., Sleep Quality): **1=Poor, 5=Excellent**
      * **Frequency/Load Features** (e.g., Headaches, Study Load): **1=High Stress, 5=Low Stress**
      * **Other Features:** **1=Poor, 5=Good**
3.  Click the **"Predict Stress Level"** button.

### 5\. View Results

1.  A message box will display the predicted stress level (Low, Medium, or High).
2.  A **Comparison Chart** will appear at the bottom of the window, showing your scores against the dataset's average student for each category.

-----

## Code Structure

The code is organized into four main sections:

1.  **DATA LOADING:** Handles file selection, reading the CSV into a `DataFrame`, dropping `NaN` values, identifying features/target, and preparing the data for the model (including target transformation and splitting into train/test sets).
2.  **MODEL TRAINING / LOADING:** Checks for the existence of `student_stress_model.pkl`. If not found, it creates and trains a `StandardScaler` + `LogisticRegression` pipeline and saves it. Otherwise, it loads the existing model.
3.  **HELPER FUNCTIONS:** Contains utility functions for:
      * `clean_label`: Formats feature names for the GUI and adds the 1-5 scale guide.
      * `validate_input`: Enforces the 1-5 input rule.
      * `invert_features`: Inverts scores for features (like 'Headaches' or 'Study Load') where a lower raw score implies *higher* stress, ensuring that for the model, a higher score always means 'better' (less stress).
      * `predict_stress`: The main logic for getting inputs, making the prediction, and showing results.
      * `show_comparison_chart`: Creates and embeds the `matplotlib` bar chart into the `tkinter` window.
4.  **TKINTER GUI SETUP:** Sets up the main window, dynamically creates input labels and fields, attaches the validation logic, and places the final prediction button.