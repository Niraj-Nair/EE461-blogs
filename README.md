
# EE461 Project Blog: AI-Based Load Forecasting
 
## 📅 Week 6: Project Introduction

This week, we began working on load forecasting using smart meter data from a garment factory in Valelevu, Suva. The main objectives were to:
- Understand the dataset features (voltage, current, energy)
- Explore the goal of the project
- Set up GitHub to document progress

## ✅ Tasks Completed:
- Created GitHub repository for blogging
- Collected and reviewed the smart meter dataset
- Identified key parameters (energy delta, current L1–L3, etc.)

## 📝 Next Steps:
- Start with data preprocessing
- Clean and normalize the dataset

## 📊 Data Preprocessing & Active Power Visualization

This week, we focused on **preprocessing the electrical load data** collected from the smart meter at the garment factory in Valelevu, Suva. The main goal was to clean and prepare the dataset for modeling, while beginning to explore the relationship between time and active power consumption.
 
### 📈 Visualization:
We created a **scatter plot** of **active power** over time to identify usage patterns. This helped us observe:
- Peak usage hours during the day (mostly between 8am–5pm)
- Sudden spikes in power usage (possibly during machine start-ups)
- Flat or low-usage periods (before or after working hours)
### 📈 Scatter Plot of Active Power Over Time

