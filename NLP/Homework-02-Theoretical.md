# Homework 02 - Theoretical

## Evaluating: Cross-Validation

Cross-validation allows us to compare different machine learning methods and see how well they will work in practice.

> Cross-Validation in machine learning is a technique used to train and evaluate our model on a portion of our database before re-portioning our dataset and evaluating it on the new portions.
> 

### Steps of evaluation in general

1. Training the machine learning methods
2. Testing the machine learning methods

<aside>
⚠️ A terrible approach would be to use all of the data to estimate the parameters. (i.e., training the algorithm) Because then there is no data to test the algorithm.
Also, using the same training data for testing the algorithm is a bad idea.

</aside>

### Steps of Cross-Validation

1. Split the data into train and test sets and evaluate the model’s performance. (do evaluation)
2. Split the data and split it into new train and test sets. Re-evaluate the model’s performance. (repeat evaluation for other Permutations)
3. To get the actual performance metric, take the average of all measures.

### **Four-Fold Cross-Validation**

You can use %75 of the data for training and the other %25 for testing the algorithm. But which one of the %25s should be used for testing the algorithm? Cross-Validation uses all these data blocks, one at a time, and summarizes the result at the end.

<aside>
💡 We divided our data into four blocks. This method is called **Four-Fold Cross-Validation**.

</aside>

There are also some other methods for these blocks.

<aside>
💡 It is widespread to divide the data into ten blocks in practice. **Ten-Fold Cross-Validation**.

</aside>

### Tuning Parameter

It is a parameter that hasn’t been estimated but is just a guess. We can use **Ten-Fold Cross-Validation** to help find the best value for that tuning parameter.

### Types of Cross-Validation

Let’s see all kinds of Cross-Validation.

1. **Hold Out Validation Approach*:** W*e usually do a train and test split. This means suppose you have a dataset; we try to do a train-test split. (the technique which we generally use). 
    - Simple, easy to understand, and implement.
    - Not suitable for an imbalanced dataset.
    - A lot of data is isolated from training the model.
2. **K-Fold Cross-Validation:** We may get different-different accuracy; this may lead to either over-fitting or under-fitting of the model. To overcome this, we use this method. In K-fold cross-validation, we can make k splits. It split the dataset into `k` consecutive folds (without shuffling by default). Each fold is then used once as validation while the `k-1` remaining folds form the training set.
    - Low time complexity
    - The model has a low bias.
    - The entire dataset is utilized for both training and validation.
    - Not suitable for an imbalanced dataset.
3. **Stratified K-fold Cross-Validation*:***  This method is reasonable when there are minority classes present in our data. When this happens, our accuracy will not correctly reflect how well minority classes are predicted. We have to split the data so that each portion has the same percentage of the different classes in the dataset.
    - Works well for an imbalanced dataset.
    - Now suitable for time series dataset.
4. **Leave-One-Out Cross-Validation:** We can call each sample a block in an extreme case. This process is closely related to the statistical method of jack-knife estimation.
    - Simple, easy to understand, and implement.
    - The model may lead to a low bias.
    - The computation time required is high.
5. **Repeated Random Test-Train: ****It creates a random split of the data like the train/test split described above but repeats the process of splitting and evaluation of the algorithm multiple times, like cross-validation.
    - The proportion of train and validation splits is not dependent on the number of iterations or partitions.
    - This technique may not select some samples for either training or validation.
    - Not suitable for an imbalanced dataset.
6. **Time Series cross-validation:** A technique to train time-series data.
7. **Nested cross-validation:** When cross-validation is used simultaneously for tuning the hyperparameters and generalizing the error estimate, nested cross-validation is required.

## Language models

You read some books in your life so that you can guess a hidden word or phrase in a sentence. But how can a computer learn to guess these words?
We need to calculate the probability of the sentence.

![Untitled](Homework%200%200752b/Untitled.png)

We don’t need to calculate all of the Probabilities before. We use an `n` parameter and take those previous words. 

### Bi-Gram

For Bi-Gram, N is equal to two.

We also need to add fake start and fake ending tokens.

**The model**

![Untitled](Homework%200%200752b/Untitled%201.png)

**To estimate the probabilities**

![Untitled](Homework%200%200752b/Untitled%202.png)

For this question, here are the sentences.

```jsx
<s> B A B A A B B </s>
<s> B A B A B A A </s>
<s> B A B B A B A </s>
```

### Calculation

‌It’s all about counting. Based on the Bayes formula

<aside>
💡 $P(A|B)= \frac{P(B|A) \times P(A)}{P(B)}$

</aside>

**A)** $P(w1=A|w2=B)= \frac{P(w2=B|w1=A) \times P(w1=A)}{P(w2=B)}$

- $P(w2=B|w1=A)= \frac{0}{3}$

So the final result turns out to be `0`.

<aside>
💡 There is no sample that `A` is word number 1. The number `0` makes sense.

</aside>

**A-Smoothing)** $P(w1=A|w2=B)= \frac{P(w2=B|w1=A) \times P(w1=A)}{P(w2=B)}$

## Bayes Rule

<aside>
💡 $P(A|B)= \frac{P(B|A) \times P(A)}{P(B)}$

</aside>

The input digit could be 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9. As the question mentioned, the system can recognize all digits except 7 and 8.

For 7,

- $P(OutputIs8|7) = \frac{1}{2}$
- $P(OutputIs7|7) = \frac{1}{2}$

For 8,

- $P(OutputIs8|8) = \frac{7}{10}$
- $P(OutputIs7|8) = \frac{3}{10}$

---

To calculate the correct recognition of digit 7,

$P(7|OutputIs7) = \frac{P(OutputIs7|7) \times P(7)}{P(OutputIs7)}$ 

- $P(7) = \frac{1}{10}$
- $P(OutputIs7)= P(OutputIs7|7) + P(OutputIs7|8) = \frac{1}{2} + \frac{3}{10} = \frac{8}{10}$
- $P(OutputIs7|7) = \frac{1}{2}$

$P(7|OutputIs7) = \frac{1}{2} \times  \frac{1}{8} = \frac{1}{16}$ *(The possibility of input 7 and correct recognition)*

So if we are sure that the input number is 7 then $\frac{10}{16}$ is the possibility for correct recognition.

---

To calculate the correct recognition of digit 8,

$P(8|OutputIs8) = \frac{P(OutputIs8|8) \times P(8)}{P(OutputIs8)}$ 

- $P(8) = \frac{1}{10}$
- $P(OutputIs8)= P(OutputIs8|7) + P(OutputIs8|8) = \frac{1}{2} + \frac{7}{10} = \frac{12}{10}$
- $P(OutputIs8|8) = \frac{7}{10}$

$P(8|OutputIs8) = \frac{7}{10} \times \frac{1}{12} = \frac{7}{120}$ *(The possibility of input 8 and correct recognition)*

So if we are sure that the input number is 8 then $\frac{7}{12}$ is the possibility for correct recognition.

# References

- **[Machine Learning Fundamentals: Cross-Validation](https://www.youtube.com/watch?v=fSytzGwwBVw)**
- ****[Cross-Validation Techniques](https://medium.com/analytics-vidhya/cross-validation-techniques-bacb582097bc)****
- **[Understanding 8 types of Cross-Validation](https://towardsdatascience.com/understanding-8-types-of-cross-validation-80c935a4976d)**
- **[NLP: Understanding the N-gram language models](https://www.youtube.com/watch?v=GiyMGBuu45w)**