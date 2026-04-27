# Promoting Conservation Behaviors via Optimistic & Pessimistic Framing — EPFL Replication

**Course:** HUM-403 Experimental Cognitive Psychology I (EPFL, MA2)
**Instructor:** Dr. Domicele Jonauskaite
**Authors:** Miquel López, Enric Guasch, Desiré Díaz
**Date:** 21 December 2025

A pre-registered-style replication and small extension of:

> Martell, J. E. M., & Rodewald, A. D. (2024). *Promoting Conservation Behaviors by Leveraging Optimistic and Pessimistic Messages and Emotions.* Society & Natural Resources, 37(4), 564–585. https://doi.org/10.1080/08941920.2023.2294847

---

## 1. Background

Biodiversity loss is hard to communicate in ways that motivate action. **Message framing** (loss vs. gain) and the **emotions** that frames evoke are two levers conservation campaigns lean on. Loss-framed messages can leverage loss aversion (Kahneman & Tversky, 1979) and elicit fear or sadness; gain-framed messages can elicit hope and a sense of efficacy (van der Linden, 2015). However, overly bleak messages risk fatalism, while purely positive ones may understate urgency.

Martell & Rodewald (2024) tested this in a U.S. online experiment using messaging from the *3 Billion Birds* campaign. They found that:

- **Pessimistic framing produced higher willingness to donate than optimistic framing.**
- **Negative emotions partially mediated** the frame → behavior path.
- **The combined frame did NOT outperform single-frame conditions.**

This project replicates the design in a European sample (n ≈ 200) and adds an objective behavioral measure (donation-link click tracking).

---

## 2. Research Question & Hypotheses

> *How do pessimistic, optimistic, and combined conservation messages affect emotions and conservation-related intentions in a European sample, and do emotions mediate these effects?*

| #  | Hypothesis                                                                                                          | Test                                                |
|----|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| H1 | Pessimistic frame → higher negative affect; optimistic frame → higher positive affect (vs. each other).             | Linear regression + pairwise (Bonferroni-corrected) |
| H2 | Pessimistic > optimistic on donation intentions.                                                                    | Linear regression + pairwise                        |
| H3 | Combined frame ≯ single-frame conditions on behavioral intentions.                                                  | Linear regression                                   |
| H4 | Positive and/or negative affect mediates frame → behavioral intentions.                                             | Hayes PROCESS Model 4, 10 000 bootstrap iterations  |

---

## 3. Design

**Between-subjects, 4 conditions, random assignment (≈25 % each), stratified on age & gender.**

| Condition       | Stimulus                                                  |
|-----------------|-----------------------------------------------------------|
| **Pessimistic** | Text + image emphasizing 2.9 B birds lost                 |
| **Optimistic**  | Text + image emphasizing raptor / waterfowl recovery      |
| **Combined**    | Both texts + both images                                  |
| **Control**     | No message — skips directly to outcome measures           |

**Flow:**

```
CONSENT → DEMOGRAPHICS → RANDOM ASSIGNMENT
   ├─ [Pess. / Opt. / Combined] → MESSAGE+IMAGE → ATTENTION CHECK → EMOTIONS
   └─ [Control] ─────────────────────────────────────────────────────────────┐
                                                                             ▼
                       BEHAVIORAL INTENTIONS (all groups, fixed order):
                          1. Seven Simple Actions  (pro-conservation, 7 items)
                          2. Political Engagement  (7 items)
                          3. Willingness to Donate (3 binary items)
                                          │
                              DEBRIEFING + DONATION LINK
                              (click tracked: 1 = clicked, 0 = not)
```

---

## 4. Sample

| Aspect              | Original (Martell & Rodewald) | This replication                    |
|---------------------|-------------------------------|-------------------------------------|
| N                   | 1 998                          | 200                                 |
| Population          | U.S. adults (Univox panel)     | European adults (online, voluntary) |
| Compensation        | Paid panel                     | Uncompensated                       |
| Age / language      | 18+, English                   | 18+, fluent English                 |
| Exclusion           | Failed attention check; missing data | Failed attention check        |

---

## 5. Measures & Indices

| Index                       | Items                                  | Scale            | α (original) |
|-----------------------------|----------------------------------------|------------------|--------------|
| Positive Affect             | hopeful, cheerful, optimistic          | 1–5 Likert       | 0.853        |
| Negative Affect             | sad, anxious, angry, afraid            | 1–5 Likert       | 0.887        |
| Pro-conservation behavior   | 7 Simple Actions items                 | 1–5 Likert       | 0.854        |
| Political engagement        | 7 items                                | 1–5 Likert       | 0.944        |
| Willingness to Donate (WTD) | 3 items (birds / wildlife / biodiv.)   | Binary (yes/no)  | 0.843        |

Each index = **mean of its items**. Emotion items adapted from McComas, Stedman & Sol Hart (2011, Cornell Climate Action Policy Survey).

**Attention check (experimental conditions only):** *"Was the story you just read about: Insects / Birds / Fish?"*

