# Beam Consistency for Hallucination Detection

## Summary

Experiments in measuring the semantic consistency between beam generations of a language model in order to detect and 
mitigate hallucinations.

The hypothesis is that information generated by language models should correlate highly with repeated generations when
factual, but not when hallucination is occurring. 

The process is:
1. Generate N samples for a given context, the goal is to have a representative distribution over all end states, so
these beams should be independent from the start
2. Collect the embedding states within the final layer for the final sequence of each generation
3. Measure the pairwise cosine similarity between each of these embedding vectors. From this, two main data can be 
extracted: 
   - the average of all the similarities, which *should* represent something like the confidence of the final generations
   - the medoid of final sequences (ideally weighted to ignore outliers), which *should* represent the highest confidence answer


## Issues/Todo
- Re #2 - this may not be the best way to extract the relevant features, as I think it weighs length/wording too highly
  - perhaps something that pays great attention to specific data points such as numbers/proper names would be better
- How do you scale average cos similarity to represent confidence? its tricky
  - the dimensionality of the embeddings skews this, as higher dimensional vectors trend towards lower cos similarities
  [see 0.2.3 here](https://www.cs.princeton.edu/courses/archive/fall13/cos521/lecnotes/lec11.pdf)
  - high dimensionality also creates issues with precision, running some of my tests with in bf16 caused infs and nans,
  which I was only able to fix by bumping it up to fp64




### Refs
[[1] Detecting and Mitigating Hallucinations of LLMs by Validating Low-Confidence Generation](https://arxiv.org/pdf/2307.03987.pdf)

[[2] Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/pdf/2203.11171.pdf)

