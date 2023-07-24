---
title: "Testing the presence of racial discrimination in the labour market"
excerpt: "Formulate 4 different questions related to discrimination and build one model for each question."
collection: portfolio
---

Is discrimination different for people from Boston and Chicago?
---

Here we want to see if discrimination is different for people from Boston and Chicago. So we need the interaction between city and ethnicity. 

I will use this model to answer:

![model_1](..\..\images\second_images\img_1.png)

The fitted model is:

![model_1](..\..\images\second_images\fitted_1.png)


First I will interpret the coefficients:

![first](..\..\images\second_images\test_1.png)


$\beta_0$: The  probability of being called back for caucasians sounding names who live in boston is 5.03%.

$ethnicity$: The average probability of being called back for an individual with an African-American sounding name living in Boston is 6.13% lower than Caucasian sounding name in Boston.

$city$: The average probability of being called back for an individual with an Caucasian sounding name living in Chicago is 6.54% lower than in Boston.

$city \times ethnicity$: The difference between the average probability of being called back for the two ethnic groups for the two different city is 7.5%.

LPM's are heteroscedastic by construction, so all your tests and confidence intervals must be robust to heteroscedasticity.

Since we checking for discrimination for people from Boston and Chicago, if there was no discrimination, the probability of being called would be same for both ethnicity. So, inoreder to answer the question we need to check the significance of coefficient of ethnicity and $city \times ethnicity$. Notice that we donot need to check city since we are cheecking to see the difference between the two ethnicities but city is for the same etnicity.

Testing for African-American sounding names in Chicago VS Caucasian sounding names in Chicago: 

Using the robust F test:

![F test](..\..\images\second_images\test_2.png)

Since the p-value of the test is greater than 5%, we do not reject the Null hypothesis. Therefore, we can conclude that there is no significant difference in discrimination for for African-American sounding names in Chicago VS Caucasian sounding names in Chicago.

Testing for African-American sounding names in Boston VS Caucasian sounding names in Boston: 

Using the robust F test:

![F test](..\..\images\second_images\test_3.png)


Since the p-value of the test is greater than 5%, we do not reject the Null hypothesis. Therefore, we can conclude that there is no significant difference in discrimination for African-American sounding names in Boston VS Caucasian sounding names in Boston.

Testing for African-American sounding names in Chicago VS African-American sounding names in Boston: 

Using the robust F test:

![F test](..\..\images\second_images\test_4.png)

Since the p-value of the test is greater than 5%, we do not reject the Null hypothesis. Therefore, we can conclude that there is no significant difference in discrimination for African-American sounding names in Chicago VS African American sounding names in Boston.


Computing robust confidence intervals:

![C interval](..\..\images\second_images\test_5.png)

Is discrimination different for male and female individuals with prior military experience having African-American sounding name?
---

For question 2, I want to see if discrimination is different for male and female that have prior militay experience or not. Since I am checking for discrimination, I will need to have "ethnicity" in my model. Other than that, I need to interact "ethnicity" with "military" and "gender", so that if the person is African-American,is male and has military expeirence, the coefficient of that interaction will give me the probability of being called.  I will also include interaction between "gender" and "military". This will to differentiate for Caucasian male with military experience. I need the term "military" too so that if "gender" is 0 (female), "military" is 0 (no expereince) and ethnicity is Caucasian, the coefficienct of that term will give me the probability of being called for female Caucasian sounding name with military experience. Lastly, I need an interaction between ethnicity and military for African-American sounding name for females with no military background.  


LPM's are heteroscedastic by construction, so all your tests and confidence intervals must be robust to heteroscedasticity.

I will use this model to answer:


![model_2](..\..\images\second_images\img_2.png)

The fitted model is:

![Model](..\..\images\second_images\fitted_2.png)

First I will interpret the coefficients:

![Co.eff](..\..\images\second_images\test_6.png)

$intercept$: The  probability of being called back for female caucasian sounding names with no prior military experience is 1.5%.

$ethnicity$: The probability of being called back when applicants have female African-American sounding names with no military experience is 0.93 percentage less.

$military$: The probability of being called back when applicants have female Caucasian sounding names with military experience is 4.38 percentage higher.

$gender$: The probability of being called back when applicants have male Caucasian sounding names with no military experience is 2.44 percentage less.

$military \times ethnicity$: Having African-American sounding name changes the probability of being called by 9.7% percentage point less when the individual had military experience.

$military \times gender$: Being male changes the probability of being called by 4.33% percentage point less when the individual had military experience.

