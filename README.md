### Project Title
Auto Loan Sub Prime Predictive Decisioning
 - Carlos Flores

#### Rationale

In prime auto lending, predictive behavior is black and white. Most customers have a credit score which is indicative of risk. 

In Sub-prime auto lending, predictive behavior is gray, demanding more nuanced evaluation as borrower behavior and risk factors are less transparent. Past patterns show that sub-prime applicants often present inconsistent or deceptive information, making risk assessment more challenging than simple credit score analysis. With such inconsitent data, it is difficult to make consistent decisions.

Enter machine learning models:
Trained on the historical decision data of senior buyers, the models effectively capture their collective knowledge and decision-making logic. Not only will this increase decision consistency, but it will also identify patterns in the data that may have been missed - including those subconscious even to the senior buyers.

#### Research Questions
Evaluate a models capacity to correctly classify application approvals.
Implement a model in production.

#### Data Sources

The dataset belongs to Pronto Finance, Inc. A sub prime secondary auto lending company. Pronto has a homegrown underwriting and loan management system. Data on applications, loans, and collections has been collected over the last 5 years. This dataset contains 40k+ applications.

data/applications.csv
 - data includes:
    - customer employment
    - income
    - credit score
    - vehicle information
    - loan structure

 - target variable: 'status'
    - Approved
    - Counter
    - Funded
    - Pending Approval
    
    - Denial

- the target variable will be simplified to Approved or Denial as follows:
    - Approved
        - Approved
        - Counter
        - Funded
        - Pending Approval
    - Denial
        - Denial

#### Outline of Project

##### Part 1-3: 
This implementation focuses on a binary classification model that predicts application approval decisions, trained on historical decisions made by senior buyers. The primary objective is to increase decision consistency across the buying process. By manually reviewing discrepancies between model predictions and actual buyer decisions, we achieve two critical goals: measuring model performance and ensuring decision consistency among our buying team.

The model optimization prioritizes recall as the primary performance metric, deliberately favoring true positive predictions even at the cost of increased false positives. In practical terms, this approach accepts a higher rate of potential false approvals to maximize true approvals. This trade-off is acceptable in our context as all prediction mismatches are manually reviewed providing an additional layer of validation.


##### Part 1: Machine Learning Models: Findings: [1_machine_learning_models.ipynb](https://github.com/losflo/auto-loan-sub-prime-predictive-decisioning/blob/main/1_machine_learning_models.ipynb)
KNN performed the best with a recall score of 99.9%.

##### Part 2: Ensemble Systems: Findings: [2_ensemble_systems.ipynb](https://github.com/losflo/auto-loan-sub-prime-predictive-decisioning/blob/main/2_ensemble_systems.ipynb)
Random Forest performed the best with a recall score of 80.5%.

##### Part 3: Neural Networks: Findings: [3_neural_networks.ipynb](https://github.com/losflo/auto-loan-sub-prime-predictive-decisioning/blob/main/3_neural_networks.ipynb)
Neural Networks performed the best with a recall score of 82.1%.

##### Part 1-3: Conclusion
Although KNN scored highest on the test set recall score, it's evaluation on real world data did not perform well. 

This issue could be due to a couple of reasons:
- The data is not representative of the real world data.
    - Data Distribution mismatch:
        - Think of this as an area of expertise. A model trained on a scale of 1-10 will not perform well outside of that scale. 
        
- The model is overfitting the data.
    - The training set is not diverse enough to represent the real world data.

- Poor feature engineering
    - Missing key features that heavily impact a real world decision will cause the model to not perform well.
        - I encountered this when evaluating the model's performance. A few key features in the credit report were not initially included in the features. Once included, the model's real world performance improved.
    - Conversly including too many features that are not relevant to the decision will cause the same issue.
    
- Concept Drift:
    - Over time, the concept of what is approved changes. This could be due to a change in the economy, the industry, the customer base etc.
    - Too much historical data may have been included in the training set. Our senior buyers do not buy the same now as they did 5 years ago. This may cause the model to detect patterns that are no longer relevant.


##### Part 4: Production API: [4_production_api.ipynb](https://github.com/losflo/auto-loan-sub-prime-predictive-decisioning/blob/main/4_production_api.ipynb)
**NOTE: This notebook is not meant to be run, it is meant to be a reference.

The final portion of this project focuses on production deployment of a machine learning model through a Flask-based API service. While FastAPI was demonstrated during office hours, Flask was chosen as it already exists in our production stack. 

The implementation handles three key components: 
(1) data ingestion from internal APIs with transforms to match the model's input requirements 
(2) model prediction
(3) prediction storage in a database for ongoing model evaluation and monitoring

This architecture enables seamless integration of the model into our production workflow while facilitating performance tracking. While the notebook included is specific to the model used in this project, the same steps can be applied to any application.

#### Next Steps
Production machine learning models typically experience performance degradation over time due to data drift, where the relationship between features and target variables evolves. Simply put, the basis of an approval changes over time. This drift can be attributed to various factors including economic conditions, industry dynamics, shifting customer demographics, and changes in deal acquisition strategies.


To maintain model effectiveness implement a monitoring and retraining pipeline:

1. Periodically evaluate the model's performance on it's prediction and the true decision.
2. If the model's performance is below a threshold re-train the model with the latest data.
3. Upload the new model to the server.
4. Update the server code above to use the new model.
5. Repeat.

Implementing this pipeline ensures a model in production is always up to date and performing at a high level.