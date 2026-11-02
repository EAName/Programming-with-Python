# Programming-with-Python

Northwestern University, M.S. in Data Science (Data Engineering specialization) · MSDS 430: Python for Data Analysis

## Overview

This is the programming foundation course of the degree. Most notebooks are guided module exercises, but three pieces go past the basics and are the reason to look here: a multi-source data integration and profiling project on police use-of-force records, a cleaning pass over a 342,381-row beer review dataset, and text processing with sentiment scoring over semi-structured hotel review JSON. Together they cover the everyday data engineering work of reading messy files in several formats, reconciling schemas, and checking data quality before any analysis.

## Highlights / Results

All values come from saved notebook outputs.

| Work | Method | Result | Source |
|------|--------|--------|--------|
| Police use-of-force data integration | Ingest CSV (Fatal Encounters), XLSX (Mapping Police Violence), CSV (FBI National Use-of-Force), and a PDF table (via `tabula`); drop unused columns; rename MPV columns to the Fatal Encounters schema; append | Fatal Encounters: 30,027 rows x 44 columns reduced to 10 columns and 11,073 complete rows. MPV: 9,087 rows x 35 columns reduced to 3,910 complete rows. Combined dataset: 14,983 rows | `Crowd_Sourcing_Police_Violence.ipynb` |
| Profiling the combined dataset | `describe()` on categorical fields, bar charts, strip plots | 51 state values, 3,790 distinct agency strings, 29 force categories; "Gunshot" in 11,796 of 14,983 rows; "Not fleeing" 9,445; "Armed" 7,011 | `Crowd_Sourcing_Police_Violence.ipynb` |
| Beer review data cleaning | Null audit, fixing missing brewery names by lookup, dropping unresolvable rows, dropping null ABV | 342,381 reviews in; 3 rows dropped and 1 brewery name repaired; 10,824 rows with null ABV removed, leaving 331,554 reviews of 23,476 beers across 104 styles | `Data_Wrangling.ipynb` |
| Beer review exploration | Date filtering, ABV outlier trimming, correlation heatmaps, groupby aggregates by brewery/style/beer | 2011-only subset: 322,211 rows; trimming ABV to [1, 11.5] leaves 316,403 rows (max ABV before trimming was 57.7) | `Data_Visualization.ipynb` |
| Hotel review text processing | Parse nested JSON, token counts with `Counter`, regex, NLTK stopwords and bigrams, TextBlob polarity | 233 reviews parsed; "bathroom" appears 29 times; 128 reviews run longer than 100 words | `Program_Structure_Text_Processing_File_IO_Sentiment_Analysis.ipynb` |

The sentiment cells (TextBlob polarity by keyword, polarity vs. star rating box plots) are written but have no saved outputs, so no polarity numbers are quoted here.

## Engineering decisions and tradeoffs

- **Harmonize to one schema, then append.** In the police data project, the MPV columns are renamed to match the Fatal Encounters column names, columns that exist in only one source are dropped, and the two frames are stacked. This trades breadth (fields like mental illness flags and encounter type are lost) for a single comparable table.
- **Reported vs. crowd-sourced counts kept separate.** The FBI National Use-of-Force file is a 3-row monthly participation summary (no response, zero reports, incident reports), so it is used as context on reporting coverage rather than merged with the incident-level crowd-sourced data.
- **PDF as a data source.** A summary table from a Fatal Encounters PDF is pulled with `tabula.read_pdf` and given explicit year headers, rather than retyped by hand.
- **Repair before drop.** In `Data_Wrangling.ipynb`, rows with a missing brewery name are investigated one at a time: one is repaired by matching other reviews of the same beer, and only the ones that cannot be resolved are dropped. Null ABV rows (10,824) are dropped in bulk because there is no reliable fill source.
- **Intermediate files between phases.** Cleaned outputs are written to CSV (`beer_reviews_final.csv`, `fatal_encounters_final.csv`, `mpv_final.csv`, `eda_dataset.csv`) and re-read by the next notebook or phase, so each stage can be rerun on its own.
- **Error handling as a pattern, not an afterthought.** `Functions_Handling_Errors_Data_Structures.ipynb` works through specific vs. generic `except` clauses, `raise`, `logging.exception` for tracebacks, and a SQLite create/insert wrapped in `try`/`finally` so the connection is always closed.

## Limitations and what I would do differently today

- `dropna()` across all retained columns removed about 63% of Fatal Encounters rows (30,027 to 11,073) and 57% of MPV rows. Today I would profile missingness per column and decide per field, and I would deduplicate across sources (MPV carries a Fatal Encounters ID that could be used as a join key) instead of appending and risking double-counted incidents.
- The backward difference encoding plus correlation heatmaps on state and agency dummies do not measure anything meaningful about the incidents. A better next step would be per-capita rates by state joined to census population.
- Notebooks run top to bottom against files in the working directory, with no parameters, tests, or data versioning. I would move the ingestion and cleaning steps into functions with pytest checks on row counts and schema, and run them as a small pipeline (for example Airflow or Prefect writing Parquet).
- The code uses `DataFrame.append`, which pandas 2.0 removed, and several cells rely on interactive state.
- Saved outputs include 13 error tracebacks. Most are deliberate (the error-handling module triggers each exception type on purpose), and a few are leftover exercise errors. They are kept as they were from the original run.