$ethnicity:gender$: Being male changes the probability of being called by 0.22% percentage point less when the individual had African-American sounding name.

There are a few conditions:\newline
1. Male African American sounding names with military experience VS Male Caucasian sounding names with military experience \newline 
2. Female African American sounding names with military experience VS female Caucasian sounding names with military experience\newline
3. Male African American sounding names with no military experience VS Male Caucasian sounding names with no military experience\newline
4. Female African American sounding names with no military experience VS Female Caucasian sounding names with no military experience\newline
5. Male African American sounding names with military experience VS Female African American sounding names with military experience\newline
6. Male African American sounding names with no military experience VS Female African American sounding names with no military experience\newline

Testing Condition 1:

Using the robust F test:

![F test](..\..\images\second_images\test_7.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for Male African American sounding names with military experience VS Male Caucasian sounding names with military experience.

Testing Condition 2:

Using the robust F test:

![F test](..\..\images\second_images\test_8.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for female African American sounding names with military experience VS female Caucasian sounding names with military experience.

Testing Condition 3:

Using the robust F test:

![F test](..\..\images\second_images\test_9.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for Male African American sounding names with no military experience VS Male Caucasian sounding names with no military experience.

Testing Condition 4:

Using the robust F test:

![F test](..\..\images\second_images\test_10.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for female African American sounding names with no military experience VS female Caucasian sounding names with no military experience.

Testing Condition 5:

Using the robust F test:

![F test](..\..\images\second_images\test_11.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for male African American sounding names with military experience VS female African American sounding names with military experience.

Testing Condition 6:

Using the robust F test:

![F test](..\..\images\second_images\test_12.png)

Since the p-value of the test if greater than 5%, we  reject the Null hypothesis. Therefore, we can conclude that there is  no significant difference in discrimination for male African American sounding names with no military experience VS female African American sounding names with no military experience.

Therefore, looking at all the cases, the answer to my question is: \textbf{There is no significant discrimination difference for male and female individuals with prior military experience having African-American sounding names.}

Computing robust confidence intervals:

![Confidence Interval](..\..\images\second_images\test_13.png)

Is the effect of experience on the probability of being called back the same for people provided email address and those that didnot?
---

I will need to add interaction between exprience and email since I am trying to check the specific relationship for the effect of experience on the probability of being called back the same for people who provided email address and who didnot.

The fitted model is:

![model](..\..\images\second_images\fitted_3.png)

LPM's are heteroscedastic by construction, so all your tests and confidence intervals must be robust to heteroscedasticity.

Printing coefficient table with robust s.e.:

![Co.eff](..\..\images\second_images\test_14.png)

First I will interpret the coefficients:

$intercept$: The the effect of experience on probability of being called back for people who didnot provided any email is 6.73%.

$ethnicity * experience$: The the effect of experience on probability of being called back when applicants have African American sounding names with no email is 0.53 percentage less.

$experience * email$: the effect of experience on the probability of being called is 1.55% percentage point more when the individual provided an email.

We need to test a few conditions:

1. African American sounding names who provided email VS Caucasian sounding names who provided email. \newline
2. African American sounding names who didnot provide email VS Caucasian sounding names who didnot provide email\newline

Testing Condition 1:

Using the robust F test:

![F test](..\..\images\second_images\test_15.png)

Since the p-value of the test is greater than 5%, we cannot reject the Null hypothesis. Therefore, we can conclude that the effect of experience on the probability of being called is not significant for African American sounding names who provided email VS Caucasian sounding names who provided email.

Testing Condition 2:

Using the robust F test:

![F test](..\..\images\second_images\test_16.png)

Since the p-value of the test is greater than 5%, we cannot reject the Null hypothesis. Therefore, we can conclude that the effect of experience on the probability of being called is not significant for African American sounding names who didnot provide email VS Caucasian sounding names who didnot provide email.

Therefore, looking at all the cases, the answer to my question is: There is no significant difference caused by the effect of experience on the probability of being called back for people who provided email address and those that didnot.

Computing robust confidence intervals:

![Confidence Interval](..\..\images\second_images\test_17.png)

Is discrimination different for people who mention some volunteering experience VS those that didnot for people applying to EOE?
---
Here, I want to see if discrimination is different for male and female that have prior volunteer experience applying to an EOE. Since I am checking for discrimination, I will need to have "ethnicity" in my model. Other than that, I need to interact "ethnicity" with "equal" and "volunteer", so that if the person is African-American has volunteer expeirence and applying to an EOE, the coefficient of that interaction will give me the probability of being called back.  I will also include interaction between "volunteer" and "equal". This will to differentiate for Caucasian vlunteer with volunteer experience or not. 

I will use this model to answer:

![model](..\..\images\second_images\img_3.png)

The fitted model is:

![Fitted model](..\..\images\second_images\fitted_4.png)

Printing coefficient table with robust s.e.:

![Co.eff](..\..\images\second_images\test_18.png)

First I will interpret the coefficients:\newline

$intercept$: The  probability of being called back for caucasian sounding names with no prior volunteer experience is and not applying to a EOE is 2.57%.

$ethnicity$: The probability of being called back when applicants have African-American sounding names with no volunteer experience applying to a EOE is 3.84 percentage less.

$volunteer$: The probability of being called back when applicants Caucasian sounding names with volunteer experience is 0.43 percentage less when applying to a non-EOE.

$equal$: The probability of being called back when applicants have Caucasian sounding names with no volunteer experience is 5.93 percentage less when applying to a EOE.

$volunteer \times ethnicity$: Having African-American sounding name changes the probability of being called by 0.67% percentage point more when the individual had volunteer experience.

$equal \times volunteer$: Having volunteer experience changes the probability of being called by 4.31% percentage point more when the individual had applied to a EOE.

$ethnicity:equal$: Having African-American sounding name changes the probability of being called by 5.21% percentage point more when applying to a EOE. 

There are a few conditions:

1. African American sounding names with Volunteer experience applying to EOE VS Caucasian sounding names with Volunteer experience applying to EOE.

2. African American sounding names with no Volunteer experience applying to EOE VS Caucasian sounding names with no Volunteer experience applying to EOE

3.African American sounding names with  Volunteer experience applying to non-EOE VS Caucasian sounding names with Volunteer experience applying to non-EOE

4. African American sounding names with no Volunteer experience applying to non-EOE VS Caucasian sounding names with no Volunteer experience applying to non-EOE

Using the robust F test:

Testing Condition 1:

![F test](..\..\images\second_images\test_19.png)

Since the p-value of the test if greater than 5%, we cannot reject the Null hypothesis. Therefore, we can conclude that there is no  significant difference in discrimination for African American sounding names with Volunteer experience applying to EOE VS Caucasian sounding names with Volunteer experience applying to EOE.

Testing Condition 2:

![F test](..\..\images\second_images\test_20.png)

Since the p-value of the test is greater than 0.05, we cannot reject the Null hypothesis. Therefore, we can conclude that there is no significant difference African American sounding names with Volunteer experience applying to EOE VS African American sounding names with no Volunteer experience applying to EOE.

Testing Condition 3:

![F test](..\..\images\second_images\test_21.png)

Since the p-value of the test is greater than 5%, we  cannot reject the Null hypothesis. Therefore, we can conclude that there is no significant difference in discrimination for African American sounding names with Volunteer experience applying to non-EOE VS Caucasian sounding names with Volunteer experience applying to non-EOE.

Testing Condition 4:

![F test](..\..\images\second_images\test_22.png)

Since the p-value of the test if greater than 5%, we cannot reject the Null hypothesis. Therefore, we can conclude that there is no significant difference in discrimination for African American sounding names with no Volunteer experience applying to non-EOE VS Caucasian sounding names with no Volunteer experience applying to non-EOE.

Therefore, looking at all the cases, the answer to my question is: \textbf{There is no significant difference in discrimination for people who mention some volunteering experience VS those that didnot for people applying to EOE}

Computing robust confidence intervals:

![F test](..\..\images\second_images\test_23.png)

The Average Partial effect of experience on call is 0.005276949. The confidence interval for this is [-0.0001751514, 0.0107290487]

Interpretation:

The probability of being called for an an African American sounding name with an extra year of experience and has provided email on their resume is 0.53% higher, when their initial number of years of experience is equal to the sample average.


The Average Partial effect of email on call is 0.03247654 and confidence interval for this is [-0.008861219, 0.073814299]


The Average Partial effect of ethnicity on call is -0.01966382 and confidence interval for this is [-0.06010273, 0.02077509]

![plot 1](..\..\images\second_images\plot_1.png)

![plot 2](..\..\images\second_images\plot_2.png)

If the individual with African American sounding name provided an email, the probability of being called back is slightly less than an individual with Caucasian sounding name who provided an email. However, as level of experience increases, the probability of being called back for an individual with African American sounding name provided an email increases.

If the individual with African American sounding name didnot provide an email, the probability of being called back is almost indistinguisable from an individual with Caucasian sounding name who didnot an email. 

Experience has a greater effect between African-American and Caucasian sounding names if they both provided an email. 