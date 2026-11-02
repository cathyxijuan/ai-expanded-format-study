# Cleaned Dataset — README

## Overview

This dataset is from the study "AI Item Generation for the Big Five Personality Traits", which evaluates how large language models generate psychological items in the Expanded format. Participants were randomly assigned to one of four conditions measuring the Big Five Inventory (BFI). All other measures were administered to all participants, although not every participant answered every item (see [Data quality](#data-quality)).

## Files

| File | Description |
|------|-------------|
| `cleaned data.csv` | Cleaned dataset (1,212 participants × 216 variables). One row per participant. |
| `survey.pdf` | Full survey, including the condition-specific BFI response options. |

The raw survey exports, the cleaning script, and participants' written responses are not included in this repository.

## Cleaning steps

The cleaned data were produced from the raw survey export (1,301 rows × 398 columns) as follows:

1. **Drop empty rows.** Rows where every column is missing are removed (81 rows).
2. **Drop the stray column** `X0...Q9`.
3. **Name the BFI blocks.** The first 240 columns are the four condition-specific BFI blocks (60 items each), renamed `BFI_Instant_ZS1–60`, `BFI_Instant_FS1–60`, `BFI_Think_ZS1–60`, `BFI_Think_FS1–60`.
4. **Collapse the BFI blocks.** The data are pivoted long and back to wide (regex `BFI_(.+?)(\d+)$` splits each name into condition and item number), keeping only non-missing responses. This yields the 60 shared columns `BFI_1`–`BFI_60` plus a `condition` column.
5. **Rename the remaining columns by position** to the variable names listed in the codebook below.
6. **Write** `cleaned data.csv`.

### Notes and caveats

- **Participants with no BFI responses are dropped.** Step 4 removes any participant who answered no BFI item in any condition (8 non-empty rows), even if they completed the other measures. 1,301 raw rows − 81 empty − 8 without BFI = 1,212 cleaned rows.
- **No scoring, reverse-keying, or exclusions** (e.g., attention checks, completion time) are applied; values are as exported.
- Each participant answered exactly one BFI condition (no overlapping blocks).
- **Written responses are excluded.** The two open-ended variables were removed from this public version of the dataset.

### Condition counts

| Condition | n |
|-----------|---|
| `Instant_ZS` | 310 |
| `Instant_FS` | 311 |
| `Think_ZS` | 298 |
| `Think_FS` | 293 |
| **Total** | **1,212** |

### Data quality

The data have not been screened, so analysts should apply their own exclusion rules.

**Missing data.** Skipped numeric items are coded `NA`; empty text fields are blank. 598 of the 1,212 participants answered every item, excluding the conditional `_specify` fields. Participants with at least one missing item, by measure:

| Measure | Participants with missing items |
|---------|---------------------------------|
| BFI (`BFI_1`–`BFI_60`) | 73 |
| SD3 (`dark_`) | 239 |
| Psychopathology (`psychopathology_`) | 112 |
| NPAS (`nerd_`) | 178 |
| NIS (`nonverbal_`) | 213 |
| Mini-Markers (`BFI_mini_`) | 279 |

**Implausible values.** Open-ended numeric items were not range-checked and include the following:

| Variable | Values to review |
|----------|------------------|
| `age` | 14 values below 18; maximum values of 89 and 99 |
| `BRF_GPA` | 1 negative value; 4 values above 100 (maximum 6,687,873) |
| `BRF_dating_partners` | 12 negative values; maximum 1,000 |
| `BRF_party_count` | 1 negative value; maximum 4,567 |
| `volunteerism` | 6 negative values; 1 value above 365 (10,005 days per year) |
| `cultural_event` | 2 negative values; maximum 1,111,221 |

**Free-text fields.** `home_language` and `country_of_birth` are as typed by participants, with inconsistent spelling and capitalization (e.g., `English` and `english`). 69 `home_language` entries and 1 `country_of_birth` entry are numbers only. Values in the `_specify` columns begin with the selected option in square brackets (e.g., `[No] 3`).

---

## Experimental Design

Each participant completed the BFI under one of four conditions:

| Condition | Description |
|-----------|-------------|
| `Instant_ZS` | Instant Zero-Shot |
| `Instant_FS` | Instant Few-Shot |
| `Think_ZS` | Think Zero-Shot |
| `Think_FS` | Think Few-Shot |

The `condition` variable identifies which condition each participant was assigned to. The 240 condition-specific BFI columns in the raw data were collapsed into 60 shared columns (`BFI_1`–`BFI_60`) with rows containing only the participant's actual responses.

---

## Variable Codebook

### Big Five Inventory (BFI) — Experimental Measure
| Variable | Description |
|----------|-------------|
| `BFI_1`–`BFI_60` | BFI items (60 items), condition-collapsed |
| `condition` | Experimental condition (`Instant_ZS`, `Instant_FS`, `Think_ZS`, `Think_FS`) |

**Response scale.** The BFI was administered in the *Expanded format*: each item presents five item-specific response statements (rather than a single shared Likert anchor set such as "Strongly Disagree – Strongly Agree"). The exact wording of the five options is unique to each item and varies by condition (`Instant_ZS`, `Instant_FS`, `Think_ZS`, `Think_FS`). The numeric values `1`–`5` in the dataset correspond to the five response options as they were presented to the participant, in the order they appeared in `survey.pdf` (where `1` = the first option shown and `5` = the fifth option shown). Please consult `survey.pdf` for the full set of response option statements for each item under each condition.

### Short Dark Triad (SD3)

**Response scale.** Participants rated each item on a 5-point agreement scale:

| Value | Label |
|-------|-------|
| 1 | Disagree strongly |
| 2 | Disagree |
| 3 | Neither agree nor disagree |
| 4 | Agree |
| 5 | Agree strongly |

| Variable | Item |
|----------|------|
| `dark_1` | It's not wise to tell your secrets. |
| `dark_2` | I like to use clever manipulation to get my way. |
| `dark_3` | Whatever it takes, you must get the important people on your side. |
| `dark_4` | Avoid direct conflict with others because they may be useful in the future. |
| `dark_5` | It's wise to keep track of information that you can use against people later. |
| `dark_6` | You should wait for the right time to get back at people. |
| `dark_7` | There are things you should hide from other people to preserve your reputation. |
| `dark_8` | Make sure your plans benefit yourself, not others. |
| `dark_9` | Most people can be manipulated. |
| `dark_10` | People see me as a natural leader. |
| `dark_11` | I hate being the center of attention. |
| `dark_12` | Many group activities tend to be dull without me. |
| `dark_13` | I know that I am special because everyone keeps telling me so. |
| `dark_14` | I like to get acquainted with important people. |
| `dark_15` | I feel embarrassed if someone compliments me. |
| `dark_16` | I have been compared to famous people. |
| `dark_17` | I am an average person. |
| `dark_18` | I insist on getting the respect I deserve. |
| `dark_19` | I like to get revenge on authorities. |
| `dark_20` | I avoid dangerous situations. |
| `dark_21` | Payback needs to be quick and nasty. |
| `dark_22` | People often say I'm out of control. |
| `dark_23` | It's true that I can be mean to others. |
| `dark_24` | People who mess with me always regret it. |
| `dark_25` | I have never gotten into trouble with the law. |
| `dark_26` | I enjoy having sex with people I hardly know. |
| `dark_27` | I'll say anything to get what I want. |

### Brief Report of Functioning (BRF)

| Variable | Item | Scale |
|----------|------|-------|
| `BRF_popularity` | In relation to people you know, how would you describe your popularity among your peers? | 9-point scale (see labels below) |
| `BRF_status` | How much status (prominence, respect, and influence) do you have within your social group? | 7-point scale (see labels below) |
| `BRF_dating_partners` | How many different people did you date over the course of the past year? | Numeric (open-ended count) |
| `BRF_GPA` | Indicate your grade point average (GPA) for the last term in percentage. (First-year students without a last-term GPA were asked to report their Grade 12 average.) | Numeric (%) |
| `BRF_party_count` | Estimate the average number of parties per month that you attend. | Numeric (per month) |

**`BRF_popularity` response options:**

| Value | Label |
|-------|-------|
| 1 | Extremely Unpopular |
| 2 | Very Unpopular |
| 3 | Moderately Unpopular |
| 4 | Somewhat Unpopular |
| 5 | Average |
| 6 | Somewhat Popular |
| 7 | Moderately Popular |
| 8 | Very Popular |
| 9 | Extremely Popular |

**`BRF_status` response options:** A 7-point scale anchored by `1 = Low Status` and `7 = High Status`; values 2–6 were presented as unlabeled numeric points between the two anchors.

### Psychopathology Scale

**Item stem.** "In your life, has a medical professional ever diagnosed you with…"

**Response scale.** Yes / No:

| Value | Label |
|-------|-------|
| 1 | Yes |
| 2 | No |

| Variable | Item |
|----------|------|
| `psychopathology_1` | Alcohol abuse or dependence |
| `psychopathology_2` | Nicotine dependence |
| `psychopathology_3` | Drug abuse or dependence |
| `psychopathology_4` | Generalized anxiety disorder |
| `psychopathology_5` | Panic disorder |
| `psychopathology_6` | Agoraphobia without history of panic disorder |
| `psychopathology_7` | Specific phobia |
| `psychopathology_8` | Social phobia |
| `psychopathology_9` | Posttraumatic stress disorder |
| `psychopathology_10` | Major depressive disorder |

### Volunteerism and Cultural Event

Both items use open-ended numeric entry (no Likert scale); responses are raw counts as entered by the participant.

| Variable | Description | Scale |
|----------|-------------|-------|
| `volunteerism` | Estimate the average number of days you normally volunteer per year. | Numeric (days per year) |
| `cultural_event` | During the past 12 months, how many times in total did you attend any of the following types of performances or events? (theatre, cabaret, classical music, opera, popular music, dance/house party, ballet) | Numeric (count) |

### Nerdy Personality Attributes Scale (NPAS)

**Response scale.** Participants rated each item on a 5-point agreement scale:

| Value | Label |
|-------|-------|
| 1 | Strongly Disagree |
| 2 | Somewhat Disagree |
| 3 | Neutral |
| 4 | Somewhat Agree |
| 5 | Strongly Agree |

| Variable | Item |
|----------|------|
| `nerd_1` | I get excited about my ideas and research. |
| `nerd_2` | I prefer academic success to social success. |
| `nerd_3` | I can be socially awkward at times. |
| `nerd_4` | I am interested in science. |
| `nerd_5` | I gravitate towards introspection. |
| `nerd_6` | I was a very odd child. |
| `nerd_7` | I would describe my smarts as bookish. |
| `nerd_8` | I sometimes prefer fictional people to real ones. |
| `nerd_9` | I am more comfortable interacting online than in person. |
| `nerd_10` | I like to read technology news reports. |
| `nerd_11` | I was in advanced classes. |
| `nerd_12` | I have started writing a novel. |
| `nerd_13` | I love to read challenging material. |
| `nerd_14` | I spend recreational time researching topics others might find dry or overly rigorous. |
| `nerd_15` | I am a strange person. |
| `nerd_16` | I like to play RPGs (e.g., D&D). |
| `nerd_17` | I enjoy learning more than I need to. |
| `nerd_18` | I collect books. |
| `nerd_19` | I watch science related shows. |
| `nerd_20` | I like science fiction. |
| `nerd_21` | I spend more time at the library than any other public place. |
| `nerd_22` | I care about super heroes. |
| `nerd_23` | My appearance is not as important as my intelligence. |
| `nerd_24` | I have played a lot of video games. |
| `nerd_25` | I am more comfortable with my hobbies than I am with other people. |
| `nerd_26` | I would rather read a book than go to a party. |

### Nonverbal Immediacy Scale (NIS)

**Response scale.** Participants rated how often each behaviour applied to them on a 5-point frequency scale:

| Value | Label |
|-------|-------|
| 1 | Never |
| 2 | Rarely |
| 3 | Occasionally |
| 4 | Often |
| 5 | Very Often |

| Variable | Item |
|----------|------|
| `nonverbal_1` | I use my hands and arms to gesture while talking to people. |
| `nonverbal_2` | I touch others on the shoulder or arm while talking to them. |
| `nonverbal_3` | I use a monotone or dull voice while talking to people. |
| `nonverbal_4` | I look over or away from others while talking to them. |
| `nonverbal_5` | I move away from others when they touch me while we are talking. |
| `nonverbal_6` | I have a relaxed body position when I talk to people. |
| `nonverbal_7` | I frown while talking to people. |
| `nonverbal_8` | I avoid eye contact while talking to people. |
| `nonverbal_9` | I have a tense body position while talking to people. |
| `nonverbal_10` | I sit close or stand close to people while talking with them. |
| `nonverbal_11` | My voice is monotonous or dull when I talk to people. |
| `nonverbal_12` | I use a variety of vocal expressions when I talk to people. |
| `nonverbal_13` | I gesture when I talk to people. |
| `nonverbal_14` | I am animated when I talk to people. |
| `nonverbal_15` | I have a bland facial expression when I talk to people. |
| `nonverbal_16` | I move closer to people when I talk to them. |
| `nonverbal_17` | I look directly at people while talking to them. |
| `nonverbal_18` | I am stiff when I talk to people. |
| `nonverbal_19` | I have a lot of vocal variety when I talk to people. |
| `nonverbal_20` | I avoid gesturing while I am talking to people. |
| `nonverbal_21` | I lean toward people when I talk to them. |
| `nonverbal_22` | I maintain eye contact with people when I talk to them. |
| `nonverbal_23` | I try not to sit or stand close to people when I talk with them. |
| `nonverbal_24` | I lean away from people when I talk to them. |
| `nonverbal_25` | I smile when I talk to people. |
| `nonverbal_26` | I avoid touching people when I talk to them. |

### Big Five Mini-Markers

**Response scale.** For each adjective, participants rated how accurately it described them on a 9-point accuracy scale:

| Value | Label |
|-------|-------|
| 1 | Extremely Inaccurate |
| 2 | Very Inaccurate |
| 3 | Moderately Inaccurate |
| 4 | Slightly Inaccurate |
| 5 | Neutral |
| 6 | Slightly Accurate |
| 7 | Moderately Accurate |
| 8 | Very Accurate |
| 9 | Extremely Accurate |

| Variable | Item | Variable | Item |
|----------|------|----------|------|
| `BFI_mini_1` | Bashful | `BFI_mini_21` | Moody |
| `BFI_mini_2` | Bold | `BFI_mini_22` | Organized |
| `BFI_mini_3` | Careless | `BFI_mini_23` | Philosophical |
| `BFI_mini_4` | Cold | `BFI_mini_24` | Practical |
| `BFI_mini_5` | Complex | `BFI_mini_25` | Quiet |
| `BFI_mini_6` | Cooperative | `BFI_mini_26` | Relaxed |
| `BFI_mini_7` | Creative | `BFI_mini_27` | Rude |
| `BFI_mini_8` | Deep | `BFI_mini_28` | Shy |
| `BFI_mini_9` | Disorganized | `BFI_mini_29` | Sloppy |
| `BFI_mini_10` | Efficient | `BFI_mini_30` | Sympathetic |
| `BFI_mini_11` | Energetic | `BFI_mini_31` | Systematic |
| `BFI_mini_12` | Envious | `BFI_mini_32` | Talkative |
| `BFI_mini_13` | Extraverted | `BFI_mini_33` | Temperamental |
| `BFI_mini_14` | Fretful | `BFI_mini_34` | Touchy |
| `BFI_mini_15` | Harsh | `BFI_mini_35` | Uncreative |
| `BFI_mini_16` | Imaginative | `BFI_mini_36` | Unenvious |
| `BFI_mini_17` | Inefficient | `BFI_mini_37` | Unintellectual |
| `BFI_mini_18` | Intellectual | `BFI_mini_38` | Unsympathetic |
| `BFI_mini_19` | Jealous | `BFI_mini_39` | Warm |
| `BFI_mini_20` | Kind | `BFI_mini_40` | Withdrawn |

### Demographics

**`age`** — Age in years (numeric)

---

**`sex`** — Sex at birth
| Value | Label |
|-------|-------|
| 1 | Male |
| 2 | Female |
| 3 | Intersex |
| 4 | Prefer not to say |

---

**`gender_identity`** — Gender identity  
**`gender_identity_specify`** — Free-text if "Another gender identity" selected
| Value | Label |
|-------|-------|
| 1 | Man |
| 2 | Woman |
| 3 | Non-Binary |
| 4 | Gender queer |
| 5 | Two-Spirit |
| 6 | Transgender man |
| 7 | Transgender woman |
| 8 | Agender |
| 9 | Another gender identity (please specify) |
| 10 | Prefer not to say |

---

**`sexual_orientation`** — Sexual orientation  
**`sexual_orientation_specify`** — Free-text if "Another sexual orientation" selected
| Value | Label |
|-------|-------|
| 1 | Heterosexual (Straight) |
| 2 | Gay |
| 3 | Lesbian |
| 4 | Bisexual |
| 5 | Pansexual |
| 6 | Asexual |
| 7 | Queer |
| 8 | Another sexual orientation (please specify) |
| 9 | Prefer not to say |

---

**`ethnicity`** — Ethnic background  
**`ethnicity_specify`** — Free-text if "Prefer to self-describe" selected
| Value | Label |
|-------|-------|
| 1 | Indigenous (First Nations, Métis, Inuit) |
| 2 | European descent (e.g., British, French, German, Italian) |
| 3 | African descent (e.g., Nigerian, Ethiopian, Ghanaian, or Caribbean heritage) |
| 4 | East Asian (e.g., Chinese, Korean, Japanese) |
| 5 | South Asian (e.g., Indian, Pakistani, Bangladeshi, Sri Lankan) |
| 6 | Southeast Asian (e.g., Filipino, Vietnamese, Thai, Cambodian) |
| 7 | Middle Eastern / West Asian (e.g., Iranian, Lebanese, Turkish, Syrian) |
| 8 | Latin American (e.g., Mexican, Brazilian, Colombian, Chilean) |
| 9 | Mixed / Multiracial (multiple ethnic backgrounds) |
| 10 | Prefer to self-describe |
| 11 | Prefer not to say |

---

**`born_in_canada`** — Born in Canada  
**`born_in_canada_specify`** — If "No" selected: "How long have you lived in Canada? (Years)". Free text, so most entries are numbers but some are not (e.g., `8 months`, `over 10 years`, `Afghanistan`)
| Value | Label |
|-------|-------|
| 1 | Yes |
| 2 | No |

---

**`home_language`** — Language(s) spoken at home or most comfortable using (free text)

**`country_of_birth`** — Country of birth (free text)

---

**`religious_affiliation`** — Religious or spiritual affiliation  
**`religious_affiliation_specify`** — Free-text if "Other" selected
| Value | Label |
|-------|-------|
| 1 | No religious or spiritual affiliation |
| 2 | Agnostic |
| 3 | Atheist |
| 4 | Buddhist |
| 5 | Christian – Catholic |
| 6 | Christian – Protestant (e.g., Anglican, Baptist, United Church) |
| 7 | Christian – Orthodox (e.g., Greek Orthodox, Eastern Orthodox) |
| 8 | Hindu |
| 9 | Jewish |
| 10 | Muslim |
| 11 | Sikh |
| 12 | Spiritual but not religious |
| 13 | Indigenous spiritual traditions |
| 14 | Other (please specify) |
| 15 | Prefer not to say |

---

**`area_of_study`** — Primary area of study  
**`area_of_study_specify`** — Free-text if "Other" selected
| Value | Label |
|-------|-------|
| 1 | Arts / Humanities |
| 2 | Social Sciences |
| 3 | Natural Sciences |
| 4 | Engineering |
| 5 | Business |
| 6 | Health Sciences |
| 7 | Education |
| 8 | Law |
| 9 | Other (please specify) |
| 10 | Prefer not to say |

---

**`relationship_status`** — Current relationship status
| Value | Label |
|-------|-------|
| 1 | Single |
| 2 | In a relationship |
| 3 | Married/Common-law |
| 4 | Separated/Divorced |
| 5 | Widowed |
| 6 | Prefer not to say |

---

**`political_orientation`** — Political orientation
| Value | Label |
|-------|-------|
| 1 | Left |
| 2 | Moderate Left |
| 3 | Center |
| 4 | Moderate Right |
| 5 | Right |
| 6 | Prefer not to say |

---

**`subjective_social_status`** — MacArthur ladder scale: self-rated social standing (1 = lowest, 10 = highest)

### Written Responses

The survey also included two open-ended items (min. 20 words each): imagined life 10 years from now, and personal core values and qualities. These responses are not included in this dataset.
