## Udacity MLND: Capstone Project Proposal
### Predicting Credit Card Defaults: An Investigation of Pre-Crisis Taiwanese Credit Card Data

### Domain Background </br>
Between 2001 and 2005, the amount of money that Taiwanese consumers spent using credit cards nearly doubled. [1] In the midst of this striking growth, however, Taiwanese banks and credit card issuers failed to properly predict and control for the related credit risks. In 2006, Taiwan's local banks wrote off nearly $5 billion (USD) in bad loans, 70% of which were credit card accounts. [2] The ensuing credit crisis put a stop to the rapid growth of revolving consumer debt in Taiwan and credit card utilization remained flat for several years, as some banks were forced by local regulators to stop issuing credit cards and other issuers pulled out of the Taiwanese market entirely. </br> </br>
Taiwan's consumer credit crisis, and the global financial crisis that would begin the following year, are instructive examples of how uncontrolled loan losses can lead banking systems, and entire economies, to grind to a halt. In view of these consequences, it is important that banks and other debt issuers are able to accurately predict the riskiness of their loan portfolios. In the context of credit cards, this means identifying customers who are unlikely to pay off their balances and designing interventions to reduce loss exposure while still serving customers' credit needs. 
</br> 
### Problem Statement </br>
Credit card issuers have an interest in predicting loan defaults (non-payment) in their portfolios, so that they can implement policies to reduce their risk exposure (from adverse actions like reducing the credit available to a customer or sending the accounts to collections, to more benign interventions like credit counseling or offers of reconsolidation of debts). Banks have a limited set of data with which to make these predictions, and some of that data may only be acquired from third party bureaus, the reliability of which will vary country to country. [3] Moreover, issuers will want to ensure that interventions intended to control credit risk do not adversely impact reliable customers or violate consumer finance regulations. This means that that issuers may need not just a single binary prediction as to whether a cardholder is more likely than not to pay their bills, but also:
* a reliable estimate of the probability of default
  - different decision boundaries may be appropriate for different interventions
  - e.g., if the planned action is cutting off a customer's accounts and selling the loan to a bill collection agency, the cost of a false positive is much higher than if the planned action were a simple email reminder of their next bill's due date, and we may set a lower decision boundary (in term of predicted probability of default) in the latter case

* simplicity of implementation and model interpretability 
  - regulators may require that predictive models meet certain standards of simplicity or interpretability, or that customers' be provided with clear reasons why their applications for or access to credit has been restricted [4]; this may may limit the issuer's ability to use models with deep trees or complex interaction terms
  - on the other hand, there may be situations where a predictive model is meant only for internal use by the issuer or where the usage is subject to different regulations (e.g. determining who should receive certain marketing or account messaging), and the issuer may be willing to sacrifice interpretability and simplicity for predictive power

### Dataset and Inputs 
The dataset is a sample of 30,000 Taiwanese credit card account records from September of 2005. There are 25 variables. The dataset includes demographic information, credit limit, billing and payment history, and a binary flag indicating whether the customer defaulted on their September 2005 payment (i.e., failed to pay the minimum due amount for that billing period). [5][6] 
Summary of inputs, adapted from the Kaggle summary of the dataset:
* ID: ID of each client
* LIMIT_BAL: Amount of credit line in NT (Taiwanese) dollars 
* SEX: Gender (1=male, 2=female)
* EDUCATION: (1=graduate school, 2=university, 3=high school, 0,4+ = other/unknown)
* MARRIAGE: Marital status (1=married, 2=single, 3= divorced, 0=  other/unknown)
* AGE: Age in years
* PAY_0: Repayment status in September, 2005 (-2= No balance and no transactions (inactive this period); -1= Balance paid in full, but positive balance remains due to recent transactions; 0= Paid the minimum due, but less than the full balance;  1=payment delay for one month; 2=payment delay for 2 months, ... 8=payment delay for eight months, 9=payment delay for nine months and above)
* PAY_2: Repayment status in August, 2005 (scale same as above)
* PAY_3: Repayment status in July, 2005 (scale same as above)
* PAY_4: Repayment status in June, 2005 (scale same as above)
* PAY_5: Repayment status in May, 2005 (scale same as above)
* PAY_6: Repayment status in April, 2005 (scale same as above)
* BILL_AMT1: Amount of bill statement in September, 2005 (NT dollar)
* BILL_AMT2: Amount of bill statement in August, 2005 (NT dollar)
* BILL_AMT3: Amount of bill statement in July, 2005 (NT dollar)
* BILL_AMT4: Amount of bill statement in June, 2005 (NT dollar)
* BILL_AMT5: Amount of bill statement in May, 2005 (NT dollar)
* BILL_AMT6: Amount of bill statement in April, 2005 (NT dollar)
* PAY_AMT1: Amount of previous payment in September, 2005 (NT dollar)
* PAY_AMT2: Amount of previous payment in August, 2005 (NT dollar)
* PAY_AMT3: Amount of previous payment in July, 2005 (NT dollar)
* PAY_AMT4: Amount of previous payment in June, 2005 (NT dollar)
* PAY_AMT5: Amount of previous payment in May, 2005 (NT dollar)
* PAY_AMT6: Amount of previous payment in April, 2005 (NT dollar)
* default.payment.next.month: Default payment (1=yes, 0=no) 

