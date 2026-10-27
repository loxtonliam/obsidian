---

## module: Data Science 141 topic: 11 title: Evaluating Models tags: [ds141, model-evaluation, classification, regression, clustering, r]

# DS141 Topic 11: Evaluating Models

> [!abstract] Big idea Don't ask whether a model is "correct"; ask whether it's **useful**. This topic covers how to compare models (e.g. decision tree vs k-NN, or 3-NN vs 5-NN) objectively. In **CRISP-DM**, this is the **Evaluation** step (previous topics were mostly **Modelling**).

---

## 1. Evaluating Classifiers

### 1.1 Why accuracy alone is not enough

- **Misclassification error** (from tree output) = proportion of incorrect predictions on the data.
- Two warnings:
    1. **Evaluating on training data → overconfidence.** Good training performance ≠ good generalisation. Use separate train/test data (see [[DS141 Topic 12]]).
    2. **Class imbalance makes accuracy misleading.** If only 5% of loan clients default, a "model" that always predicts _repay_ gets **95% accuracy** but is useless.

### 1.2 Confusion matrix

A cross-tabulation of **predicted** vs **actual** values. Correct predictions are on the **diagonal**; errors are off-diagonal.

||**Actual Positive**|**Actual Negative**|
|---|---|---|
|**Predicted Positive**|True Positive (TP)|False Positive (FP)|
|**Predicted Negative**|False Negative (FN)|True Negative (TN)|

> [!important] "Positive" ≠ good The **positive class = the class of interest**, often the _bad_ outcome (loan default, having a disease, fraud). Sometimes the choice is arbitrary (bird vs plane).

- **TP:** correctly classified as positive
- **TN:** correctly classified as negative
- **FP:** classified positive, actually negative
- **FN:** classified negative, actually positive

Different errors have different consequences: in the loan example, an FN (predict repay, client defaults) costs the lender money, while an FP (predict default, client would have repaid) loses a good client.

### 1.3 Performance measures

|Measure|Formula|Meaning|
|---|---|---|
|**Accuracy**|$\dfrac{TP+TN}{TP+TN+FP+FN}$|Proportion correct overall|
|**Misclassification error**|$\dfrac{FP+FN}{TP+TN+FP+FN}$|Proportion incorrect ($= 1 - \text{accuracy}$)|
|**Precision** (positive predictive value)|$\dfrac{TP}{TP+FP}$|When it predicts positive, how often is it right?|
|**Recall** (= sensitivity = true positive rate)|$\dfrac{TP}{TP+FN}$|Of all actual positives, how many did it catch?|
|**Specificity** (true negative rate)|$\dfrac{TN}{TN+FP}$|Of all actual negatives, how many did it get right?|
|**False positive rate**|$\dfrac{FP}{FP+TN}$|Specificity $= 1 - \text{FPR}$|
|**F-measure / F1**|$2\cdot\dfrac{\text{precision}\cdot\text{recall}}{\text{precision}+\text{recall}}$|Harmonic mean of precision and recall|

