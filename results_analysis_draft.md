# Results and Analysis Draft

## Segment-level correlation with human MQM (human_score = -MQM)

### DEV all
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 933 |     0.341525 |  6.42368e-27 |      0.247097 | 5.15212e-27 |
| chrf     | 933 |     0.414664 |  4.50472e-40 |      0.296612 | 3.4376e-38  |
| comet    | 933 |     0.612554 |  3.46304e-97 |      0.454346 | 6.96017e-88 |

### DEV stress
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 465 |     0.371018 |  1.27085e-16 |      0.269758 | 9.90159e-17 |
| chrf     | 465 |     0.416922 |  5.58265e-21 |      0.301418 | 1.66497e-20 |
| comet    | 465 |     0.636341 |  3.77098e-54 |      0.475542 | 4.84921e-49 |

### DEV non-stress
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 468 |     0.29894  |  4.07517e-11 |      0.2157   | 3.75474e-11 |
| chrf     | 468 |     0.412137 |  1.2902e-20  |      0.292155 | 3.26117e-19 |
| comet    | 468 |     0.588979 |  4.85812e-45 |      0.431317 | 3.51465e-40 |

### TEST all
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 233 |     0.248333 |  0.00012792  |      0.180091 | 0.000153548 |
| chrf     | 233 |     0.237117 |  0.000260018 |      0.17394  | 0.000254025 |
| comet    | 233 |     0.4555   |  2.45457e-13 |      0.337648 | 1.07811e-12 |

### TEST stress
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 104 |     0.333629 |  0.000538264 |      0.238492 | 0.000648357 |
| chrf     | 104 |     0.368298 |  0.000119746 |      0.260262 | 0.00019623  |
| comet    | 104 |     0.532422 |  5.9867e-09  |      0.398407 | 1.10385e-08 |

### TEST non-stress
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     | 129 |     0.229684 |  0.00883387  |      0.17124  | 0.00961627  |
| chrf     | 129 |     0.207661 |  0.0182053   |      0.153817 | 0.0198318   |
| comet    | 129 |     0.367131 |  1.87188e-05 |      0.276482 | 2.68421e-05 |

## System-level correlation (small n_systems)

### DEV
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     |  13 |     0.895604 |  3.48097e-05 |      0.717949 | 0.000283978 |
| chrf     |  13 |     0.862637 |  0.000147448 |      0.717949 | 0.000283978 |
| comet    |  13 |     0.928571 |  4.60743e-06 |      0.769231 | 7.03049e-05 |

### TEST
| metric   |   n |   spearman_r |   spearman_p |   kendall_tau |   kendall_p |
|:---------|----:|-------------:|-------------:|--------------:|------------:|
| bleu     |  13 |     0.582418 |  0.0367413   |      0.410256 | 0.0572568   |
| chrf     |  13 |     0.67033  |  0.0121662   |      0.487179 | 0.0215816   |
| comet    |  13 |     0.884615 |  5.90575e-05 |      0.769231 | 7.03049e-05 |

## Metric disagreement analysis (dev only)
| measure      |   stress_n |   non_stress_n |   stress_mean |   non_stress_mean |   mean_diff_stress_minus_non |   mw_u |      mw_p |   boot_ci_low |   boot_ci_high |
|:-------------|-----------:|---------------:|--------------:|------------------:|-----------------------------:|-------:|----------:|--------------:|---------------:|
| d_comet_bleu |       1544 |           1141 |      0.710197 |          0.746062 |                   -0.035865  | 836323 | 0.0249364 |   -0.0782203  |     0.00493719 |
| d_comet_chrf |       1544 |           1141 |      0.651822 |          0.612391 |                    0.0394309 | 903267 | 0.258999  |    0.00166327 |     0.0786548  |

## Manual error analysis setup
- Annotation file: `manual_error_sample_seed6761.csv` (40 examples).
- Pool: flagged dev rows from `error_analysis_dev_seed6761.csv`.
- Taxonomy: number corruption; entity substitution/omission; negation/meaning reversal; paraphrase handling issues.

### Taxonomy counts in sampled 40
| taxonomy                     |   count |
|:-----------------------------|--------:|
| paraphrase handling issues   |      20 |
| entity substitution/omission |      19 |
| number corruption            |       1 |

### 6 Representative examples

