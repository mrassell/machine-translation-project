This report presents an error analysis of a large-scale machine translation (MT) dataset consisting of hundreds of thousands of translated segments with human-annotated errors. The goal is to identify systematic failure modes of MT systems and evaluate how well current automatic metrics capture these errors.

The analysis focuses on three main dimensions:

Accuracy (Faithfulness/Hallucinations)
Fluency (Correct grammatical structure)
Style (How natural translated sentences feel)


2. Error Distribution Overview

The dataset reveals a highly skewed distribution of error types:

Accuracy errors (dominant)
Style errors (notably unnatural or awkward translations)
Fluency errors (less frequent and more surface-level)

The distribution of type within the dataset is as follows:

| Category     | Total Count | Share of Errors | Key Insight                                                |
|--------------|-------------|-----------------|------------------------------------------------------------|
| Accuracy     | 1179        | Highest         | Issues related to preserving meaning and structure.        |
| Fluency      | 645         | Medium          | Mostly grammatical or general surface-level issues.        |
| Style        | 368         | Medium          | Naturalness and improper interpretation of idioms.         |
| Terminology  | 82          | Low             | Inconsistent meaning between terms and their translations. |
| Source Issue | 56          | Low             | Noise and/or Input related issues.                         |

Accuracy-related errors dominate the distribution, suggesting that current MT systems struggle more with semantic fidelity than with surface-level grammatical correctness. This distribution already suggests that modern MT systems struggle more with meaning preservation and natural expression than with grammatical correctness. 

Going further in depth, of the accuracy errors present within the data set, mistranslations accounted for over 75% of the issues presented, whereas the other categories, Addition, Reinterpretation, Omission, and Fragmenting, were minimal.


3. Accuracy Errors

Accuracy errors are the most frequent and most impactful category. They reflect failures to correctly preserve the meaning of the source text.

3.1 Addition (Overgeneration)

Addition errors occur when the model generates content not present in the source.

Observed patterns:

Repetition (e.g., duplicated phrases)
Redundant paraphrasing
Hallucinated content
Mixed cases (partial translation + addition)

Interpretation:
These errors indicate overgeneration, where the model prioritizes fluency and completeness over faithfulness.

Key insight:
Even severe addition errors can receive high scores from metrics like BLEU Score and chrF, demonstrating their inability to penalize duplicated or hallucinated content.

3.2 Omission (Compression)

Omission errors occur when parts of the source text are missing in the translation.

Observed patterns:

Dropped modifiers (e.g., “essentially”)
Missing clauses
Omitted discourse elements (e.g., “It worked”)
Loss of descriptive details

Interpretation:
These errors reflect a compression strategy, where the model simplifies complex input.

Key insight:
Omissions often preserve overall meaning but reduce nuance and completeness, making them difficult for automatic metrics to detect.

3.3 Mistranslation (Incorrect Meaning)

Mistranslation errors involve incorrect rendering of source meaning.

Observed patterns:

Misinterpreted idioms and informal expressions
Literal translation of figurative language
Argument structure errors (role reversal)
Incorrect lexical choices

Interpretation:
These errors represent true semantic failures, often caused by ambiguity or domain mismatch (e.g., social media language).

For example, abbreviated terms often used in internet dialogues (i.e. FWIW) were approximated incorrectly. The NLB systems (Greedy and MBR Bleu) had difficulty understanding the acronym and would often assume a mispelling of Frau:

"(fwiw, I've tried nearly every 'humane' trap on the market with very little success. I'm not especially happy with killing them, but I won't be taking comments on the ethics of killing mice.)"

After translation became:
"(Frau, ich habe fast jede 'menschliche' Falle auf dem Markt mit sehr wenig Erfolg ausprobiert. Ich bin nicht besonders glücklich, sie zu töten, aber ich werde keine Kommentare über die Ethik des Tötens von Mäusen annehmen.)"

3.4 Creative Reinterpretation (Semantic Drift)

Creative reinterpretation refers to translations that are fluent and plausible but deviate subtly from the source.