</br>Categorical inputs, such as demographic variables, will be one-hot encoded prior to modeling. PAY_X variables can take on values from -2 to 9. Values from 1 to 9 are ordinal, and represent how months the customer was past due on their payments, but values from -2 to 0 all represent 0 months past due (customer is current on their payments), however with different balances statuses. Thus, the PAY_X variables are only partially ordinal. To account for this, we'll split the PAY_X variables into a few different variables: one ordinal with values 0-9 that indicate months past due, and another set of three one-hot encoded variables to cover the values from -2 to 0. 
### Proposed Solution
There is unlikely single model of customer credit behavior that serves all of a credit issuer's needs outlined in our Problem Statement. Rather, issuers must identify a suite of models that serve different business needs. The problem we have outlined does not have a single solution, but a range of possible solutions that represent different tradeoffs. With this in mind, this project will have several distinct goals:
* to test a variety of modeling approaches with the goal of predicting credit card payment default
* to identify which customer performance variables are most important to predicting future default
* to identify the modeling approach with the best predictive performance, based on our chosen evaluation metrics (see below), with no restrictions on features
* to develop another best-accuracy model, this time under a set of simulated regulatory and business restrictions (i.e. no demographic variables, and no neural networks or difficult-to-interpret high-order interaction terms)
* to explain how our choice of model and decision boundary would change depending on the relative cost of false negatives and false positives 
* to classify all tested modeling approaches according to both the strength of prediction and situational utility/interpretability of the model, and identify situations in which we may choose each model over other approaches 

</br>More broadly then, our proposed solution will consist of both the predictive models that we develop (best-accuracy models both with and without added constraints) and our evaluation of the suitability of the models we've tested and decision rules we've considered to different business scenarios. 
### Benchmark Model 
In a business context, many times we may begin a modeling project to improve on a policy that has been determined through a combination of simple data investigation and business intuition. In this instance, such an approach provides us with a simple benchmark model. In exploring the available data, we may note that we already have a record of whether the customers in our dataset paid their bill on time in the months prior to the period for which we want to predict payment. That is, we know how many months delinquent customers' are on their payments in August of 2005. This variable has an intuitive relationship with the default rate in the following month: the more months delinquent a customer is in August, the less likely they are (in general) to pay their bill in September. We can build a simple model on this single variable whereby p(default) = p(default | PAY_2 = X) = (# of defaults / total customers) among customers with PAY_2 = X. We adjust this model slightly by bucketing all values of PAY_2 > 3 together to force a monotonic relationship between months past due and our prediction. </br></br>
This simple model yields a Somers' D of 0.4128 and an F1 score of 0.4516 when the decision boundary is set at predicted p(default) = 0.5.

