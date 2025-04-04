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


