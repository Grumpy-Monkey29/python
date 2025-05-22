## Isolation Forest
### Predict Vs Decision Function Scores
- The predictions output will contain values of 1 and -1. A value of 1 indicates that the data point is considered an inlier (normal), and -1 indicates that it is considered an outlier (anomaly).
- The decision_scores output will contain the anomaly scores for each data point. The scores indicate how isolated each sample is compared to the rest of the dataset. Higher scores (more positive values) indicate that the data point is likely to be an inlier, while lower scores (more negative values) suggest that the point is an outlier.
- Each SHAP value for a feature represents the contribution of that feature's value to the difference between the predicted score for that instance and the base value (average prediction)
- Higher positive SHAP values for a feature mean that the feature's value for that instance pushed the prediction towards a higher anomaly_score (more anomalous)

| Term                  | What It Means                                             | Analogy                                                       |
| --------------------- | --------------------------------------------------------- | ------------------------------------------------------------- |
| **Contamination**     | The percent of the data you assume is weird or suspicious | You guess "maybe 10% of visitors today are acting strange"    |
| **Anomaly Score**     | How weird someone is, on a scale                          | Lower score = stranger                                        |
| **Threshold**         | A cutoff score — below it, someone is marked suspicious   | You set a “weirdness” limit: below this score → call security |
| **Labeled Anomalies** | Final decision: who is suspicious (`-1`) vs normal (`1`)  | You tag people as suspicious or okay based on the threshold   |


- When you apply model.predict() or model.decision_function(), it's assigning a score or classification based on this learned structure, not on a pre-defined notion of what an "anomaly" is in your domain.
- the `anomaly score (from model.decision_function(X))` is defined such that:
  - `Lower scores = more anomalous`
  - `Higher scores = more normal`
---
- You're a librarian in charge of organizing books.
- You don't know what books are bad or weird in general — you just know what most books in your library usually look like:
  1. They're mostly novels
  2. Between 200–400 pages
  3, Normal-sized fonts
  4. Written in English
  5. You’ve never seen a “bad” book. No one told you “this is what a weird book looks like.”
---
- 📦 Step 1: The Librarian Learns
- You scan the whole library and say:
“Hmm… most books are pretty similar. If I see something too different, I’ll call it weird.”
You start looking at:
  1. Page count
  2. Font size
  3. Language
  4. Genre
- And you build a mental model: “This is what a normal book looks like.”
---
- 🧮 Step 2: You Apply Your Judgment
- Now, new books come in. You look at one:
  1. 50 pages, in Klingon, in size 30 font
  2. You don’t know if Klingon books are bad.
  3. You don't have a list that says "Klingon = weird."

- But your brain goes:

- "Whoa. That’s way off from everything else I’ve seen."
- You give it a weirdness score, and maybe call it anomalous.
---
- ✅ This is exactly what Isolation Forest does:
- It learns the pattern of what “normal” looks like from your data
- It doesn't know your real-world definition of an anomaly
- It just says: “This point is unlike the majority I saw earlier.”

- 🔄 model.decision_function():
  - “How far off is this point from normal?”
  - Gives a score (lower = more weird)
- 🔄 model.predict():
  - “Based on the contamination level, should I call this an outlier?”
  - Uses the score and threshold to say -1 = anomaly or 1 = normal
---
- The model isn’t judging based on your definition of weird — it’s judging based on what it learned from the data you gave it.
- 🎯 contamination is a blind assumption
- It tells the model:
- "Please label this proportion of the data as anomalies, no matter what."
- 🔍 What Happens Internally?
  - The model learns what “normal” looks like (based on isolation depth).
  - But when you call predict(), it uses the contamination value to set a threshold on anomaly scores.
  - It then forces that percentage of the data (10%) to be labeled as -1 (anomalous) — even if they’re not truly weird in your domain.
---
- You have a dataset where 20% of the data points are true anomalies, but you tell the IsolationForest: contamination=10%
- 🤖 What the Model Does:
  - Learns the structure of the data using all points.
  - Computes anomaly scores for all samples (lower score = more "anomalous").
  - Sorts the scores from lowest to highest.
  - Picks a threshold so that only the bottom 10% (by score) are labeled as anomalies (-1).
  - Ignores the rest — even if they are actually anomalies in your domain.
- ✅ Result:
  - The model will only label 10% of the data as anomalies — the ones with the lowest scores.
  - The remaining 10% of true anomalies with slightly higher scores will be missed (i.e., false negatives).
---


