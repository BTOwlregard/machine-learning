## Udacity MLND: Capstone Project Proposal
### Predicting Credit Card Defaults: An Investigation of Pre-Crisis Taiwanese Credit Card Data

### Domain Background </br>
Between 2001 and 2005, the amount of money that Taiwanese consumers spent using credit cards nearly doubled.1 In the midst of this striking growth, however, Taiwanese banks and credit card issuers failed to properly predict and control for the related credit risks. In 2006, Taiwan's local banks wrote off nearly $5 billion (USD) in bad loans, 70% of which were credit card accounts.2 The ensuing credit crisis put a stop to the rapid growth of revolving consumer debt in Taiwan and credit card utilization remained flat for several years, as some banks were forced by local regulators to stop issuing credit cards and other issuers pulled out of the Taiwanese market entirely. </br> </br>
Taiwan's consumer credit crisis, and the global financial crisis that would begin the following year, are instructive examples of how uncontrolled loan losses can lead banking systems, and entire economies, to grind to a halt. In view of these consequences, it is important that banks and other debt issuers are able to accurately predict the riskiness of their loan portfolios. In the context of credit cards, this means identifying customers who are unlikely to pay off their balances and designing interventions to reduce loss exposure while still serving customers' credit needs. 
</br> 
### Problem Statement </br>
Credit card issuers have an interest in predicting loan defaults (non-payment) in their portfolios, so that they can implement policies to reduce their risk exposure (from adverse actions like reducing the credit available to a customer or sending the accounts to collections, to more benign interventions like credit counseling or offers of reconsolidation of debts). Banks have a limited set of data with which to make these predictions, and some of that data may only be acquired from third party bureaus, the reliability of which will vary country to country.3 Moreover, issuers will want to ensure that interventions intended to control credit risk do not adversely impact reliable customers or violate consumer finance regulations. This means that that issuers may need not just a prediction as to whether a cardholder is more likely than not to pay their bills, but also:
* a reliable estimate of the probability of default
  - different decision boundaries may be appropriate for different interventions
  - e.g., if the planned action is cutting off a customer's accounts and selling the loan to a bill collection agency, the cost of a false positive is much higher than if the planned action were a simple email reminder of their next bill's due date, and we may set a lower decision boundary (in term of predicted probability of default) in the latter case

* interpretability of the predictive models 
  - regulators may require that predictive models meet certain standards of interpretability, or that customers' be provided with clear reasons why their applications for or access to credit has been restricted 4
  - on the other hand, there may be situations where a predictive model is meant only for internal use by the issuer or where the usage is subject to different regulations (e.g. determining who should receive certain marketing or account messaging), and the issuer may be willing to sacrifice interpretability and simplicity for predictive power

Thus, there is unlikely single model of customer credit behavior that serves all of a credit issuer's needs. Rather, issuers must identify a suite of models that serve different business needs. With this in mind, this project will have several distinct goals:
* to test a variety of modeling approaches with the goal of predicting credit card payment default
* to identify which customer performance variables are most important to predicting future default
* to identify the modeling approach with the best predictive performance, based on our chosen evaluation metrics (see below), with no restrictions on features
* to develop another best-performance model, this time under a set of simulated regulatory and business restrictions (i.e. no demographic variables, and no neural networks or difficult-to-interpret high order model features)
* to explain how our choice of model and decision boundary would change depending on the relative cost of false negatives and false positives 
* to classify all tested modeling approaches according to the strength of prediction and interpretability of the model, and identify situationsin which we may choose these models over other approaches 

### Dataset and Inputs 
### Proposed Solution
### Benchmark Model 
### Evaluation Metrics 
### Project Design

###References
1. The Payment and Settlement Systems in the Republic of China (Taiwan), Central Bank of the Republic of China (Taiwan), 2010, https://www.cbc.gov.tw/public/Data/010269422971.pdf
2. Taiwan's Credit Crisis: The calm after the storm, Cards International - Issue 375, VRL Financial News, 2007, http://www.vrl-financial-news.com/cards--payments/cards-international/issues/ci-2007/ci375/taiwan%E2%80%99s-credit-crisis-the-ca.aspx
3. Credit Bureaus in Asia, Asia Focus, Federal Reserve Bank of San Francisco, 2011, https://www.frbsf.org/banking/files/october-credit-bureaus-in-asia-Oct-2011.pdf
4. Adverse Action Notice Requirements Under the ECOA and the FCRA, Consumer Compliance Outlook, 2013, https://consumercomplianceoutlook.org/2013/second-quarter/adverse-action-notice-requirements-under-ecoa-fcra/