**Example 1** (high_comet_bad_mqm, paraphrase handling issues, MQM=32.400000000000006, NLLB_MBR_BLEU)
- Source: The more I read of studies on sex, gender, psychology, the more the statement of "My sex is male, my gender is female" seems simplistic and inaccurate. It almost seems like ceding part of an argument to TERFs. I feel tha
- MT: Je mehr ich über Studien über Sex, Geschlecht, Psychologie lese, desto mehr erscheint mir die Aussage "Mein Geschlecht ist männlich, mein Geschlecht ist weiblich" vereinfacht und ungenau. Es scheint fast, als würde man d
- Ref: Je mehr ich über Studien zu Sex, Geschlecht und Psychologie lese, desto eher scheint die Aussage „Ich bin männlich, mein Geschlecht ist weiblich“ vereinfacht und ungenau zu sein. Es scheint fast, als würde man einen Teil
- MQM category/severity: Terminology/Inappropriate for context/Major

**Example 2** (high_comet_bad_mqm, paraphrase handling issues, MQM=21.0, ONLINE-M)
- Source: The more I read of studies on sex, gender, psychology, the more the statement of "My sex is male, my gender is female" seems simplistic and inaccurate. It almost seems like ceding part of an argument to TERFs. I feel tha
- MT: Je mehr ich über Studien zu Sex, Gender und Psychologie lese, desto vereinfachter und ungenauer erscheint die Aussage „Mein Geschlecht ist männlich, mein Geschlecht ist weiblich“. Es scheint fast so, als würde man einen 
- Ref: Je mehr ich über Studien zu Sex, Geschlecht und Psychologie lese, desto eher scheint die Aussage „Ich bin männlich, mein Geschlecht ist weiblich“ vereinfacht und ungenau zu sein. Es scheint fast, als würde man einen Teil
- MQM category/severity: Terminology/Inappropriate for context/Major

**Example 3** (high_bleu_bad_mqm, paraphrase handling issues, MQM=24.0, ONLINE-Y)
- Source: The more I read of studies on sex, gender, psychology, the more the statement of "My sex is male, my gender is female" seems simplistic and inaccurate. It almost seems like ceding part of an argument to TERFs. I feel tha
- MT: Je mehr ich über Studien zu Sex, Gender und Psychologie lese, desto mehr erscheint die Aussage „Mein Geschlecht ist männlich, mein Geschlecht ist weiblich“ simpel und ungenau. Es scheint fast so, als würde man einen Teil
- Ref: Je mehr ich über Studien zu Sex, Geschlecht und Psychologie lese, desto eher scheint die Aussage „Ich bin männlich, mein Geschlecht ist weiblich“ vereinfacht und ungenau zu sein. Es scheint fast, als würde man einen Teil
- MQM category/severity: Terminology/Inappropriate for context/Major

**Example 4** (high_chrf_bad_mqm, entity substitution/omission, MQM=35.0, ZengHuiMT)
- Source: So like from the beginning for all my birthdays in the past, I always had rough ones. Whenever May would pop up I dreaded trying to celebrate my bday. Something would always happen, go wrong, or just end up stressful and
- MT: Also wie von Anfang an bei all meinen Geburtstagen in der Vergangenheit hatte ich immer schwierige Geburtstage. Immer wenn der Mai auftauchte, fürchtete ich mich vor dem Versuch, meinen Geburtstag zu feiern. Irgendetwas 
- Ref: Also, wie bei allen meinen Geburtstagen in der Vergangenheit, war es immer schwierig für mich. Immer, wenn es Mai wurde, fürchtete ich mich vor dem Versuch, meinen Geburtstag zu feiern. Etwas sollte immer passieren, schi
- MQM category/severity: Accuracy/Addition/Major

**Example 5** (low_bleu_good_mqm, paraphrase handling issues, MQM=0.0, NLLB_Greedy)
- Source: In 1998-06 Gamefan, in relation to the lon
**Example 5** (low_bleu_good_mqm, paraphrase handling issues, MQM=0.0, NLLB_Greedy)
- Source: In 1998-06 Gamefan, in relation to the long development of the first Unreal game, Jason Schrieber commented,
- MT: In 1998-06 Gamefan, in Bezug auf die lange Entwicklung des ersten Unreal-Spiel, Jason Schrieber kommentiert,
- Ref: Im Gamefan 06/1998 kommentierte Jason Schrieber bezüglich der langen Entwicklungszeit des ersten Unreal-Spiels,
- MQM category/severity: No-error/No-error

**Example 6** (low_comet_good_mqm, paraphrase handling issues, MQM=0.0, ONLINE-A)
- Source: Much as we might like some folks, if they are just providing a minor league career to fascists, they probably should wear it.
- MT: So sehr wir auch einige Leute mögen mögen, wenn sie Faschisten nur eine Karriere in der Minor League ermöglichen, sollten sie es wahrscheinlich tragen.
- Ref: So sehr wir manche Leute auch schätzen mögen, wenn sie Faschisten nur eine unbedeutende Karriere bieten, sollten sie es vielleicht tragen.
- MQM category/severity: No-error/No-error
