# Answer Correctness

The assessment of Answer Correctness involves gauging the accuracy of the generated answer when compared to the ground truth. This evaluation relies on the `ground truth` and the `answer`, with scores ranging from 0 to 1. A higher score indicates a closer alignment between the generated answer and the ground truth, signifying better correctness.

Answer correctness encompasses two critical aspects: semantic similarity between the generated answer and the ground truth, as well as factual similarity. These aspects are combined using a weighted scheme to formulate the answer correctness score. Users also have the option to employ a 'threshold' value to round the resulting score to binary, if desired.


```{hint}
Ground truth: Einstein was born in 1879 in Germany.

High answer correctness: In 1879, Einstein was born in Germany.

Low answer correctness: Einstein was born in Spain in 1879.

```

## Example

```{code-block} python
:caption: Answer correctness with custom weights for each variable
from ragas.metrics import AnswerCorrectness
answer_correctness = AnswerCorrectness(
    weights=[0.4,0.6]
)

# Dataset({
#     features: ['answer','ground_truth'],
#     num_rows: 25
# })
dataset: Dataset

results = answer_correctness.score(dataset)

```

## Calculation

Let's calculate the answer correctness for the answer with low answer correctness. It is computed as the sum of factual correctness and the semantic similarity between the given answer and the ground truth.

Factual correctness quantifies the factual overlap between the generated answer and the ground truth answer. This is done using the concepts of:
- TP (True Positive): Facts or statements that are present in both the ground truth and the generated answer.
- FP (False Positive): Facts or statements that are present in the generated answer but not in the ground truth.
- FN (False Negative): Facts or statements that are present in the ground truth but not in the generated answer.

In the second example:
- TP: `[Einstein was born in 1879]`
- FP: `[Einstein was born in Spain]`
- FN: `[Einstein was born in Germany]`

Now, we can use the formula for the F1 score to quantify correctness based on the number of statements in each of these lists:


```{math}
\text{F1 Score} = {|\text{TP} \over {(|\text{TP}| + 0.5 \times (|\text{FP}| + |\text{FN}|))}}
```

Next, we calculate the semantic similarity between the generated answer and the ground truth. Read more about it [here](./semantic_similarity.md).


Once we have the semantic similarity, we take a weighted average of the semantic similarity and the factual similarity calculated above to arrive at the final score. You can adjust this weightage by modifying the `weights` parameter.

