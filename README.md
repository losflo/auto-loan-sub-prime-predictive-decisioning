## Data Set:

The dataset belongs to Pronto Finance, Inc. A sub prime secondary auto loan company. Pronto has a homegrown underwriting and loan management system. Over the last 5 years we have collected data on applications, loans, and collections. This dataset contains 40k+ applications.

## Research Question
How accurately can we predict if an application will be approved or denied?

## Business Understanding

In prime auto lending, predictive behavior is black and white. Most customers have a credit score which is a good indicator of risk. The chances of a customer lying about their credit score are much lower than in sub prime lending.

In sub prime auto lending, predictive behavior is gray. There is an art to predicting risk and whether a customer will pay back their loan. Before we can do that there has to be consistency in how we decision applications. The point of part 1 of this project is to verify that we are making the same decisions for similar applications so we can build on that foundation.

# Part 1: 1_application_approval_risk_discovery.ipynb
In this part we are trying to predict if an application will be approved or denied. If there is a mismatch in the decisions, we want to know about it.
For this reason, we will use recall as our metric. Per the definition of recall, we want to maximize the true positives at the expense of the false positives. This is fine for now since we will be manually reviewing the mismatches, but once we are confident in the model we will want to switch to precision as this will help us reduce the number of false positives. Therefore reducing the number of mismatches.

# Part 1: Findings
KNN performed the best with a recall score of 99.9%.

# Part 2: 2_write_off_risk_discovery.ipynb
In this part we are trying to predict if a loan will be written off. I'm taking into consideration the structure of the loan as well as the customer's profile.

# Part 2: Findings
KNN performed the best with a recall score of 77.5%. Using Permutation Importance, we found that industry was the most important feature.

After further analysis into the industry feature, we found Transport and Real Estate were the worst performing industries. with a 55%, and 48% write-off rate respectively.

# Part 3: Future Work
The ability to take the insights from Part 2 and feed them back into Part 1 would be valueable. Assessing the risk of an application based on the industry or any other feature provides a more holistic view of the risk. You could use the insights from Part 2 to guide the scoring in Part 1, flag items for further review, or even reject applications outright.