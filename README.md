# IBM-Data-Science-Capstone-Project

# Space Y Rocket Launch Analysis and Prediction

## Background of the Project
Space Y is a new aerospace company looking to compete with SpaceX in the commercial rocket launch industry. The project focuses on collecting and analyzing SpaceX's Falcon 9 launch data to predict the likelihood of rocket stage reuse and estimate the cost of each launch. Data is gathered using the SpaceX API and web scraping techniques to create a comprehensive dataset for analysis and machine learning modeling.

## Executive Summary
In this project, we use the SpaceX REST API and web scraping techniques to collect data on past SpaceX Falcon 9 launches. The dataset contains valuable information about rockets, payloads, landing outcomes, and more. We aim to clean and analyze this data, create visualizations, and build a predictive machine learning model to determine whether SpaceX will attempt to land a rocket. An interactive dashboard will provide insights into key metrics, helping Space Y optimize its rocket launch operations and reduce costs through reuse of rocket stages.

## Methodology

### Data Collection - API
1. **API Endpoint**: We gather SpaceX launch data from the endpoint `api.spacexdata.com/v4/launches/past`.
2. **Requesting Data**: Using the `requests` library, we perform a GET request to retrieve the past launch data.
3. **JSON Parsing**: The API response is a JSON list of launch objects. We parse the data using `.json()` and then normalize the JSON structure into a flat table using the `json_normalize()` function.
4. **Handling Rocket IDs**: Some columns, like the rocket column, contain IDs instead of descriptive data. We use additional API endpoints (e.g., `/rockets`, `/launchpads`) to retrieve relevant details.
5. **Filtering Data**: We filter the dataset to include only Falcon 9 launches and exclude Falcon 1 launches.
6. **Handling Null Values**: Missing values in the `PayloadMass` column are replaced with the calculated mean, while NULL values in `LandingPad` are left for later handling with one-hot encoding.
7. The cleaned dataset is **exported to a CSV file** for further analysis.

### Data Collection - Web Scraping
1. Falcon 9 launch data is **scraped from Wikipedia** using `BeautifulSoup`.
2. **HTML Parsing**: We create a `BeautifulSoup` object from the HTML response and extract table headers and data from the HTML tables.
3. A **dictionary** is created from the parsed data, which is converted into a Pandas DataFrame.
4. The cleaned dataset is **exported to a CSV file** for further analysis.

### Data Wrangling
1. We transform landing outcomes into binary values: 1 for successful landings and 0 for unsuccessful landings.
2. **Sampling Data**: The data is further sampled and cleaned to include only relevant variables for analysis and modeling.

### EDA with Visualization
1. **Visualizations** are created to analyze relationships between variables such as payload mass, booster version, and launch success.
2. **Comparisons** of launch success rates across different payload types, launch sites, and booster versions are visualized to identify key trends.

### EDA with SQL
1. **SQL queries** are used to explore the dataset further, allowing us to analyze launch success rates by site, booster version, and payload type.

### Maps with Folium
1. **Maps are created using Folium** to visualize launch sites, launch outcomes, and their proximity to coasts and the equator, which can influence launch success.

### Dashboard with Plotly Dash
1. An **interactive dashboard** is built using Plotly Dash to monitor and visualize key metrics:
   - A **Pie chart** showing the proportion of successful launches.
   - A **Scatter chart** displaying Payload Mass vs. Success Rate, categorized by Booster Version.

### Predictive Analytics
1. The dataset is prepared for modeling by creating a **NumPy array** of class labels (successful landing or not).
2. **Standardization**: We standardize the data using `StandardScaler` and split it into training and testing sets with `train_test_split`.
3. **GridSearchCV** with cross-validation (`cv=10`) is applied to optimize hyperparameters for several machine learning algorithms:
   - Logistic Regression (`LogisticRegression()`)
   - Support Vector Machine (`SVC()`)
   - Decision Tree (`DecisionTreeClassifier()`)
   - K-Nearest Neighbor (`KNeighborsClassifier()`)
4. The models are evaluated on the test set using accuracy scores and confusion matrices to measure performance.
5. **Best Model Selection**: The best-performing model is selected based on Jaccard Score, F1 Score, and Accuracy.

### Conclusion
- **Model Performance**: The decision tree model slightly outperforms the other models, showing higher accuracy in predicting whether a rocket will land successfully.
- **Launch Geography**: Most launch sites are located near the equator to benefit from Earth's rotational speed, which reduces fuel costs.
- **Coastal Proximity**: All launch sites are near the coast, ensuring safer landings and recoveries.
- **Success Rates**: Over time, launch success rates have improved due to SpaceX's advancements in rocket technology.
- **Key Insights**: KSC LC-39A has the highest success rate, and higher payload masses are generally associated with higher success rates.

