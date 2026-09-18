# Data-Analysis-Club

Cleaning and standardising a raw animal shelter outcomes dataset in Excel: parsing, deduplication and formatting so the data can actually be analysed.

## The dataset

[`shelter-outcomes-RAW.xlsx`](shelter-outcomes-RAW.xlsx) is the raw file, exactly as downloaded and before any cleaning was done.

It holds one row per shelter outcome across 12 columns:

| Column | Description |
|---|---|
| `animal_id` | Shelter reference for the animal |
| `date_of_birth` | Date of birth |
| `name` | Name given to the animal |
| `datetime` | Date and time of the outcome |
| `monthyear` | Month and year of the outcome |
| `outcome_type` | Adoption, Transfer, Euthanasia, Return to Owner and so on |
| `outcome_subtype` | Reason or route, for example Foster, Suffering, Partner |
| `animal_type` | Dog, Cat, Bird or Other |
| `sex_upon_outcome` | Sex and sterilisation status combined in one field |
| `age_upon_outcome` | Age written as free text, for example "2 years" |
| `breed` | Breed |
| `color` | Colour |

## Before

![Raw shelter outcomes data before cleaning](raw-data.png)

The raw file cannot be analysed as it stands:

- `sex_upon_outcome` packs two separate facts into one field, for example "Neutered Male".
- `age_upon_outcome` is text rather than a number: "2 years", "5 months", "1 weeks". It cannot be averaged, sorted or grouped.
- `datetime` holds the date and the time together in a single text value.
- Many names carry a leading asterisk (`*Hotch`, `*Violet`, `*Jack Sparrow`) and some rows have no name at all.
- `outcome_type` is inconsistent, with both "Return to Owner" and "Rto-Adopt" appearing.
- Duplicate rows are present. Animal `A925190` appears twice with identical details.
- `monthyear` repeats information already held in `datetime`.

## After

![Cleaned dataset with pivot table summaries](cleaned-data.png)

What was done:

- **Split `sex_upon_outcome` into two columns**, `Sterilized` (YES / NO / Unknown) and `sex` (Male / Female / Unknown), so each column holds one fact.
- **Converted `age_upon_outcome` into a number of years**, so "2 months" becomes 0.167. Age can now be averaged and grouped.
- **Separated the date and time**, keeping `Time_arrived` as a proper time value.
- **Removed duplicate rows** and dropped the columns that added nothing to the analysis (`monthyear`, `name`, `date_of_birth`).
- **Standardised the outcome labels** so each category is counted once instead of being split across spellings.
- **Formatted the result as an Excel Table**, which gives filter dropdowns, banded rows and a range that grows automatically as rows are added.
- **Built pivot tables** to summarise the cleaned data.

## What the cleaned data shows

The cleaned file covers **2,001 outcome records for 1,958 animals**.

**Animals by type**

| Animal type | Count |
|---|---|
| Dog | 961 |
| Cat | 891 |
| Other | 140 |
| Bird | 9 |
| **Total** | **2,001** |

**Outcomes**

| Outcome | Count |
|---|---|
| Adoption | 1,093 |
| Transfer | 496 |
| Return to owner | 185 |
| Euthanasia | 181 |
| Died | 20 |
| Rto_adopt | 15 |
| Disposal | 10 |
| Relocate | 1 |
| **Total** | **2,001** |

**Adoption rate by animal type**

| Animal type | Adopted | Rate |
|---|---|---|
| Dog | 599 | 62% |
| Cat | 470 | 53% |
| Other | 23 | 16% |
| Bird | 1 | 11% |

Adoption is the single most common outcome, accounting for just over half of all records. Dogs are adopted at a noticeably higher rate than cats, and animals outside the two main categories are far less likely to be adopted.

On sterilisation, 1,286 animals were sterilised, 466 were not, and 249 had no status recorded. Sex was recorded as male for 935 and female for 817, with the same 249 unknown. That overlap suggests the missing sex and missing sterilisation status come from the same records, which is worth keeping in mind before drawing conclusions from either field.

## Tools

Microsoft Excel: text splitting, find and replace, remove duplicates, formulas for age conversion, Excel Tables and pivot tables.