Observed patterns:

Idiom misinterpretation
Stylistic paraphrasing with tone shifts
Incorrect translation of named entities

Interpretation:
These errors highlight a trade-off between fluency and precision, where the model produces acceptable but not equivalent translations.


4. Style Errors

4.1 Unnatural or Awkward Expressions

These errors occur when translations are grammatically correct but sound non-native.

Observed patterns:

Literal translation of expressive phrases
Non-idiomatic lexical choices
Awkward phrasing
Register mismatches

Interpretation:
MT systems often produce fluent but non-native output, especially in informal contexts.

Key insight:
These errors frequently receive perfect or near-perfect scores from:

BLEU Score
chrF

This demonstrates that current metrics fail to capture naturalness and pragmatic appropriateness.


5. Fluency Errors
5.1 Grammatical and Surface Issues

Fluency errors include grammatical, punctuation, and orthographic issues.

Observed patterns:

Agreement and inflection errors
Word order issues
Punctuation inconsistencies
Minor spelling/capitalization issues

Interpretation:
These errors are relatively infrequent and less severe compared to accuracy errors.

Key insight:
Unlike other categories, fluency errors are more reliably captured by:

BLEU Score
chrF
COMET


6. Cross-Cutting Observations
6.1 Overgeneration vs Compression

Two opposing tendencies are observed:

Addition → overgeneration
Omission → compression

This suggests instability in how models balance completeness and conciseness.

6.2 Impact of Informal Text

The dataset consists largely of informal, social media-style text, which introduces:

Abbreviations (e.g., “fwiw”)
Figurative language
Non-standard syntax

These factors significantly increase error rates, particularly for:

mistranslation
omission
style issues
6.3 Metric Limitations

A major finding is the mismatch between human judgments and automatic metrics:

BLEU Score and chrF:
Fail to penalize:
repetition
omissions
subtle meaning shifts
COMET:
More robust
Still misses:
pragmatic errors
fine-grained semantic differences


7. Conclusion

This analysis shows that modern MT systems:

Can produce fluent and grammatically correct output
Can capture general meaning effectively

But struggle with:

Staying faithful to the source content (addition, omission, mistranslation)
Maintaining semantic precision (creative reinterpretation)
Naturalness and idiomaticity (style errors)

Overall, the results suggest that current MT systems are strong at surface-level quality but still lack robust semantic and pragmatic control, particularly in informal and expressive text domains.

The following is a more extensive breakdown of the supplied errors produced by error_analysis_display.ipynb:

# MT Error Analysis Summary

## Error Distribution
| Error Category                        |   Count |   Percentage |
|:--------------------------------------|--------:|-------------:|
| Accuracy/Mistranslation               |     921 |   34.3657    |
| Style/Unnatural or awkward            |     344 |   12.8358    |
| Fluency/Punctuation                   |     317 |   11.8284    |
| No-error                              |     232 |    8.65672   |
| Fluency/Grammar                       |     227 |    8.47015   |
| Found                                 |      92 |    3.43284   |
| Accuracy/Omission                     |      85 |    3.17164   |
| Terminology/Inappropriate for context |      81 |    3.02239   |
| Fluency/Spelling                      |      74 |    2.76119   |
| Accuracy/Source language fragment     |      73 |    2.72388   |
| Accuracy/Creative Reinterpretation    |      62 |    2.31343   |
| Source issue                          |      56 |    2.08955   |
| Accuracy/Addition                     |      38 |    1.41791   |
| Style/Bad sentence structure          |      24 |    0.895522  |
| Other                                 |      20 |    0.746269  |
| Fluency/Register                      |      15 |    0.559701  |
| Fluency/Inconsistency                 |      12 |    0.447761  |
| Locale convention/Currency format     |       3 |    0.11194   |
| Missed                                |       3 |    0.11194   |
| Terminology/Inconsistent              |       1 |    0.0373134 |

