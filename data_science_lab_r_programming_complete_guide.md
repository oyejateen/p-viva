# 7CIT4-21: Fundamentals of Data Science Lab — Complete Preparation Guide

A clean, structured reference containing simple theory, real-world examples, executable R code, and key viva questions for all 8 practical experiments.

## Practical 1: Install R & RStudio, Explore Interface, and Basic Commands

### 1. Theory & Real-World Example

* **R:** The core open-source statistical programming language engine that handles calculations, data manipulation, and modeling behind the scenes.

* **RStudio:** An Integrated Development Environment (IDE) built on top of R that provides a clean workspace divided into **4 panes**:

  1. **Source (Top-Left):** Write, edit, and save `.R` script files.

  2. **Console (Bottom-Left):** Execute commands and view immediate text output.

  3. **Environment / History (Top-Right):** View active variables, loaded datasets, and command history.

  4. **Files / Plots / Packages / Help (Bottom-Right):** View generated visualizations, manage libraries, and read documentation.

* **Core Data Structures:**

  * **Vector:** A 1D sequence of elements of the *same* data type (created using `c()`). *Example:* A list of daily temperatures `[30, 32, 29, 31]`.

  * **Data Frame:** A 2D table (like an Excel sheet or SQL table) where different columns can hold different data types (numbers, text, booleans).

### 2. R Code

```
# 1. Basic arithmetic and variable assignment
x <- 10
y <- 20
sum_result <- x + y
print(paste("The sum of x and y is:", sum_result))

# 2. Creating and working with a Vector (c = combine)
numbers_vector <- c(5, 12, 18, 25, 30)
mean_val <- mean(numbers_vector)
print(paste("Mean of the vector:", mean_val))

# 3. Creating and exploring a Data Frame
student_data <- data.frame(
  ID = c(1, 2, 3),
  Name = c("Aarav", "Priya", "Rohan"),
  Score = c(85, 92, 78)
)

# Inspect structure and statistical summary
str(student_data)
summary(student_data)

# Access a specific column
print(student_data$Name)

```

### 3. Key Viva Questions

* **Q: What is the difference between R and RStudio?**

  * **A:** R is the statistical programming language and computation engine, while RStudio is the graphical IDE interface that makes writing code, viewing plots, and managing workspaces easier.

* **Q: What does the `str()` function do?**

  * **A:** It compactly displays the internal structure of an R object, including column data types, dimensions, and sample values.

## Practical 2: Descriptive Statistics & Data Visualizations

### 1. Theory & Real-World Example

Suppose a class of 5 students scores `[50, 60, 70, 80, 100]` on a test:

* **Mean (Average):** Sum of all values divided by the count ($\frac{360}{5} = 72$). Sensitive to extreme values.

* **Median:** The exact middle value when sorted (`70`). Resistant to extreme outliers.

* **Variance & Standard Deviation (SD):** Measure how spread out the data is around the mean. SD is the square root of variance, bringing the measurement back to the original units.

* **Visualizations (using `ggplot2`, based on the *Grammar of Graphics*):**

  * **Histogram:** Groups continuous data into bins to show frequency distribution.

  * **Boxplot:** Displays the five-number summary (Min, Q1, Median, Q3, Max) and highlights **outliers**.

  * **Scatter Plot:** Plots two continuous variables against each other to reveal relationships/correlations.

### 2. R Code

```
# Load ggplot2 library and inspect built-in mtcars dataset
library(ggplot2)
head(mtcars)

# 1. Descriptive Statistics for Miles Per Gallon (mpg)
mean_mpg   <- mean(mtcars$mpg)
median_mpg <- median(mtcars$mpg)
var_mpg    <- var(mtcars$mpg)
sd_mpg     <- sd(mtcars$mpg)

cat("Mean MPG:", mean_mpg, "\nMedian MPG:", median_mpg, "\nVariance:", var_mpg, "\nSD:", sd_mpg, "\n")

# 2. Histogram (Frequency distribution)
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram(binwidth = 3, fill = "skyblue", color = "black") +
  labs(title = "Histogram of Car Mileage", x = "Miles Per Gallon", y = "Frequency")

# 3. Boxplot (Distribution and outliers by cylinder count)
ggplot(mtcars, aes(x = as.factor(cyl), y = mpg)) +
  geom_boxplot(fill = "orange") +
  labs(title = "MPG Distribution by Cylinders", x = "Cylinders", y = "MPG")

# 4. Scatter Plot (Car Weight vs Mileage)
ggplot(mtcars, aes(x = wt, y = mpg)) +
  geom_point(color = "darkred", size = 3) +
  labs(title = "Car Weight vs Mileage", x = "Weight (1000 lbs)", y = "MPG")

```

### 3. Key Viva Questions

* **Q: When is Median preferred over Mean?**

  * **A:** When a dataset contains extreme outliers or is heavily skewed (e.g., household income data), because outliers pull the mean disproportionately.

* **Q: Why do we use Standard Deviation instead of Variance for interpretation?**

  * **A:** Variance is expressed in squared units; taking the square root yields Standard Deviation, which shares the exact same units as the original data.