> [!tip] Precision vs recall: which matters more?
> 
> - **High precision** → minimise **FPs**. Example: **spam filter** (don't send important email to spam).
> - **High recall** → minimise **FNs**. Example: **medical diagnosis** (don't tell a sick patient they're healthy).
> - There's usually a **trade-off** between the two, hence the F-measure.

> [!note] Terminology by field
> 
> - ML: **precision & recall**
> - Medicine/biology: **sensitivity & specificity**
> - ⚠️ Older textbook editions (p. 204) have sensitivity and specificity **swapped**. Trust the notes.

### 1.4 Worked example

1000 observations, 50 positive:

||Actual +|Actual −|
|---|---|---|
|**Pred +**|20|50|
|**Pred −**|30|900|

- Accuracy $= \dfrac{20+900}{1000} = 0.92$
- Precision $= \dfrac{20}{20+50} = 0.2857$
- Recall $= \dfrac{20}{20+30} = 0.4$
- F-measure $= 2\cdot\dfrac{(0.2857)(0.4)}{0.2857+0.4} = \tfrac{1}{3} \approx 0.333$

Looks fine at 92%, **but** a majority classifier would get **95%** (only 5% positive).

### 1.5 Class imbalance

- Common in fraud detection, direct marketing response, caravan insurance (Practical 9).
- Problems: misleading accuracy, and too few minority examples to learn from (possible bias).
- Simple fixes:
    - **Undersample** the majority class (discard majority observations; bad if data is small).
    - **Oversample** the minority class (e.g. duplicate minority observations).
- More advanced methods come in later modules.
- Confusion matrices also extend to **multi-class** problems.

---

## 2. Evaluating Regression Models

### 2.1 Setup

Model: $y = f(x) + \varepsilon$, approximated as $y = \beta_0 + \beta_1 x + \varepsilon$.

- $\varepsilon$ captures non-linearity, missing variables, and noise, so perfect prediction is impossible even with the true $\beta$s.
- With no model, the best guess for $y$ is $\bar{y}$.
- Eyeballing lines on a scatterplot only works for 1 predictor, so objective measures are needed.

### 2.2 Residual Standard Error (RSE)

Estimate of the standard deviation of $\varepsilon$; roughly the **average deviation of the response from the true regression line**.

$$RSE = \sqrt{\frac{1}{n-p-1},RSS}$$

- $n$ = number of observations, $p$ = number of predictors
- Simple linear regression: $RSE = \sqrt{\dfrac{RSS}{n-2}}$
- **Small RSE → good fit.** But it's in the **units of $y$**, so what counts as "small" depends on context.

### 2.3 $R^2$ (coefficient of determination)

**Proportion of variability in $y$ explained by the model.** Always between 0 and 1; scale-free.

$$TSS = \sum_{i=1}^{n}(y_i-\bar{y})^2 \qquad R^2 = 1 - \frac{RSS}{TSS}$$

- $TSS$ = total variability in $y$
- $RSS$ = variability left **unexplained**
- $TSS - RSS$ = variability **explained**
- Close to 1 → good fit; close to 0 → model explains little.
- "Good" $R^2$ depends on the field (physical sciences expect near 1; social sciences accept lower).

### 2.4 Worked example (from Topic 8, Ex 8.3)

$n=5$, $p=1$, $RSS = 1.1$, $TSS = 6$:

- $RSE = \sqrt{\dfrac{1.1}{5-1-1}} = 0.6055$
- $R^2 = 1 - \dfrac{1.1}{6} = 0.8167$ → 81.67% of variability in $y$ explained.

### 2.5 $R^2$ and correlation

In **simple** linear regression: $R^2 = r^2$.

- Exercise 11.1: $r = 0.9037$, and $0.9037^2 \approx 0.8167$ ✔
- `cars` data: $r = 0.8068949$, $r^2 = 0.6512$ = Multiple R-squared in output ✔

### 2.6 Reading R output (`summary(lm(...))`)

```
Residual standard error: 15.38 on 48 degrees of freedom   ← RSE
Multiple R-squared: 0.6511, Adjusted R-squared: 0.6438     ← R² and adjusted R²
```

For multiple regression you only need to **find and interpret** RSE and $R^2$, not compute them by hand.

### 2.7 Adjusted $R^2$

> [!warning] $R^2$ always increases when you add a variable, even pure noise. **Adjusted $R^2$ penalises unnecessary variables.** Use it to compare models with different numbers of predictors, and still think about whether each variable makes sense.

`mtcars` example (predicting `mpg`):

|Model|Predictors|RSE|$R^2$|Adj. $R^2$|
|---|---|---|---|---|
|Q4.4|`wt`|3.046|0.7528|0.7446|
|Q4.5|`wt`, `cyl`|**2.568**|0.8302|**0.8185**|
|Q4.6|all 10|2.65|**0.869**|0.8066|

→ The full model has the highest $R^2$, but the **`wt + cyl` model is better** by adjusted $R^2$ and RSE. (Better of these, not necessarily _the best_ model.)

---

## 3. Evaluating Clustering Models

- **Unsupervised** → no target variable, so nothing to compare predictions against.
- Different choices (k-means vs hierarchical, distance measure, linkage, number of clusters) give different results, and there's no single "correct" answer.
- In practice: try several setups, prefer the **most useful / interpretable** solution, and look for patterns that **consistently emerge**.
- Metrics like the **silhouette score** exist but are out of scope.

---

## 4. Practical 11: Confusion Matrices in R

### Setup

```r
library(tidyverse); library(rpart); library(caret)

# Binary problem: drop setosa, keep only width attributes
myiris <- iris %>% filter(Species != "setosa") %>%
  select(Sepal.Width, Petal.Width, Species)
myiris$Species <- factor(myiris$Species)   # re-factor to drop the empty "setosa" level

ggplot(myiris, aes(Sepal.Width, Petal.Width, color = Species)) +
  geom_point(position = "jitter")          # jitter fixes overplotting
```

### Fit models

```r
dectree <- rpart(Species ~ ., data = myiris)
onenn   <- knn3(Species ~ ., data = myiris, k = 1)   # caret; formula syntax like rpart
fivenn  <- knn3(Species ~ ., data = myiris, k = 5)
```

### Predict (exclude the target column)

```r
dectree.pred <- predict(dectree, myiris[,-3], type = "class")
onenn.pred   <- predict(onenn,   myiris[,-3], type = "class")
fivenn.pred  <- predict(fivenn,  myiris[,-3], type = "class")
```

`type = "class"` returns predicted categories instead of probabilities.

### Confusion matrix

```r
confusionMatrix(predicted, actual)   # order: predictions first, then actual
dectree.conf <- confusionMatrix(dectree.pred, myiris$Species)
# change the positive class with: positive = "virginica"
```

- `table(pred, actual)` also works but gives no statistics.
- In the output, **Reference = actual values**.

**Output fields to know:**

- **Accuracy**
- **No Information Rate** = accuracy of a majority classifier (proportion of the largest class)
- **Sensitivity**, **Specificity**
- **Pos Pred Value** (= precision), **Neg Pred Value** $= \dfrac{TN}{TN+FN}$
- **'Positive' Class** (defaults to the first factor level, here _versicolor_)

### Results (on training data)

|Model|TP|FN|FP|TN|Accuracy|Sens.|Spec.|PPV|NPV|
|---|---|---|---|---|---|---|---|---|---|
|Decision tree|49|1|5|45|0.94|0.98|0.90|0.9074|0.9783|
|1-NN|47|3|1|49|**0.96**|0.94|0.98|0.9792|0.9423|
|5-NN|48|2|4|46|0.94|0.96|0.92|0.9231|0.9583|

_(Positive = versicolor; No Information Rate = 0.5 for all.)_

### Takeaways

- **1-NN looks best, but that's expected on training data**: each point's nearest neighbour is (usually) itself, so it essentially memorises the training set. Not evidence it generalises.
- Tree and 5-NN tie on accuracy but differ in sensitivity/specificity. For irises the two error types cost about the same, but in many problems that matters.
- Proper evaluation needs **unseen test data** → [[DS141 Topic 12]].

> [!todo] Must be able to Calculate **accuracy, sensitivity, specificity, PPV, NPV** by hand from a confusion matrix, and interpret `confusionMatrix()` output.

---

## 5. Tutorial 11 (churn data)

Data: `churn.csv`, 20 000 customers, target `LEAVE`, 11 attributes (same as Practical 5).

**Practical tasks:**

1. Load `tree` and `caret`.
2. Import as `churn`, check `str()`, fix data types.
3. Fit a decision tree with `tree()` (defaults) and plot it.
4. Confusion matrix for the tree (predict first).
5. Fit **1-NN**, **5-NN**, **21-NN** with `knn3()`; confusion matrix for each.

**Discussion questions:**

- Why is the 1-NN accuracy what it is? _(training-data evaluation / memorisation)_
- How is the **No Information Rate** calculated and what does it represent?
- Ignoring 1-NN, which model is best? Justify using the evaluation measures.

---

## 📚 Textbook reading (Ch. 7)

- Intro: pp. 187–188 (link results back to the original business goal)
- Classifier evaluation: pp. 165–167, 203–204; positive/negative terminology: pp. 188–189
- Imbalanced data: pp. 190–193
- Cost-sensitive evaluation (read for interest only): pp. 194–203
- Majority classifiers: pp. 204–207

Related: [[DS141 Topic 5]] (decision trees) · [[DS141 Topic 8]] (regression) · [[DS141 Topic 9]] (k-NN) · [[DS141 Topic 10]] (clustering) · [[DS141 Topic 12]]