## Severity Breakdown
| category                              |   HOTW-test |   Major |   Minor |   No-error |
|:--------------------------------------|------------:|--------:|--------:|-----------:|
| Accuracy/Addition                     |           0 |      28 |      10 |          0 |
| Accuracy/Creative Reinterpretation    |           0 |       2 |      60 |          0 |
| Accuracy/Mistranslation               |           0 |     385 |     536 |          0 |
| Accuracy/Omission                     |           0 |      71 |      14 |          0 |
| Accuracy/Source language fragment     |           0 |      43 |      30 |          0 |
| Fluency/Grammar                       |           0 |      25 |     202 |          0 |
| Fluency/Inconsistency                 |           0 |      11 |       1 |          0 |
| Fluency/Punctuation                   |           0 |       7 |     310 |          0 |
| Fluency/Register                      |           0 |       1 |      14 |          0 |
| Fluency/Spelling                      |           0 |       1 |      73 |          0 |
| Found                                 |          92 |       0 |       0 |          0 |
| Locale convention/Currency format     |           0 |       0 |       3 |          0 |
| Missed                                |           3 |       0 |       0 |          0 |
| No-error                              |           0 |       0 |       0 |        232 |
| Other                                 |           0 |      11 |       9 |          0 |
| Source issue                          |           0 |      23 |      33 |          0 |
| Style/Bad sentence structure          |           0 |      22 |       2 |          0 |
| Style/Unnatural or awkward            |           0 |      53 |     291 |          0 |
| Terminology/Inappropriate for context |           0 |      43 |      38 |          0 |
| Terminology/Inconsistent              |           0 |       0 |       1 |          0 |

## System Comparison
| system        |   Accuracy/Addition |   Accuracy/Creative Reinterpretation |   Accuracy/Mistranslation |   Accuracy/Omission |   Accuracy/Source language fragment |   Fluency/Grammar |   Fluency/Inconsistency |   Fluency/Punctuation |   Fluency/Register |   Fluency/Spelling |   Found |   Locale convention/Currency format |   Missed |   No-error |   Other |   Source issue |   Style/Bad sentence structure |   Style/Unnatural or awkward |   Terminology/Inappropriate for context |   Terminology/Inconsistent |
|:--------------|--------------------:|-------------------------------------:|--------------------------:|--------------------:|------------------------------------:|------------------:|------------------------:|----------------------:|-------------------:|-------------------:|--------:|------------------------------------:|---------:|-----------:|--------:|---------------:|-------------------------------:|-----------------------------:|----------------------------------------:|---------------------------:|
| AIRC          |                   5 |                                    2 |                       132 |                  26 |                                  12 |                24 |                       0 |                    31 |                  0 |                  5 |       6 |                                   0 |        0 |          6 |       0 |              4 |                              1 |                           34 |                                       6 |                          0 |
| GPT4-5shot    |                   1 |                                    2 |                        41 |                   1 |                                   2 |                21 |                       0 |                    30 |                  1 |                  7 |       8 |                                   0 |        0 |         17 |       1 |              7 |                              2 |                           32 |                                       2 |                          0 |
| Lan-BridgeMT  |                   1 |                                   11 |                        95 |                   3 |                                   7 |                32 |                       2 |                    32 |                  0 |                 10 |       9 |                                   1 |        0 |         12 |       2 |              5 |                              1 |                           33 |                                       8 |                          0 |
| NLLB_Greedy   |                   1 |                                    4 |                        89 |                  22 |                                   3 |                22 |                       0 |                    31 |                  0 |                  7 |      10 |                                   0 |        0 |         11 |       2 |              4 |                              1 |                           28 |                                       6 |                          0 |
| NLLB_MBR_BLEU |                   2 |                                    6 |                        96 |                  19 |                                   7 |                24 |                       0 |                    33 |                  1 |                  5 |       7 |                                   0 |        0 |         13 |       2 |              1 |                              4 |                           31 |                                       9 |                          0 |
| ONLINE-A      |                   0 |                                    3 |                        59 |                   1 |                                   5 |                 8 |                       1 |                    31 |                  0 |                  7 |       5 |                                   0 |        0 |         26 |       0 |              2 |                              3 |                           20 |                                       7 |                          0 |
| ONLINE-B      |                   2 |                                    2 |                        47 |                   1 |                                   2 |                15 |                       0 |                     7 |                  1 |                  1 |       7 |                                   0 |        0 |         27 |       0 |              0 |                              2 |                           20 |                                       6 |                          0 |
| ONLINE-G      |                   3 |                                    6 |                        86 |                   1 |                                   4 |                24 |                       4 |                    37 |                  2 |                  3 |       3 |                                   0 |        0 |         16 |       0 |              2 |                              1 |                           25 |                                       5 |                          0 |
| ONLINE-M      |                   1 |                                    6 |                        68 |                   6 |                                   7 |                12 |                       0 |                     8 |                  0 |                  5 |       6 |                                   1 |        1 |         20 |       1 |              5 |                              3 |                           23 |                                       7 |                          1 |
| ONLINE-W      |                   0 |                                    4 |                        32 |                   2 |                                   1 |                 6 |                       2 |                    36 |                  1 |                  3 |       6 |                                   0 |        0 |         23 |       3 |              8 |                              0 |                           23 |                                       9 |                          0 |
| ONLINE-Y      |                   1 |                                    7 |                        67 |                   2 |                                  16 |                14 |                       3 |                    10 |                  1 |                  6 |      10 |                                   1 |        1 |         18 |       1 |              5 |                              1 |                           28 |                                       9 |                          0 |
| ZengHuiMT     |                  19 |                                    4 |                        84 |                   1 |                                   4 |                16 |                       0 |                    24 |                  5 |                 10 |       7 |                                   0 |        1 |         12 |       7 |              7 |                              5 |                           24 |                                       6 |                          0 |
| refA          |                   2 |                                    5 |                        25 |                   0 |                                   3 |                 9 |                       0 |                     7 |                  3 |                  5 |       8 |                                   0 |        0 |         31 |       1 |              6 |                              0 |                           23 |                                       1 |                          0 |