## Practical 3: K-Means Clustering Algorithm

### 1. Theory & Real-World Example

Imagine sorting a basket of unlabeled fruits into $K = 3$ piles based purely on weight and diameter.

* **Unsupervised Learning:** There are no predefined target labels; the algorithm discovers natural groupings on its own.

* **How K-Means Works:**

  1. Randomly place $K$ center points (**centroids**).

  2. Assign every data point to its nearest centroid using Euclidean distance.

  3. Recalculate each centroid's position as the mathematical average of all points assigned to it.

  4. Repeat steps 2 and 3 until centroids stop moving (**convergence**), minimizing Within-Cluster Sum of Squares (WCSS).

### 2. R Code

```
set.seed(42)
data(iris)

# Exclude the target Species column and scale numerical features
iris_features <- scale(iris[, 1:4])

# Run K-Means with K = 3 and 25 random initial configurations
k_result <- kmeans(iris_features, centers = 3, nstart = 25)

# Display cluster centers and cluster sizes
print(k_result$centers)
print(k_result$size)

# Plot clusters and mark centroids with stars
plot(iris$Sepal.Length, iris$Sepal.Width, 
     col = k_result$cluster, 
     pch = 19, 
     main = "K-Means Clustering on Iris Dataset",
     xlab = "Sepal Length", ylab = "Sepal Width")
points(k_result$centers[, 1], k_result$centers[, 2], col = 1:3, pch = 8, cex = 3, lwd = 3)

```

### 3. Key Viva Questions

* **Q: How do you find the optimal number of clusters (**$K$**)?**

  * **A:** Using the **Elbow Method**, where WCSS is plotted against various $K$ values, and the point where the rate of decrease sharply bends ("the elbow") is chosen.

* **Q: Why must features be scaled before running K-Means?**

  * **A:** Because K-Means relies on Euclidean distance; features with larger numeric ranges would otherwise dominate the distance calculations.

## Practical 4: Association Rule Mining (`arules` Package)

### 1. Theory & Real-World Example

Used in **Market Basket Analysis** (e.g., *"Customers who buy bread and butter also buy milk"*).

* **Key Evaluation Metrics:**

  1. **Support:** Proportion of all transactions containing both $X$ and $Y$.
     

     $$
     \text{Support}(X \rightarrow Y) = P(X \cap Y)
     $$

  2. **Confidence:** Conditional probability that $Y$ is purchased given $X$ was purchased.
     

     $$
     \text{Confidence}(X \rightarrow Y) = P(Y\vert{}X) = \frac{\text{Support}(X \cap Y)}{\text{Support}(X)}
     $$

  3. **Lift:** Ratio of observed support to expected support if $X$ and $Y$ were independent.

     * $\text{Lift} = 1$: No association (independent).

     * $\text{Lift} > 1$: Strong positive association.

     * $\text{Lift} < 1$: Negative association.

* **Apriori Algorithm:** Uses the *Apriori Property* (all subsets of a frequent itemset must also be frequent) to prune infrequent combinations early and save computation.

### 2. R Code

```
library(arules)

# Load built-in Groceries transaction dataset
data("Groceries")
summary(Groceries)

# Mine association rules using Apriori (min support = 0.1%, min confidence = 80%)
rules <- apriori(Groceries, parameter = list(supp = 0.001, conf = 0.8))

# Inspect top 5 rules sorted by confidence
inspect(head(rules, n = 5, by = "confidence"))

```

### 3. Key Viva Questions

* **Q: What does a Lift value greater than 1 signify?**

  * **A:** It indicates a positive correlation—buying the antecedent item increases the likelihood of buying the consequent item beyond random chance.

* **Q: What is the core principle of the Apriori algorithm?**

  * **A:** Any subset of a frequent itemset must also be frequent; conversely, any superset of an infrequent itemset is automatically pruned.

## Practical 5: Simple Linear Regression (`lm` Function)

### 1. Theory & Real-World Example

Modeling how a car's **stopping distance (**$Y$**)** increases as its **speed (**$X$**)** increases.

* **Equation:**
  

  $$
  Y = \beta_0 + \beta_1 X + \epsilon
  $$

  * $\beta_0$: Intercept (baseline value of $Y$ when $X = 0$).

  * $\beta_1$: Slope (change in $Y$ for every 1-unit increase in $X$).

  * $\epsilon$: Residual error term.

* **Ordinary Least Squares (OLS):** Finds the best-fitting straight line by minimizing the sum of squared residuals (vertical distances between actual data points and the regression line).

### 2. R Code

```
head(cars)

# Fit linear regression model: dist (dependent) ~ speed (independent)
lin_model <- lm(dist ~ speed, data = cars)

# Display coefficients, p-values, and R-squared
summary(lin_model)

# Scatter plot with fitted regression line
plot(cars$speed, cars$dist, 
     main = "Speed vs Stopping Distance",
     xlab = "Speed (mph)", ylab = "Stopping Distance (ft)",
     pch = 16, col = "darkblue")
abline(lin_model, col = "red", lwd = 2)

```

