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



    ![active power](https://github.com/user-attachments/assets/76d2e883-3d78-4286-8ea9-dc8d396d69e2)