## Repository map

Substantive analyses first, then the course modules in order.

**Applied analyses**

| Notebook | Topic |
|----------|-------|
| `Crowd_Sourcing_Police_Violence.ipynb` | Final EDA project: proposal, data dictionary, cleaning (Phase 1), integration and profiling (Phase 2) across Fatal Encounters, Mapping Police Violence, and FBI National Use-of-Force data; PDF table extraction with `tabula`; category encoding and correlation heatmaps |
| `Program_Structure_Text_Processing_File_IO_Sentiment_Analysis.ipynb` | Nested dict/JSON parsing of TripAdvisor hotel reviews; `Counter`, regex, NLTK stopwords and bigrams; TextBlob sentence polarity vs. star ratings |
| `Data_Wrangling.ipynb` | Open beer database and beer review CSVs: typing, null audit, repairing and dropping bad rows, timestamp conversion, groupby/merge for brewery and style market research |
| `Data_Visualization.ipynb` | Continues the beer data: date filtering, ABV outlier trimming, seasonal features, correlation heatmaps, groupby aggregates, seaborn/matplotlib/plotly charts |

**Course modules**

| Notebook | Topic |
|----------|-------|
| `Python.ipynb` | Jupyter workflow, comments, syntax/runtime/semantic errors, variables, formatted output |
| `Data_Types_Conditionals_Operators.ipynb` | `int`, `float`, `str`, `bool`; operators; `input()`; string methods; `if`/`elif`/`else` and nested conditionals |
| `Loops_Functions.ipynb` | `for`/`while` loops; built-in and user-defined functions; `math` module |
| `File_IO_and_Modules.ipynb` | Text file read/write; pandas `read_csv`/`read_excel`/`read_json`; Excel export with `xlsxwriter`; standard library and third-party imports |
| `Functions_Handling_Errors_Data_Structures.ipynb` | `try`/`except`/`else`/`finally`; specific vs. generic exceptions; `raise`; logging with tracebacks; SQLite connection with guarded cleanup |
| `Classes_and_Objects.ipynb` | Classes, instance and class attributes, methods, class methods (Circle, State, PizzaOrder, CreditCard examples) |

## Reproducing

- Notebooks were last run on Python 3.8 (per notebook metadata).
- Install dependencies: `pip install -r requirements.txt`, then `jupyter notebook`. The police data notebook uses `DataFrame.append`, so install `pandas<2` if you want it to run unchanged. `tabula-py` needs a Java runtime.
- Data files are not bundled. Place them next to the notebooks:

| File | Used by | Source |
|------|---------|--------|
| `fatal_encounters.csv`, `Fatal Encounters Lethal Use of Force.pdf` | `Crowd_Sourcing_Police_Violence.ipynb` | Fatal Encounters (fatalencounters.org) |
| `MPVDatasetDownload.xlsx` | `Crowd_Sourcing_Police_Violence.ipynb` | Mapping Police Violence (mappingpoliceviolence.org) |
| `National_Use_of_Force.csv` | `Crowd_Sourcing_Police_Violence.ipynb` | FBI Crime Data Explorer, National Use-of-Force Data Collection |
| `open-beer-database.csv`, `beer_reviews.csv` | `Data_Wrangling.ipynb` (writes `beer_reviews_final.csv` for `Data_Visualization.ipynb`) | Course-provided |
| `hotel_reviews.json` | `Program_Structure_Text_Processing_File_IO_Sentiment_Analysis.ipynb` | Course-provided TripAdvisor extract |
| `cars.csv`, `cars.xlsx`, `cars.json`, `cars.txt` | `File_IO_and_Modules.ipynb` | Course-provided |

- The NLTK cells download `stopwords` and `punkt` on first run.
- No credentials are needed. The string `password = 'GQskUXr'` in `Data_Types_Conditionals_Operators.ipynb` is input for a string-manipulation exercise, not a credential.
- Saved outputs reflect the original course run and were not regenerated.

## Stack

| Layer | Tools |
|-------|-------|
| Language | Python 3 |
| Environment | Jupyter Notebook |
| Data | pandas, NumPy |
| Visualization | matplotlib, seaborn, plotly |
| Text / NLP | NLTK, TextBlob, `re`, `collections.Counter` |
| Encoding | category_encoders |
| File formats | CSV, JSON, TXT, XLSX (`xlsxwriter`), SQLite |
| PDF parsing | tabula-py |
| Standard library | `math`, `json`, `logging`, `sqlite3`, `string`, `itertools` |
