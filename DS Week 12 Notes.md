# DS141 Topic 12: Overfitting

> [!summary] Executive Summary
> This lecture note summarizes DS141 Topic 12 on overfitting. It covers the concepts of overfitting and underfitting, diagnosing overfitting using training and test data partitioning, evaluating model complexity with fitting graphs, using validation data and cross-validation, and strategies for controlling model complexity in decision trees and k-NN models. It also introduces the bias-variance trade-off[cite: 2, 4, 8, 10, 12, 14].

## 1. Core Concepts: Signal vs. Noise

> [!info] The Goal of Modeling
> The primary goal when building models is to find the =="signal" or pattern== in the data[cite: 2].
> If a model captures this signal using past data, it can make predictions on new, unseen cases with minimal human intervention[cite: 2].

> [!important] Overfitting vs. Underfitting
> -   **Overfitting:** Occurs when a model is allowed to follow the training data too closely, capturing ==noise== alongside the signal[cite: 2].
>     -   This typically results in a decline in the model's predictive ability on unseen data[cite: 2].
> -   **Underfitting:** Occurs when a model is ==too simple== to capture the underlying signal in the training data[cite: 2].
>     -   This typically results in poor performance on the training data itself[cite: 2].
> -   **The "Goldilocks" Zone:** The optimal model complexity where it is "just right"—neither too complex (overfitting) nor too simple (underfitting)[cite: 2].

## 2. Diagnosing Overfitting

Evaluating a model solely on the data used to build it (training data) yields an overoptimistic estimate of its performance[cite: 4].

### Partitioning Data

To get a fairer estimate, data is partitioned into training and test components:
-   **Training Data:** Used to construct the model[cite: 2].
-   **Test Data (Holdout Data):** Unseen data used to evaluate the model's performance[cite: 4].
    -   An overfit model usually performs poorly on test data[cite: 4].
    -   Good test data performance increases confidence that the model is not overfitting[cite: 4].

> [!example] Data Splitting
> A common, convenient split is ==80% training / 20% testing==[cite: 5].
> The allocation must be random to ensure both sets are representative[cite: 5].

### Fitting Graphs

Fitting graphs visualize model performance (like accuracy) on both training and test data as a function of model complexity[cite: 7].

| Data Set | Trend as Complexity Increases |
| :--- | :--- |
| **Training Data** | Accuracy continuously ==increases==[cite: 8]. |
| **Test Data** | Accuracy increases up to a point, stabilizes, and then eventually ==decreases==[cite: 8]. |

> [!important] The "Sweet Spot"
> The point on a fitting graph where test accuracy stabilizes before decreasing indicates the point after which overfitting begins[cite: 8].

## 3. Advanced Evaluation Methods

### Validation Data

When comparing multiple models with varying complexities to find the "sweet spot," using the test data repeatedly can lead to biased performance estimates[cite: 9].

-   **Solution:** Split data into three components:
    1.  **Training Data:** To build the models[cite: 9].
    2.  **Validation Data:** A holdout set used to ==choose among the trained models==[cite: 9].
    3.  **Test Data:** A final holdout set used only once to evaluate the chosen final model[cite: 9].
-   *Example split:* 80% training, 10% validation, 10% test[cite: 9].

### Cross-Validation

Used when data is limited or to obtain repeated performance measurements (mean and variance) to assess confidence[cite: 9, 10].

> [!info] k-Fold Cross-Validation
> 1. The data is split into $k$ (e.g., 5 or 10) equal subsets (folds)[cite: 10].
> 2. The modeling process is repeated $k$ times[cite: 10].
> 3. In each iteration, one fold acts as the holdout (test) data, and the remaining folds are used for training[cite: 10].
> 4. Performance is averaged across all folds[cite: 10].

## 4. Controlling Model Complexity

### Decision Trees

If grown indefinitely, trees will have subsets of a single observation, achieving perfect training accuracy but poor generalization (overfitting)[cite: 12].

Two main strategies to prevent tree overfitting:
1.  **Stop Growing:** Halt the process before the tree becomes too complex (e.g., set a minimum number of observations per node)[cite: 12].
2.  **Pruning:** Grow a large tree and then "chop off" lower branches if their removal doesn't adversely affect performance[cite: 12].

### Regularization

Adds a ==penalty term== to the model's objective function[cite: 13].
-   The penalty is higher for more complex models, giving simpler models an advantage[cite: 13].

### Ensembles

Combine multiple weaker models to create a stronger overall model (e.g., bagging, boosting)[cite: 13].

## 5. The Bias-Variance Trade-off

This concept captures the tension between model complexity and predictive accuracy[cite: 13].

-   **Bias:** Error from incorrect assumptions built into the model (e.g., assuming a linear relationship when it's a curve)[cite: 14].
    -   *Analogy:* Systematic aiming error (aim is off)[cite: 14].
-   **Variance:** Error from sensitivity to fluctuations in the training set[cite: 14].
    -   *Analogy:* Scattered shots (wide dispersion)[cite: 14].

| Model Type | Bias | Variance | Consequence |
| :--- | :--- | :--- | :--- |
| **Very Simple Model** | High | Low | May underfit the data[cite: 15]. |
| **Very Complex Model** | Low | High | Sensitive to small training data fluctuations; may overfit[cite: 15]. |

> [!important] Key Takeaway
> The goal is to manage the trade-off to achieve low bias and low variance, often using techniques like cross-validation[cite: 15].

---

## Tasks / Study Plan
- [ ] Review Topic 4 and 9 notes on introductory overfitting concepts.
- [ ] Complete Practical Assignment 12 in RStudio (Diagnosing overfitting with trees and k-NN)[cite: 18].
- [ ] Complete Tutorial 12 on fitting classifiers and cross-validation[cite: 30, 31].
- [ ] Read textbook Chapter 5 (pages 111-118, 126-129, 133-134, 139)[cite: 17].
- [ ] Read textbook Chapter 12 (pages 308-311)[cite: 17].