# Veridion-Project


When I first built the solution, I started with a forced matching approach: for every company in the dataset, I picked the “best possible match” from the candidates, even if the match wasn’t actually very strong.
At first, this seemed fine because it guaranteed that every input had an output. But after reviewing the results, I wasn’t really satisfied with it, because forcing a match when the data is uncertain can be rather risky.
So I changed my approach  and decided to build a second version of the pipeline where I introduced a confidence threshold; instead of forcing the match, it asks a simple question: “Is this match actually good enough to trust?”
•	If the answer is yes: we keep the match
•	If the answer is no:we return “No match”.
Let me explain my thinking process: in my first version, I built a model that for each company in the dataset, it looked at all possible candidate matches and picked the one that looked the most similar. Basically, even if the match wasn’t the best, the system had to choose something.
1.	The first model

For each input company, I had a list of potential candidates, respectively for each of these, I calculated a similarity score based on a few signals:
•	how similar the company names are (using fuzzy matching logic)
•	whether the country matches
•	whether the city matches
•	whether there is a website/ domain signal available;
Each of these signals contributed to a final score that measured how likely is it that two records refer to the same real-world company. After scoring all candidates, it selected the one with the highest score. The grouping logic was to group all candidates by input company and after to pick the row with the maximum score.
So the system to return could never return “no match”, which I thought afterwards that it’s not 100% correct. The biggest limitation is that it does not handle uncertainty; it represents a good start in my opinion, but it does not return the best there could be.

2.	The second model

The final pipeline works as following:for each input company, I compared it with its possible matches using: fuzzy name similarity (to handle spelling differences and variations), country match (strong signal), city match (weaker signal) and last but not least - optional website/ domain signals. Each of these “signals” contribute to a final score between 0 and 1.Afterwards, it picks the candidate with the highest score, but not before checking its quality. If the score is way too low, it labels it, so there is no guessing left.

3.	Conclusion

I kept both of the models because they show two different concepts, two ways of handling data, but especially because I wanted to literally show the flow of the thought process that I had.
