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

# EE461 Project Blog: Week 7-8
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
# plot the cleaned data 

