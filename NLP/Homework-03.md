# NLP - Homework 03

## PoS Tagging using HMM

Tags are

- Verb
- Noun
- Modal

The sentences are

- Mark can watch.
- Will can mark watch.
- Can Tom watch?
- Tom will mark watch.

### Emission Probability

| Words | Noun | Modal | Verb |
| --- | --- | --- | --- |
| Tom | 2/6 | 0 | 0 |
| Mark | 1/6 | 0 | 2/4 |
| Watch | 2/6 | 0 | 2/4 |
| Can | 0 | 3/4 | 0 |
| Will | 1/6 | 1/4 | 0 |

### Transition Probability

- <S> Mark can watch <E>
- <S> Will can mark watch <E>
- <S> Can Tom watch <E>
- <S> Tom will mark watch <E>

|  | Noun | Modal | Verb | <E> |
| --- | --- | --- | --- | --- |
| <S> | 3/4 | 1/4 | 0 | 0 |
| Noun | 0 | 3/6 | 1/6 | 2/6 |
| Modal | 1/4 | 0 | 3/4 | 0 |
| Verb | 2/4 | 0 | 0 | 2/4 |

### Graph

- I removed the vertices with zero probability.
- I removed the vertices that have no way to the `<E>` character.

This is the result

![Untitled](NLP%20-%20Homework%2003%20837479fc5b03420c818782719ba34fb5/Untitled.png)

$P(Sentence)= \frac{1}{4} \times \frac{3}{4} \times \frac{3}{4} \times \frac{2}{6} \times \frac{1}{6} \times \frac{2}{4} \times \frac{2}{4} \times \frac{2}{6} \times \frac{2}{6}=0.0006510416$

## CYK Algorithm

We should pick the sub-tree that has the maximum value and attach that to the main tree from bottom to top until we reach the root. 

| John | eats | pie | with | cream |  |
| --- | --- | --- | --- | --- | --- |
| NP(0.04), Noun(0.2) | S(0.00384) | S(0.000058) |  | S(0.000173) | John |
|  | VP(0.12), Verb(0.3) | VP(0.0018) |  | VP(0.000864), VP(0.0054), VP(0.000013), VP(0.000013) | eats |
|  |  | NP(0.02), Noun(0.1) |  | NP(0.000144) | pie |
|  |  |  | P(0.6) | PP(0.036) | with |
|  |  |  |  | NP(0.06), Noun(0.3) | cream |

### Some calculations

$NP \to Noun \to John = 0.2 \times 0.2 = 0.04$

$NP \to Noun \to pie = 0.2 \times 0.1 = 0.01$

$NP \to Noun \to cream = 0.2 \times 0.3 = 0.06$

$PP \to P,NP = 1.0 \times 0.06 \times 0.6 = 0.036$

$VP \to Verb \to eats = 0.4 \times 0.3=0.12$

$NP \to NP,PP = 0.2 \times 0.02 \times 0.036=0.000144$

$VP \to VP,PP = 0.2 \times 0.12 \times 0.036=0.000864$

$VP \to Verb,NP = 0.3 \times 0.3 \times 0.02=0.0018$

$S \to NP,VP = 0.8 \times 0.04 \times 0.12=0.00384$

$S \to NP,VP = 0.8 \times 0.04 \times 0.0018=0.000058$

$VP \to Verb,NP = 0.3 \times 0.3 \times 0.06=0.0054$

$VP \to Verb,NP = 0.3 \times 0.3 \times 0.000144=0.000013$

$VP \to VP,PP = 0.2 \times 0.0018 \times 0.036=0.000013$

$S \to NP,VP = 0.8 \times 0.04 \times 0.0054=0.000173$

## PCFGs Limitations

### Independence Assumptions:

Rules assume probabilities for rules the same, no matter where they occur.

- Rule expansion is context-independent and it allows us to multiply the probabilities.
- Must annotate parents to capture info. (This is the solution to the problem)

### No Lexical Conditioning:

PCFGs do not take lexical information into account. Specific words in different subcategories result in different probabilities.

For example, consider prepositional phrase statements;

- Should it attach to the object or verb?
    - John saw the girl with the cat.
    - John saw the moon with the telescope.
- It’s not part of speech and depends on lexical items.

Lexicalized grammars systematically associate each elementary structure with a lexical anchor. This means that in each structure at least one lexical item is realized.

## PoS Tagging Implementation

The implementation is in the `PoS_Tagger.ipynb` notebook. ([Click here](Homework-03-Practical.ipynb))

<aside>
💡 It also contains the report.

</aside>

### Create test and train dataset

I shuffled the tagged sentences and consider the first %80 of this list for training and the rest of that for testing.

```python
# Shuffling
shuffle(tagged_sentences)
length_of_tagged_sentences = len(tagged_sentences)

# Split the train and test datasets
train_data, test_data = tagged_sentences[:int(length_of_tagged_sentences * 0.8)], tagged_sentences[int(length_of_tagged_sentences * 0.8):]

# Show some statistics
print(f"All the data: {length_of_tagged_sentences}")
print(f"Train data: {len(train_data)}")
print(f"Test data: {len(test_data)}")
```

I defined a function to create the X and Y by having tagged.

```python
def create_dataset(tagged_sentences):
  X, Y = [], []      
  for tagged in tagged_sentences:         
    untag_sen = [w for w, t in tagged]  
    for index in range(len(tagged)):
      X.append(features(untag_sen, index))
      Y.append(tagged[index][1])
  
  return X, Y
```

### Features

It's time to define some features for our input dataset.

```python
def features(sentence, index):
  return {
    'word': sentence[index], 
    'len': len(sentence[index]),

    # Position
    'is_first': index == 0, 
    'is_last': index == len(sentence) - 1,  
    
    # Other words
    'prev_word': '' if index == 0 else sentence[index - 1],
    'next_word': '' if index == len(sentence) - 1 else sentence[index + 1],   
    
    # Suffix & Prefix  
    'prefix-1': sentence[index][0],       
    'prefix-2': sentence[index][:2],      
    'prefix-3': sentence[index][:3],       
    'suffix-1': sentence[index][-1],     
    'suffix-2': sentence[index][-2:],      
    'suffix-3': sentence[index][-3:],     

    # Type of characters
    'is_numeric': sentence[index].isdigit(),
    'is_punc': any([sentence[index] == p for p in punctuation]),

    # Captalization
    'is_capitalized': sentence[index][0].upper() == sentence[index][0],
    'capitals_inside': sentence[index][1:].lower() != sentence[index][1:],
    'is_all_caps': sentence[index].upper() == sentence[index],
    'is_all_lower': sentence[index].lower() == sentence[index]
  }
```

### Training

I used the Sklearn package to create a pipeline. I used the DictVectorizer which can vectorize the features for each sentence. DecisionTreeClassifier is the classifier that is going to train our data.

I used the first 20000 x's of my train dataset since I didn't have enough amount of memory to train all of the train datasets.

```python
classifier = Pipeline([('vectorizer', DictVectorizer(sparse=False)),('classifier', DecisionTreeClassifier(criterion='entropy'))])
classifier.fit(x_train[:20000], y_train[:20000])
```

### Test the model

I created the train dataset to test the provided model.

```python
x_test, y_test = create_dataset(test_data)
print(f"Accuracy: %{classifier.score(x_test, y_test)}")
# Accuracy: %92.39
```