### 3. Key Viva Questions

* **Q: What does the** $R^2$ **(R-squared) metric measure?**

  * **A:** The proportion of variance in the dependent variable ($Y$) that is explained by the independent variable ($X$), measuring goodness of fit.

* **Q: What does a p-value** $< 0.05$ **for the slope indicate?**

  * **A:** There is a statistically significant linear relationship between the predictor and the target variable.

## Practical 6: Logistic Regression (`glm` Function)

### 1. Theory & Real-World Example

Predicting whether a student will **Pass (1)** or **Fail (0)** based on **study hours (**$X$**)**.

* **Why not Linear Regression?** A straight line can output values $< 0$ or $> 1$, which violates probability boundaries.

* **Logit / Sigmoid Curve:** Logistic regression is a Generalized Linear Model (GLM) that maps predictions onto an S-shaped curve bounded strictly between **0 and 1**:
  

  $$
  \log\left(\frac{p}{1 - p}\right) = \beta_0 + \beta_1 X
  $$

* If the predicted probability $p \ge 0.5$, the observation is classified as `1`; otherwise `0`.

### 2. R Code

```
head(mtcars)

# Predict transmission type (am: 0 = automatic, 1 = manual) from hp and wt
log_model <- glm(am ~ hp + wt, data = mtcars, family = binomial)

# View model coefficients and significance
summary(log_model)

# Predict probabilities between 0 and 1
predicted_probs <- predict(log_model, type = "response")
print(head(predicted_probs))

```

### 3. Key Viva Questions

* **Q: What does `family = binomial` specify in `glm()`?**

  * **A:** It configures the model to use the binomial error distribution and the logit link function required for binary classification.

* **Q: What is the role of the Sigmoid function?**

  * **A:** It transforms any real-valued linear combination into a valid probability value strictly bounded between 0 and 1.

## Practical 7: Naive Bayes Classifier & Cross-Validation

### 1. Theory & Real-World Example

Classifying emails as **Spam** or **Not Spam** based on word occurrences.

* **Bayes' Theorem:** Calculates the probability of a class $Y$ given observed features $X$:
  

  $$
  P(Y\vert{}X) = \frac{P(X\vert{}Y)P(Y)}{P(X)}
  $$

* **Why "Naive"?** It assumes all predictor features are **conditionally independent** given the class label—an assumption rarely true in real life, yet computationally fast and highly effective.

* **Train/Test Split & Cross-Validation:** Splitting data into training and testing subsets ensures the model is evaluated on unseen data, preventing **overfitting**.

### 2. R Code

```
library(e1071)
library(caret)
data(iris)

# Split data: 80% training, 20% testing
set.seed(42)
trainIndex <- createDataPartition(iris$Species, p = 0.8, list = FALSE)
train_data <- iris[trainIndex, ]
test_data  <- iris[-trainIndex, ]

# Train Naive Bayes model
nb_model <- naiveBayes(Species ~ ., data = train_data)

# Predict on test set
predictions <- predict(nb_model, test_data)

# Evaluate accuracy via Confusion Matrix
confusionMatrix(predictions, test_data$Species)

```

### 3. Key Viva Questions

* **Q: Why is the algorithm called "Naive" Bayes?**

  * **A:** Because it makes the naive assumption that all input features are completely independent of one another given the class label.

* **Q: What is a Confusion Matrix?**

  * **A:** A performance evaluation table comparing actual labels against predicted labels, showing True Positives, False Positives, True Negatives, and False Negatives.

## Practical 8: Decision Trees & Tree Visualization (`rpart`)

### 1. Theory & Real-World Example

Making a decision through a flowchart of yes/no questions (e.g., *Is it raining?* $\rightarrow$ *Yes: Stay indoors; No: Check temperature...*).

* **Tree Components:**

  * **Root Node:** Topmost feature split representing the entire dataset.

  * **Internal Nodes:** Intermediate decision branches splitting data on feature thresholds.

  * **Leaf Nodes:** Terminal nodes providing the final class prediction.

* **Splitting Criteria:** The tree selects splits that maximize **Information Gain** by minimizing node impurity using **Gini Impurity** or **Entropy**:

  * $\text{Gini} = 0$: Completely pure node (all items belong to one class).

  * $\text{Gini} = 0.5$: Maximum impurity for binary classes (50/50 split).

### 2. R Code

```
library(rpart)
library(rpart.plot)
data(iris)

# Construct classification decision tree
tree_model <- rpart(Species ~ ., data = iris, method = "class")

# Visualize tree splits cleanly
rpart.plot(tree_model, main = "Decision Tree for Iris Dataset", extra = 104)

# Print text-based decision rules
print(tree_model)

```

### 3. Key Viva Questions

* **Q: What does a Gini Impurity of 0 mean?**

  * **A:** It means the node is 100% pure—all data points inside that node belong to a single target class.

* **Q: How do you prevent overfitting in Decision Trees?**

  * **A:** Through **pruning** (cutting back branches that add minimal predictive power) or setting constraints like maximum tree depth and minimum samples required per split.