## Metric Summary by Error Type
| category                              |    bleu |    chrf |    comet |
|:--------------------------------------|--------:|--------:|---------:|
| Accuracy/Addition                     | 37.4323 | 66.277  | 0.744756 |
| Accuracy/Creative Reinterpretation    | 41.0565 | 66.6539 | 0.786506 |
| Accuracy/Mistranslation               | 36.1021 | 63.5081 | 0.73467  |
| Accuracy/Omission                     | 21.9827 | 50.3372 | 0.659432 |
| Accuracy/Source language fragment     | 43.6136 | 68.0208 | 0.743394 |
| Fluency/Grammar                       | 38.7367 | 66.0271 | 0.777332 |
| Fluency/Inconsistency                 | 22.6379 | 53.0298 | 0.790387 |
| Fluency/Punctuation                   | 37.316  | 65.3848 | 0.791579 |
| Fluency/Register                      | 56.2657 | 74.3939 | 0.776782 |
| Fluency/Spelling                      | 38.8905 | 64.9402 | 0.747356 |
| Found                                 | 47.3881 | 71.4462 | 0.803233 |
| Locale convention/Currency format     | 40.3071 | 60.9541 | 0.921962 |
| Missed                                | 40.0107 | 65.5815 | 0.79231  |
| No-error                              | 56.9352 | 78.7482 | 0.896359 |
| Other                                 | 40.4654 | 65.3214 | 0.710155 |
| Source issue                          | 45.6571 | 66.5847 | 0.766377 |
| Style/Bad sentence structure          | 28.856  | 63.4785 | 0.773866 |
| Style/Unnatural or awkward            | 41.6137 | 67.7273 | 0.782435 |
| Terminology/Inappropriate for context | 38.663  | 68.1634 | 0.826783 |
| Terminology/Inconsistent              | 29.8985 | 59.5602 | 0.827795 |