### Evaluation Metrics 
Model performance will be measured with Somers' D, which is a simple transformation of the area under the Receiver Operating Characteristic (ROC) curve. Somers' D = (2 * ROCAUC) - 1 </br></br>
The ROC curve expresses the tradeoff between true positive rate and false positive rate across a range of possible decision boundaries for a particular binary classification model.[7] The area under the curve thus provides a succinct summary of the model's performance agnostic of any particular choice of decision boundary. As such, it is appropriate to our task here, whereas we are interested in estimating the likelihood of credit payment default, a binary outcome, and may have reason to evaluate different decision boundaries for different uses of the model. </br>
An AUC of 1 would indicate a perfect model that capture all true positives without any true negatives, whereas an AUC of 0.5 is the expected performance of a completely random classifier. Somers' D rescales ROCAUC to a range of [-1,1], such that 0 represents the expected performance of a random model, which may be a more intuitive scoring range for our purposes. </br></br>
When evaluating a model with a particular decision boundary, such that our predictions are not a score or predicted probability of default but a binary (0 or 1) prediction, as a default we will evaluate predictive performance using the F1 score. The F1 score is a combination of precision and recall that falls in the range (0,1] and thus is an expression of classification accuracy that takes into account both the rate at which we capture true positives and how precise our positive predictions are. [8] </br></br>
Finally, while Somers' D and F1 score will be our default evaluation metrics, we will also introduce alternative measures of accuracy that weight true positive and false positives differently to help illuminate how different financial incentives may drive different choices of model or decision boundary.  

### Project Design
We must begin with some data preprocessing. We will perform separate transformations on the categorical and continuous features. Categorical variables (e.g. MARRIAGE) will be one-hot encoded. The PAY_X variables will be one-hot encoded in part (see Dataset and Inputs). We'll inspect continuous variables for excessive skew and perform a logarithmic transformations where appropriate to prevent extremely high or low values from  harming the performance of our learning algorithms. </br>
The dataset will be split into 3 subsets: build, validation, and test. We will then evaluate at least four modeling approaches:
* logistic regression (sklearn.linear_model.LogisticRegression)
* gradient boosted tree (sklearn.ensemble.GradientBoostingClassifier)
* random forest (sklearn.ensemble.RandomForestClassifier)
* neural network (keras with TensorFlow backend; fully connected layers, with sigmoid final activation)

</br>We will perform hyperparamter tuning using the validation set, and will report final model accuracy (both Somers' D and F1-score with decision boundary at predicted_probability = 0.5 -- see Evaluation Metrics) on the test set. From this process of modeling and evaluation, we will determine which model produces the highest accuracy (Somers' D); this will be our best-accuracy model. We will then impose some restrictions on our data according to a hypothetical business and regulatory constraints (remove demographic variables and restrict usage of neural networks and trees beyond a given depth), and re-run our modeling and hyperparameter tuning exercise to produce a best-accuracy model under these constraints. Having evaluated the accuracy of all the models tested, we will discuss the relative advantages of each model, identify reasonable decision boundaries under different scenarios (e.g. high/low relative cost of false positives), and suggest situations in which each may provide the best available classifier. Our final output will be structured as a series of hypothetical recommendations - i.e., the advice that we would give to a credit card issuer that has an interest in predicting credit card payment delinquency, and designing policies and interventions accordingly, based on this data.

### References
1. The Payment and Settlement Systems in the Republic of China (Taiwan), Central Bank of the Republic of China (Taiwan), 2010, https://www.cbc.gov.tw/public/Data/010269422971.pdf
2. Taiwan's Credit Crisis: The calm after the storm, Cards International - Issue 375, VRL Financial News, 2007, http://www.vrl-financial-news.com/cards--payments/cards-international/issues/ci-2007/ci375/taiwan%E2%80%99s-credit-crisis-the-ca.aspx
3. Credit Bureaus in Asia, Asia Focus, Federal Reserve Bank of San Francisco, 2011, https://www.frbsf.org/banking/files/october-credit-bureaus-in-asia-Oct-2011.pdf
4. Adverse Action Notice Requirements Under the ECOA and the FCRA, Consumer Compliance Outlook, 2013, https://consumercomplianceoutlook.org/2013/second-quarter/adverse-action-notice-requirements-under-ecoa-fcra/
5. Default of Credit Card Clients Dataset, Kaggle, accessed Mar. 13, 2018, https://www.kaggle.com/uciml/default-of-credit-card-clients-dataset
6. UCI Machine Learning Repository, University of California, Irvine, accessed Mar. 13, 2018, https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients
7. Receiver operating characteristic, Wikipedia, accessed: Mar. 16, 2018, https://en.wikipedia.org/wiki/Receiver_operating_characteristic
8. F1 Score, Wikipedia, accessed: Mar. 16, 2018, https://en.wikipedia.org/wiki/F1_score
