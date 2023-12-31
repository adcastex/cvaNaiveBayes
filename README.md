# naive_bayes_r

This R package was developed by Cyrielle, Victor and Adrien. It can be used to create a Naive Bayes model of the categorical type. This package was developed under R for use on R. It has been developed as an R6 class.

[Click here](https://c4sf5g-victor-sigogneau.shinyapps.io/shiny_test/) to find the shiny application.

[Click here](https://github.com/victorsigogneau/shiny-app-NBC/) to find the git for the shiny application.

## Library import 

The first step in using the package is to install it.
There are two ways to install it:
- The first is to go through the .tar.gz file. To do this, you first need to clone the git folder. Then, using Rstudio's Install feature, you can select the .tar.gz file. If you do, Studio will install the library.
- The other option is to download the package directly from GitHub. To do this you would need to install the devtools library and run the following command: 
```R 
remotes::install_github("adcastex/cvaNaiveBayes",dependencies = TRUE)
```


## Library functions

### NaiveBayes$new()

To train the model, first call the class constructor using the function : modNB::naive_bayes_r$new()
The constructor initializes the class then you can call up its functions.

### NaiveBaye$fit(X, y, preproc = TRUE, nb_classe = 6, epsilon = NULL, g_na = TRUE)

This function is used to pre-process the data and train the naive bayes categorical model.
the function parameters are as follows :
- X : The dataframe of variables used for prediction
- y : The variable to be predicted
- preproc : A boolean which, if set to TRUE, launches the Preprocecing function, which discretizes numeric variables
- nb_classe : Number of classes after discretization (default 6)
- epsilon : 
- g_na : If TRUE, launches a preprocecing function which replaces NA with another value

### NaiveBaye$predict(new_data)

This function launches predictions from the model trained in the fit function on the new_data dataframe, which has the same number of variables as X. The function returns a vector containing the predictions made.
In this function, new_data must have the same number of columns as variable X in the fit function.

### NaiveBaye$predict_proba(new_data)

This function launches predictions from the model trained in the fit function on the new_data dataframe, which has the same number of variables as X. The function returns a dataframe of prediction probabilities.
In this function, new_data must have the same number of columns as variable X in the fit function.

### NaiveBaye$print()

This function takes no parameters. It displays some minimal information about the model (the number of variables, the number of output classes and the size of the training sample).

### NaiveBaye$summary()

This function takes no parameters. This function displays model details.
This function will display :
- A summary of variables (number of predictor variables, number of classes to predict)
- A summary of preprocessing
- A summary of variables (number of observations, min and max of each variable, number of classes of each variable)
- Prior probabilities


### NaiveBaye$compute_and_plot_importance()

This function displays a graph showing the importance of variables 

## Example of use 

#### First import devtools package
```R
# Check if devtools is not already installed
if (!requireNamespace("devtools", quietly = TRUE)) {
  # Install devtools
  install.packages("devtools")
}

library(devtools)
```
#### Install the package
```R 
remotes::install_github("adcastex/cvaNaiveBayes",dependencies = TRUE)
```
#### Import the package and test help 
```R
# Install and load the cvaNaiveBayes package from GitHub
library(cvaNaiveBayes)

# Display documentation for the NaiveBayes class
?NaiveBayes
```

#### Now create your datasets 
[Click here](https://github.com/victorsigogneau/shiny-app-NBC/tree/main/Dataset) to find the git for example files

```R
# Read the training data and select specific columns
healthcare_data <- read.csv("healthcare_TRAIN.csv",sep=";")

selected_columns <- c("Medical.Condition", "Admission.Type","Test.Results")

healthcare_data<-healthcare_data[,selected_columns]

X_train <- healthcare_data[, -3]

y_train <- healthcare_data[, 3]


# Separate features (X_train) and target variable (y_train) for training

X_test=read.csv("healthcare_TEST.csv",sep=";")

selected_columns <- c("Medical.Condition", "Admission.Type")

X_test<-X_test[,selected_columns]
```

#### Initialize class
```R
# Create a new instance of the NaiveBayes class
nb_model=cvaNaiveBayes::NaiveBayes$new()
```

#### Train model
```R
# Fit the Naive Bayes model using training data
nb_model$fit(X_train,y_train)
```

#### Print model information 
```R
# Compute and plot the importance of variables
nb_model$compute_and_plot_importance()

# Print the details of the Naive Bayes model
nb_model$Print()

# Display a summary of the Naive Bayes model
nb_model$Summary()
```

#### Make prediction 

```R
# Predict classes for the test data
pred=nb_model$predict(X_test)

# Combine test data with predictions
predictions=cbind(X_test, Prediction = pred)

# Get probabilities of class membership for the test data
probas=nb_model$predict_proba(X_test)
```
