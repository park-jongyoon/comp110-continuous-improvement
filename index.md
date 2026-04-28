---
layout: default
title: "Continuous Improvement for COMP110 — Jongyoon Park"
description: "A data-driven proposal for adding optional pre-lecture videos to COMP110, based on the Spring 2026 mid-semester survey."
---

# Continuous Improvement for COMP110: Who Benefits Most From Optional Pre-Lecture Videos?

<p class="byline">By Jongyoon Park</p>

## Summary

COMP110 enrolls a student body that is mostly non-CS (713 of 764, or 93%) and mostly without prior programming experience (478 of 764, or 63%). In the anonymized mid-semester survey, 51% of respondents rated optional pre-lecture videos at 6 or 7 on a 7-point support scale. Read at the class level, that looks like a clear endorsement. The more interesting question, and the one this project takes up, is whether that endorsement is uniform or whether it sits in a particular corner of the class. The analysis below works through five passes over the data and finds that demand for pre-lecture videos is concentrated in the largest student group (beginners) and in the most concrete pain point in the survey (a pace that feels fast).

---

## Analysis

The starting point for this project was a personal observation. As a Statistics major with a few months of prior programming experience, the live lecture pace already felt brisk, which made it worth asking whether that experience generalizes across the class or is idiosyncratic. The two anonymized survey CSVs (one per section) were combined with `read_csv_rows`, `columnar`, and `concat` from the course's `data_utils` into a single 764-response table. From there, four columns carry the analysis: `prior_exp` (programming background), `comp_major` (CS-major intent), `pace` (perceived pace of the course), and `pre_lecture_videos` (level of agreement that optional pre-lecture videos would help).

The first pass establishes the overall shape of demand. A histogram of `pre_lecture_videos` shows the distribution leaning heavily toward agreement: the modal response is 7 (250 students, roughly 33% of the sample), and the combined 6-and-7 group accounts for 51% of all responses. A meaningful minority disagrees, with about 14% of students sitting at ratings 1, 2, or 3.

![Histogram of pre-lecture video support ratings]({{ '/charts/01_overall_support.png' | relative_url }})

Demand is broad in the aggregate, but the more useful question is *who* is driving it. Splitting the same column by prior programming experience answers part of that question. A boxplot of `pre_lecture_videos` against `prior_exp`, ordered from least to most experience, shows the median falling as experience rises. Beginners (the "None to less than one month" bucket, n = 478) sit at a median of 6 with a tight upper quartile pinned at 7. Students with more than two years of prior experience (n = 15) sit at a median of 5 and show a much wider distribution that reaches into the disagreement zone. In proportional terms, 56.3% of beginners rate support at 6 or higher, compared with only 33.3% of the most experienced students.

![Boxplot of pre-lecture video support by prior programming experience]({{ '/charts/02_by_prior_exp.png' | relative_url }})

Prior experience is one axis, but it is not the only one. The proposal for pre-lecture videos rests on the idea that they help students keep up with the live pace, which means demand should also concentrate among students who report the pace feels fast. The joint distribution of `pace` and `pre_lecture_videos`, rendered as a 7-by-7 heatmap, makes that pattern easy to read. The single hottest cell is at `pace = 5, pre_lecture_videos = 7` (83 students), and the upper-right region (pace at least 5 and pre_lecture_videos at least 6) holds the densest cluster of responses anywhere in the matrix: 243 students, or 31.8% of the combined sample. Students who feel the course is moving fast and students who want pre-lecture videos are largely the same people.

![Heatmap of perceived pace against pre-lecture video support]({{ '/charts/03_pace_heatmap.png' | relative_url }})

A reasonable concern with adding pre-lecture videos is that they might end up serving primarily the CS-track students, who are arguably the most invested in the material to begin with. That concern does not survive the data. Mean support across the four `comp_major` categories falls in a narrow band, from roughly 5.3 to 5.9, and that range includes the 713 non-CS-majors who form the bulk of the class. The benefit of the proposed change is shared broadly, not captured by a CS-bound minority.

![Bar chart of mean pre-lecture video support across CS-major intent categories]({{ '/charts/04_by_comp_major.png' | relative_url }})

The final pass narrows the lens to the population the proposed change is most directly aimed at. A custom helper function, `filter_by_value`, isolates the 478 beginners (63% of the class). Within that subgroup, perceived pace is bucketed into Slow (1-3), Moderate (4), and Fast (5-7), and the distribution of `pre_lecture_videos` is shown as a violin plot. The shape lifts visibly as perceived pace rises. Beginners who report the course feels fast cluster near the top of the support scale, with most of their distribution sitting at 6 and 7, while beginners reporting a slower pace show a distribution that is much more spread out. Within the beginner subgroup overall, 57.1% report the pace feels fast and 56.3% want videos.

![Violin plot of pre-lecture video support among beginners, split by perceived pace bucket]({{ '/charts/05_beginners_violin.png' | relative_url }})

---

## Conclusion

Taken across the five passes, the data supports the proposed change. The overall mean support for pre-lecture videos is 5.33 / 7. Demand is concentrated where the analysis predicts it should concentrate: among students with the least prior experience, and among students who report that the live pace feels fast. Mean support is also similar across all four CS-intent categories, so the benefit is not narrowly captured by a single segment of the class. A reasonable next move for COMP110 is to pilot a small set of short, optional pre-lecture videos focused on beginner-level concepts.

The change is not free. The most direct cost falls on the instructional staff: producing high-quality videos takes meaningful time to script, record, and edit, and that time has to come from somewhere, usually from preparation for live lectures or office-hours availability. Optional videos can also produce a quieter form of attendance erosion when students treat them as a substitute for live instruction rather than as a supplement. The 14% of respondents at ratings 1, 2, or 3 actively disagree that pre-lecture videos would help, and the most experienced of those students may perceive resources being directed at content that is not aimed at them. Finally, video maintenance becomes an ongoing rather than a one-time cost whenever the syllabus shifts across semesters.

Several follow-up steps would sharpen the recommendation. The first is to pilot pre-lecture videos for a single unit (loops or dictionaries are natural candidates) and re-survey afterward, comparing the unit's `understanding` and `pace` scores against a comparable past unit. The second is to host the videos on a platform that records view counts, so the predicted high-need subgroup (beginners who feel the pace is fast) can be checked against the actual audience. The third is a two-tier production model that pairs short "intuition only" videos for beginners with longer optional "deep-dive" videos for the experienced students who currently feel under-challenged; that pairing addresses both ends of Idea 4 (tiered exercises) without requiring a full curricular overhaul. The fourth is a small addition to future surveys: a question asking "Did you watch the pre-lecture videos?", cross-referenced with `understanding`, `pace`, and `would_recommend`, would let the next iteration of this analysis move from correlation toward a closer-to-causal estimate of the videos' effect.