We used MATLAB to create a scatter plot showing how active energy consumption increased from **July 2023 to March 2025**.
  ![active power](https://github.com/user-attachments/assets/76d2e883-3d78-4286-8ea9-dc8d396d69e2)
```matlab
% Create scatter plot
figure;
scatter(time_data, active_power, 'b', 'filled'); % Blue filled dots
xlabel('Time');
ylabel('Active Power (kWh)');
title('Scatter Plot of Active Power Over Time');
grid on;
```
## 🔌 Week 6: Visualizing Three-Phase Currents Over Time

These plots help us understand:
- **How electrical load varies across all three phases**
- Whether there is any **imbalance** in current distribution
- The **pattern of current usage** throughout working hours
![phase currents](https://github.com/user-attachments/assets/9aab74d5-d9b8-4713-a8c7-331b597150ec)

```% Plot currents
figure;
plot(time_data, A1, 'r', 'LineWidth', 1.5); hold on;
plot(time_data, A2, 'g', 'LineWidth', 1.5);
plot(time_data, A3, 'b', 'LineWidth', 1.5);
hold off;

% Labels & legend
xlabel('Time'); ylabel('Current (A)');
title('Phase Currents Over Time');
legend('A1', 'A2', 'A3');
grid on;
```
## Addressing missing data from currents 
```
% Remove rows with any missing data
 LoadDataset = rmmissing(LoadDataset);
 % Ensure time_data is in datetime format
 time_data = datetime(LoadDataset.Datetime);
 % Extract currents
 A1 = LoadDataset.A1;
 A2 = LoadDataset.A2;
 A3 = LoadDataset.A3;
 % Remove outliers (values beyond 1.5*IQR)
 A1 = filloutliers(A1, 'previous');
 A2 = filloutliers(A2, 'previous');
 A3 = filloutliers(A3, 'previous');
 % Plot currents
 11
figure;
 plot(time_data, A1, 'r', 'LineWidth', 1.5); hold on;
 plot(time_data, A2, 'g', 'LineWidth', 1.5);
 plot(time_data, A3, 'b', 'LineWidth', 1.5);
 hold off;
 % Labels & legend
 xlabel('Time'); ylabel('Current (A)');
 title('Cleaned Phase Currents Over Time');
 legend('A1', 'A2', 'A3');
 grid on
```
## Shows the cleaned plot of the three phase currents 
![cleaned currents](https://github.com/user-attachments/assets/0e1730b7-9300-463d-b5cf-894743f9f17d)

# 📅EE461 Project Blog: Week 7-8
```
PLOT ALL RAW VARIABLES TO VIZUALIZE THE GRAPHS
% Step 1: Get variable names except the first column (Datetime)
varNames = LoadDataset.Properties.VariableNames(2:end);

% Step 2: Loop through each variable and plot
for i = 1:length(varNames)
    figure;
    plot(LoadDataset.Datetime, LoadDataset.(varNames{i}), 'LineWidth', 1.2);
    title(['Plot of ', varNames{i}, ' over Time']);
    xlabel('Time');
    ylabel(varNames{i});
    grid on;
end
```
![image](https://github.com/user-attachments/assets/2fdbbb4d-0c4c-4b3e-ae5e-d0d9917abfe4)
![image](https://github.com/user-attachments/assets/b0b3783e-54f3-47b4-9def-c448f07bb81d)
![image](https://github.com/user-attachments/assets/66abf5e8-d811-4c51-9c2b-8108de9d8af1)
![image](https://github.com/user-attachments/assets/324f87a6-1e19-4f66-b144-ee0f231ab359)
![image](https://github.com/user-attachments/assets/20b58d3a-56a6-4e46-8962-8b96d7bd525e)
![image](https://github.com/user-attachments/assets/8d49bdf1-5f29-4cce-820c-c5950450e24a)
![image](https://github.com/user-attachments/assets/00279e88-dcdf-4dfb-b15e-904032b6f34a)

When working with real-world power load datasets—like those collected from smart meters or industrial monitoring systems—it's crucial to clean and prepare the data before diving into analysis. In this post, we’ll walk through a practical MATLAB script that does just that: it removes missing values, filters out outliers, standardizes the data, and then visualizes each parameter over time.
```
%% Step 1: Remove rows with missing values
cleanData = rmmissing(LoadDataset)
%% Step 2: Extract numeric data (exclude the Datetime column)
numericData = cleanData{:, 2:end}
%% Step 3: Standardize the numeric values (z-score)
standardizedData = zscore(numericData)
%% Step 4: Remove outliers (keep rows where all z-scores are between -3 and +3)
noOutliersIdx = all(abs(standardizedData) <= 3, 2);
finalData = cleanData(noOutliersIdx, :)
%% Step 0: Rename columns correctly (based on your dataset)
finalData.Properties.VariableNames = { ...
    'Datetime', 'ActivePower', 'VoltageL1', 'VoltageL2', 'VoltageL3', ...
    'CurrentL1', 'CurrentL2', 'CurrentL3'};

%% Step 1: Get variable names (excluding 'Datetime')
varNames = finalData.Properties.VariableNames(2:end);

%% Step 2: Loop through and plot each variable
for i = 1:length(varNames)
    figure;
    plot(finalData.Datetime, finalData{:, varNames{i}}, 'LineWidth', 1.2);
    title(['Cleaned Plot of ', varNames{i}, ' over Time']);
    xlabel('Time');
    ylabel(varNames{i});
    grid on;
    drawnow;
    pause(0.5);  % Optional pause to ensure rendering
end
```
# ✅plot the cleaned data 
![image](https://github.com/user-attachments/assets/f4473857-0353-4f9a-b6eb-b2d1daea77ce)
![c voltahe v1](https://github.com/user-attachments/assets/9c201c07-0a5b-425b-855f-feef4952541d)

![c current ](https://github.com/user-attachments/assets/8a4afd58-900e-4a97-b496-0af35ff4b4bc)

🔍 Visualizing the Cleaned Power Load Data
Once the dataset was cleaned and outliers were removed, we visualized the key electrical parameters — Active Power, Voltage (L1, L2, L3), and Current (L1, L2, L3) — to better understand how the system performed over time.

⚡ Active Power Trends
The plot of Active Power over time shows a dynamic and fluctuating load profile across the entire observation period. While most values remain within a high range, there are clear dips in power consumption — likely due to low-demand periods or operational shutdowns. The cleaned plot provides a more accurate reflection of real usage patterns, making it suitable for further forecasting or energy management analysis.

🔌 Voltage Stability
All three phase voltages (VoltageL1, VoltageL2, VoltageL3) remained relatively stable, hovering around the 250V mark, which indicates a healthy supply system. The one noticeable dip around early 2024 could be attributed to a brief fault or sensor issue, but otherwise, the data shows no major signs of voltage instability.

⚙️ Current Load Behavior
The current values (CurrentL1, CurrentL2, CurrentL3) followed expected patterns, with minor variations between the three phases. The cleaned data ensures that any extreme or faulty readings have been removed, allowing us to clearly observe how the load is distributed across the phases. This is important for detecting phase imbalance, which can affect equipment performance and power quality.

## Secondary Exploration Data Analysis 
After cleaning the dataset, we dove into exploratory data analysis (EDA) to better understand the behavior and relationships between key electrical features—namely active power, voltage (L1–L3), and current (L1–L3). These insights will help us in later modeling steps like feature extraction and forecasting.

## 📌Summary Statistics
We started with basic summary statistics to get a feel for the data range and distribution.

Datetime entries span from July 2023 to March 2025, providing a solid timeline for time series forecasting.

Active Power ranges from 0 to 106 kW, with a median of 5 kW.

Voltage across all three lines (L1, L2, L3) is fairly consistent, centered around 252–254 V.

Current values are more variable, with maximums reaching over 300 A on some phases, hinting at high load demands.

## 🔍 Pairwise Scatter Plots
We generated pairwise scatter plots to observe relationships between all combinations of features.
This helped in identifying:

Correlations, such as between voltage and current

Clusters or outliers, which may influence model training later

Linear or nonlinear patterns, which guide model selection

## 📦 Boxplots
Boxplots provided a quick visual for spotting:

Skewed distributions

Outliers that were missed during z-score filtering

Comparison of feature ranges (e.g., Voltage vs Current)

## 📈 Histograms
Histograms showed the distribution shape of each variable:

Some features like voltage had a narrow, symmetrical spread

Others like current had right-skewed distributions, showing occasional high spikes in load

These insights are useful for normalizing and selecting proper transformations before modeling.

## 🔺 3D Scatter Plots
We visualized Active Power vs. Voltage vs. Current in 3D scatter plots for each phase (L1, L2, L3).
These plots:

Helped us understand how these core features interact

Suggested phase-wise differences in current draw

Revealed nonlinear relationships that may benefit from advanced ML models (like SVM or neural networks)

## 📈 Correlation Analysis
We began by calculating the correlation matrix for all numeric features (excluding Datetime). A heatmap was then used to visualize the strength and direction of these relationships.

Key Findings:

Active Power had a very strong positive correlation with all three current phases (L1–L3), especially CurrentL2 (0.9949), indicating current values are reliable predictors of power consumption.

Voltage values were positively correlated among themselves, but negatively correlated with both current and active power — typical behavior in power systems.

This analysis confirmed that Voltage and Current behave inversely, especially under high loads, aligning with expected electrical theory.

![Correlation Heatmap Placeholder]

## 🧠 Principal Component Analysis (PCA)
To simplify the dataset while retaining most of its variance, we applied PCA on the standardized numeric features.

Steps Taken:

Standardized the data using z-score normalization

Applied MATLAB’s pca() function to extract principal components

Generated visualizations for variance, score plots, and feature contributions

🔍 Key Insights:
PC1 and PC2 together explained over 99% of the total variance, indicating that the dataset is highly compressible

The Pareto chart helped us understand how much variance each principal component contributed

2D and 3D PCA plots showed clear structure and separability in the data

The PCA biplot revealed that Active Power and Currents dominate PC1, while Voltage variables contributed more toward PC2
![pca biplot](https://github.com/user-attachments/assets/b406fa0e-6d27-4d70-b630-665f66655bf7)
![pca 3d](https://github.com/user-attachments/assets/ab0f1d2b-5a23-44d8-9440-9c9553df09d5)

## 🛠️ Feature Engineering
We created new features that capture time patterns and power behavior:

Hour of the day: To track daily usage trends

Day of the week: To observe weekly demand patterns

Total current: Combined current from all three phases

Lag feature: Active power from the previous time step

Rolling average: Smoothed average of active power over 3 time steps

Target variable: Active power one step ahead (for supervised learning)

These features help the model understand when and how much power is typically used.

## 📈 Feature Exploration
We removed rows with missing values (caused by lag/rolling calculations) and then explored the relationships between the engineered features.

✅ Summary Statistics
Hour ranged from 0 to 23, confirming coverage across the day

Total current peaked at over 890 A, highlighting some heavy usage spikes

Rolling averages and lag features were aligned closely with current power behavior

🔍 Correlation Matrix
We analyzed correlations to find the most predictive features.

Highlights:

Lag features and rolling averages were highly correlated with current power

Hour showed a weak to moderate correlation, which is still useful for pattern learning

Current total had a strong positive correlation with power-related features

A heatmap helped visualize these relationships quickly.

## 📊 Visualizations
We used several plots to inspect feature behavior:

Pairwise scatter plots showed strong linear relationships between power, current, and engineered features

Boxplots highlighted value ranges and helped spot potential outliers

Histograms revealed the distribution of each feature — useful for deciding on scaling or transformations
## WEEK 9 and 10 Blog update

🔋 Smarter Load Forecasting: How We Predicted Energy Demand Using ML
⚙️ Introduction
As energy systems become smarter and more dynamic, accurately predicting electrical load is crucial for efficiency and grid stability. In our EE461 project at USP, we explored load forecasting using feature engineering and three different machine learning models: Linear Regression (LR), Support Vector Regression (SVR), and Gradient Boosted Regression Trees (GBRT). Our goal was to forecast future power consumption based on smart meter time-series data.

## 🧠 Feature Engineering
We engineered 16+ predictive features from raw voltage and current data including:

Rolling RMS of current phases (L1, L2, L3)

Voltage imbalance and lag features

Current imbalance and its relative ratio

Mean and rolling values of active/reactive power

Lag features of reactive power

These features were visualized using scatter plots, box plots, and histograms to understand data distribution, outliers, and patterns.

## 🔍 PCA & Correlation Analysis
We used Principal Component Analysis (PCA) to reduce feature dimensionality. The first principal component alone explained 75% of the variance, highlighting strong correlation among features like CurrentL3, current_mean, and ActivePower.

A correlation heatmap revealed which variables were strongly aligned. This helped us identify potential multicollinearity and focus on the most influential predictors.

## 🧪 Model Training & Testing
We trained the following models using a 70/30 train-test split:

Linear Regression (LR)

Support Vector Regression (SVR)

Gradient Boosted Trees (GBRT)

To evaluate performance, we used MAPE (Mean Absolute Percentage Error).

## 📉 Results:
Model	MAPE (%)
LR	0.00
SVR	44.51
GBRT	0.73

Linear Regression showed almost perfect performance—potentially a sign of overfitting or ideal feature-target alignment.

SVR struggled with generalization in the test period, especially after Oct 2024.

GBRT balanced accuracy and generalization, performing best overall.

## 📊 Visual Insights
Figure 10–12: Plots show individual model predictions vs. actual load.

Figure 13: A combined plot compared all models visually.

The GBRT model’s green line almost perfectly follows actual load (black line), especially during test data—confirming it as our best model.

![combined forcast ](https://github.com/user-attachments/assets/06318d27-ce3a-4133-b437-cacf8e0ade5b)
![linear regression ](https://github.com/user-attachments/assets/9061ed73-90aa-4349-8f75-80a051013766)
![gbrt ](https://github.com/user-attachments/assets/3aec48fa-207d-4474-9849-44e5a791476f)
![svr](https://github.com/user-attachments/assets/6000e89f-5113-4630-b783-b4033fbb2a7e)

## week 11 and 12
⚡ Redefining the Forecast: Corrected Models for Load Prediction
In my initial approach to load forecasting, I applied smoothing techniques to the dataset before training the models. At the time, it seemed like a logical way to reduce noise and improve clarity. However, I later realized that this introduced bias and reduced the models’ ability to learn important fluctuations in the data — especially during peak loads and sudden transitions.

These smoothing-based models produced forecasts that looked accurate visually but failed to generalize well during real-time testing. After identifying this issue, I decided to rebuild the models from scratch — this time without smoothing — using a clean, raw version of the data and proper preprocessing techniques.

## 🧼 Step 1: Clean Data, No Smoothing
This time, I focused on preserving the natural behavior of the data. I:

Removed missing entries and zero-load rows

Selected only meaningful numeric features (voltage, current, etc.)

Applied z-score normalization (no smoothing filters) for consistency

This ensured that the models trained on accurate, unaltered load behavior — improving their ability to generalize.

## 🤖 Step 2: Training the Corrected Models
With the clean data ready, I trained three core models:

Linear Regression (LR):
A fast and simple baseline model that captures linear trends.

Gradient Boosted Regression Trees (GBRT):
An ensemble method that builds a series of decision trees, learning from previous errors. It’s strong on complex data.

Support Vector Regression (SVR):
A kernel-based model that handles non-linear relationships well, particularly effective for time-series data.

Each model was trained on 70% of the data, and predictions were made across the full dataset.

## 📉 Step 3: Performance Evaluation
I used Mean Absolute Percentage Error (MAPE) to evaluate how well each model performed:

GBRT delivered the most accurate forecasts with the lowest MAPE.

SVR handled fluctuations and trends well, with solid results.

LR performed reasonably, but struggled with non-linear behavior.

These corrected models far outperformed the earlier smoothed versions, especially in real-world test scenarios.

## 🔍 Step 4: Zoomed Forecast Analysis
To visualize accuracy, I zoomed in on a forecast window from September 1 to September 20, 2024. A red marker indicated the split between training and test data. This allowed me to clearly compare actual vs predicted load and see how each model handled unseen data.

The corrected models captured patterns more sharply — especially GBRT, which aligned almost perfectly with actual usage.
![all models](https://github.com/user-attachments/assets/d5dcf192-1dd2-439d-ab4c-766855e77666)
![svr](https://github.com/user-attachments/assets/9ae4dcfb-6d91-4aa2-9a24-a66473dcd423)
![gbrt](https://github.com/user-attachments/assets/b3e9ea65-a6ab-404b-843c-05836caba8a0)
![lr](https://github.com/user-attachments/assets/50030c97-b3ab-456a-b69d-6bf90519f074)
![zoomed models](https://github.com/user-attachments/assets/b8b8c47b-9511-49cd-972d-9040750166d6)

### 📘 Project Blog Update – Week 13 & 14
## 🔎 Week 13 Blog Update: Forecasting with Feedforward Neural Networks
This week marked a major step forward in our journey toward accurate load forecasting using machine learning. We transitioned from data wrangling and exploratory analysis to implementing our first neural network model—a Feedforward Neural Network (FNN).

## 🧠 Why Feedforward Neural Network?
Feedforward NNs are the backbone of traditional machine learning models in the neural world. They are simple yet powerful when it comes to mapping complex nonlinear relationships—exactly what we needed to begin predicting active power demand from our feature-rich dataset sourced from a garment factory in Suva.

## 🛠️ Step-by-Step Implementation
📌 Feature Selection
We handpicked a robust set of features from our cleaned dataset:

Temporal patterns (day of week),

Current & voltage stats (mean, std, imbalance),

Rolling power values (to capture trend),

Lag values (to learn short-term dependencies).

These were carefully selected to feed into the neural network and give it context about the underlying load behavior.

🧪 Data Preparation
We structured the input features (X_all) and output targets (Y_all) as per MATLAB’s Neural Network Toolbox requirements (with transposed dimensions).

We also applied a sequential time-based split:

70% for training,

15% for validation,

15% held back as a hidden test set to assess real-world performance.

## 🧰 Model Training via Neural Net Fitting App
Using MATLAB’s nnstart, we launched the Neural Network Fitting App, configured a simple FNN (with 10 hidden neurons and default settings), and trained it on the selected dataset.

📈 Evaluation & Results
Once trained, we tested the model on the unseen test data.

## 🔢 Evaluation Metrics:
RMSE: Root Mean Square Error

MAE: Mean Absolute Error

MAPE: Mean Absolute Percentage Error



![fnn](https://github.com/user-attachments/assets/e973822e-6dfc-48b2-97a4-79b74e7088af)

### 🧠 Week 14 Blog Update: Time Series Forecasting with LSTM Networks
Following our work with Feedforward Neural Networks last week, Week 14 was all about stepping into more advanced territory—LSTM (Long Short-Term Memory) networks—specifically designed for handling time-dependent data like electrical load patterns.

LSTM networks are a type of recurrent neural network (RNN) that can remember patterns over long sequences, making them ideal for forecasting applications where past values influence future behavior.

## 🔍 Why LSTM?
Unlike FNNs that treat each data point independently, LSTMs retain memory of previous time steps. In our case, this allows the model to:

Learn seasonal trends (e.g., weekday vs. weekend loads),

Track load momentum over hours or days,

Improve predictions for nonlinear and volatile demand.

🛠️ How We Built It
📅 Input Setup
We used the same cleaned dataset, but this time reshaped it into sequences suitable for LSTM input. Each sample consisted of several time steps of features leading up to the current time, with the goal of predicting future active power demand.

## ⚙️ Network Architecture
Sequence Input Layer

LSTM Layer (e.g., 50 hidden units)

Fully Connected Layer

Regression Output Layer

This architecture was manually built using MATLAB's Deep Network Designer tool due to GUI limitations (e.g., missing regression layer by default). We used a workaround by forcing the addition of a regression output through code.

🧪 Training
We trained the model on sequential time windows using:

80% training,

20% testing,
ensuring the temporal order was preserved (no random shuffling).

## 📊 Results & Comparison
After training, we evaluated the LSTM model's predictions on the test set and plotted Actual vs. Predicted Load over time.

## 📈 Performance Metrics:
RMSE: 20.12

MAE: 15.89
![lstm](https://github.com/user-attachments/assets/26fbdbde-b991-432a-92e3-1f6f688283c0)

