# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Sibeso Mubonda, `sibeso.mubonda@gwu.edu`
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Project.ipynb](DNSC_6301_Project.ipynb)

### Intended Use
* **Primary intended uses**: This model is a probability of default classifier, with a case for determining eligibility for a credit line increase.
* **Primary intended users**: DNSC 6301 bootcamp professor and fellow students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* * **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* * **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

* Data dictionary: 


| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |


### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')`
```
### Quantitative Analysis

* **Metrics used for evaluation**: 
* Area Under the Curve (AUC) and Adverse Impact Ratio(AIR) are metrics used for evaluation.
* Adverse Impact Ratio(AIR) is the rate of positive outcome for a protected group. The standard AIR is 0.8 or 80%. An AIR of below 0.8 is a major problem.  To calculate AIR, define the function AIR and return value in. For the first model, the AIR of hispanic to white in the race group is 0.76 at a cutoff of 0.15. This is below the standard value so we raise the cutoff to 0.18 and get an AIR of 0.83 which is acceptable.
* AUC is used to measure discrimination or bias in the model. An AUC of 0.80 is acceptable and anything above that is excellent. In the final model, the Hispanic to White AUC maxes out at a depth of about 7 which shows an AUC of above 0.80 which is acceptable and means the model exhibits high accuracy.

|  | Training Data  | Validation Data| Test Data|
| ---- | ------------- | ---------------- | ---------- |
|**AIR**| 0.87 | 0.76| 0.83 |
| **AUC(At depth of 6)** | 0.78| 0.75 | 0.84

#### Correlation Heatmap
![Correlation Heatmap](https://github.com/Sibeso04/Sibeso-6301-Project/blob/main/Correlation%20Heatmap.png)

### Decision Tree
![Decision Tree](https://github.com/Sibeso04/Sibeso-6301-Project/blob/main/Decision%20Tree%20.png)

#### Iteration Plot Final Model
![Iteration Plot.png](https://github.com/Sibeso04/Sibeso-6301-Project/blob/main/Iteration%20Plot.png)

### Ethical Considerations

* Model may incur some bias or discrimination from training data due to pattern of dependency on past outcomes. To determine whether a customer's next payment is deliquent, the training model refers to data from the past which may cause group biases. There may be group disparities due to past outcomes and this may lead to negative impacts on a group e.g., race bias or gender bias.

* Real-word risks of using the model involve hackers or security and privacy issues that may arise. It is possible that hackers may alter the algorithm and change data which may lead to discriminstation. 

* It was unexpected for the model not to have any missing values in the data needed for training the model. 
* Another unexpected thing was for the model to produce the Women to Men AIR it did both in the validation and test data. Because of this, we can see that human minds already have certain notions that may affect the machine algorithm and lead to bias and discrimination.

