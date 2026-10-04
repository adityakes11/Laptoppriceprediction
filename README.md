# Laptop Price Predictor

A simple **Streamlit** web application that predicts the price of a laptop based on its specifications.

## ✨ Features
- Interactive UI for selecting laptop characteristics (brand, type, RAM, weight, touchscreen, IPS, screen size, resolution, CPU, storage, GPU, OS).
- Uses a pre‑trained regression model (stored in `pipe.pkl`) to estimate the price.
- Shows the predicted price as Indian Rupees (₹) directly in the app.

## 📂 Repository Structure
```
.
├─ app.py          # Streamlit app – UI and inference logic
├─ df.pkl          # Pickled pandas DataFrame with the training data (used for populating dropdowns)
├─ pipe.pkl        # Pickled scikit‑learn pipeline (model + preprocessing)
└─ README.md       # This documentation
```

## 🛠️ Setup & Installation
1. **Clone the repository** (or open the existing project folder).
2. Ensure you have Python 3.12+ installed.
3. Create a virtual environment and install the required packages:
   ```bash
   python -m venv .venv
   .venv\Scripts\activate   # PowerShell: .venv\Scripts\Activate.ps1
   pip install --upgrade pip
   pip install streamlit numpy pandas scikit-learn
   ```
4. The model files (`df.pkl`, `pipe.pkl`) are already included, so no additional training is required.

## ▶️ Running the Application
```bash
streamlit run app.py
```
Open the URL displayed in the console (usually `http://localhost:8501`).

## How It Works
- The app loads the DataFrame (`df.pkl`) to populate selectable options for categorical features.
- When the **Predict Price** button is pressed, the selected inputs are transformed into a feature vector:
  - `ppi` (pixels per inch) is calculated from screen resolution and size.
  - Boolean features (`touchscreen`, `ips`) are converted to binary values.
- The feature vector is fed to the pre‑trained pipeline (`pipe.pkl`).
- The model predicts the **log‑price**; the app exponentiates (`np.exp`) to obtain the actual price and displays it.
## 🤖 Model Details
- The model is stored in `pipe.pkl` as a scikit‑learn **Pipeline**.
- Typical preprocessing steps:
  - `ColumnTransformer` with `OneHotEncoder` for categorical columns (`Company`, `TypeName`, `Cpu brand`, `Gpu brand`, `os`).
  - `StandardScaler` (or similar) for numeric features (`ram`, `weight`, `ppi`, `hdd`, `ssd`).
- The regression estimator is a **LinearRegression** (or `Ridge`) trained on log‑transformed prices.
- The pipeline outputs the **log‑price**; the app applies `np.exp` to convert it back to the actual price.
## 📦 Customisation
- **Add new features**: Modify the UI in `app.py` and adjust the feature vector accordingly.
- **Retrain the model**: Replace `pipe.pkl` with a new pipeline trained on an updated dataset (ensure the same column order as used in `app.py`).
- **Change data source**: Update `df.pkl` with a new DataFrame that contains the necessary categorical columns (`Company`, `TypeName`, `Cpu brand`, `Gpu brand`, `os`).

## ⚠️ Notes
- The model expects the input order `[company, type, ram, weight, touchscreen, ips, ppi, cpu, hdd, ssd, gpu, os]`.
- The price is shown in Indian Rupees (₹) and is rounded to the nearest integer.

## 📜 License
This project is provided for educational purposes. Feel free to use, modify, and distribute it as needed.