**Behavioral tracking (extension):** Final debriefing page contains a real donation hyperlink; the survey platform logs click vs. no-click.

---

## 6. Analysis Plan

1. Linear regressions: condition → each DV, with demographic covariates (backward stepwise selection).
2. Pairwise condition contrasts with **Bonferroni** correction.
3. **Mediation:** Hayes PROCESS Model 4, 10 000 bootstraps, 95 % bias-corrected CIs (positive & negative affect as mediators).
4. **Click-through:** χ² and logistic regression across conditions; relate self-reported WTD to actual click.
5. α = 0.05; effect sizes as standardized β (linear) and odds ratios (logistic).
6. Software: SPSS v28 (planned); reproducible Python alternatives in `*.ipynb`.

---

## 7. Repository Layout

```
.
├── README.md                                  ← this file
├── animals_sth_sth_group_report.pdf           ← group report (replication protocol)
├── Promoting Conservation Behaviors ... .pdf  ← original Martell & Rodewald (2024)
├── survey_backbone.md                         ← survey spec (EPFL replication framing)
├── survey_backbone_2.md                       ← survey spec (general replication framing)
├── make_survey_docx.py                        ← builds survey.docx from backbone
├── survey.docx                                ← printable survey
├── img_pessimistic_2.9billion_gone.jpg        ← stimulus (loss frame)
├── img_optimistic_raptors_gained.jpg          ← stimulus (gain frame)
├── img_seven_simple_actions.jpg               ← Cornell 7-actions banner
├── synthetic_personality_bot.py               ← simulated-respondent generator
├── data_analysis.ipynb / data_analysis_2.ipynb / SHS_analysis.ipynb
│                                              ← analysis notebooks (Plotly + statsmodels + sklearn)
├── environment.yml                            ← conda env (`shs-analysis`)
└── Experimental_Cognitive_Pyschology/         ← submodule with raw data (data/SHS_raw.csv)
```

---

## 8. Reproducing the Analysis

```bash
# 1. Create the env
conda env create -f environment.yml
conda activate shs-analysis

# 2. Place SHS_raw.csv next to the notebook (or update the path)
#    Then launch:
jupyter lab SHS_analysis.ipynb
```

Outputs are written to `SHS_outputs/` (cleaned CSV, hypothesis-results JSON, interactive HTML plots, and PNG exports if `kaleido` is available).

---

## 9. Stimuli & Materials

- **Pessimistic infographic:** [3billionbirds.org — "2.9 billion birds gone"](https://www.3billionbirds.org/share-on-social)
- **Optimistic infographic:** [3billionbirds.org — "+15 million raptors gained"](https://www.3billionbirds.org/share-on-social)
- **Seven Simple Actions banner:** [Cornell Lab of Ornithology](https://www.birds.cornell.edu/home/seven-simple-actions-to-help-birds/)
- Verbatim message texts for all three frames are in [survey_backbone.md](survey_backbone.md#section-2-the-survey) §2.3.

---

## 10. Transparency & Open Science

- **Not pre-registered.** Confirmatory vs. exploratory tests are flagged in the analysis notebooks.
- Materials, message wording, and measurement scales are documented in this repo.
- Anonymized data and analysis code will be shared with the instructor and on request, subject to participant-privacy and IRB constraints.
- Original study (Martell & Rodewald, 2024) was approved under Cornell IRB protocol #1912009295; this replication follows EPFL ethics requirements.

---

## 11. Key References

- Martell, J. E. M., & Rodewald, A. D. (2024). Promoting conservation behaviors by leveraging optimistic and pessimistic messages and emotions. *Society & Natural Resources*, 37(4), 564–585.
- Rosenberg, K. V., et al. (2019). Decline of the North American avifauna. *Science*, 366(6461), 120–124.
- McComas, K. A., Stedman, R., & Sol Hart, P. (2011). Community support for campus approaches to sustainable energy use. *Energy Policy*, 39(5), 2310–2318.
- Kahneman, D., & Tversky, A. (1979). Prospect theory. *Econometrica*, 47(2), 263–291.
- Lerner, J. S., Li, Y., Valdesolo, P., & Kassam, K. S. (2015). Emotion and decision making. *Annual Review of Psychology*, 66, 799–823.
- van der Linden, S. (2015). The social-psychological determinants of climate change risk perceptions. *Journal of Environmental Psychology*, 41, 112–124.
- Hornsey, M. J., Harris, E. A., Bain, P. G., & Fielding, K. S. (2016). Meta-analyses of the determinants and outcomes of belief in climate change. *Nature Climate Change*, 6(6), 622–626.
- Cacciatore, M. A., Scheufele, D. A., & Iyengar, S. (2016). The end of framing as we know it. *Mass Communication and Society*, 19(1), 7–23.
- Entman, R. M. (1993). Framing: Toward clarification of a fractured paradigm. *Journal of Communication*, 43(4), 51–58.
- Hayes, A. F., & Preacher, K. J. (2013). Conditional process modeling.
