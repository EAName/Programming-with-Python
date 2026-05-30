# Programming-with-Python

Graduate coursework in Python programming fundamentals, data structures, file I/O, exploratory analysis, text processing, and object-oriented design through sequential Jupyter modules (MSDS 430).

---

## 1. Title and Summary

**Programming with Python**  
Northwestern University M.S. in Data Science (Data Engineering specialization): build Python fluency from syntax and control flow through pandas EDA, multi-format file handling, NLP-style text analytics, OOP, and a multi-phase capstone analyzing crowd-sourced police use-of-force datasets.

---

## 2. Concepts and Methods

- **Python basics and tooling:** Jupyter workflow, comments, syntax/runtime/semantic errors, variables, formatted output (`Python.ipynb`)
- **Types, operators, and conditionals:** `int`, `float`, `str`, `bool`; mathematical, relational, and logical operators; `input()`; string methods; `if`/`elif`/`else` and nested conditionals (`Data_Types_Conditionals_Operators.ipynb`)
- **Loops and functions:** `for`/`while` loops; built-in and user-defined functions; `math` module (e.g., `gcd`) (`Loops_Functions.ipynb`)
- **File I/O and modules:** read/write text files; pandas `read_csv`; JSON load/dump; Excel export with `xlsxwriter`; standard library and third-party imports (`File_IO_and_Modules.ipynb`)
- **EDA and data wrangling:** ingest open beer database and beer review CSVs; cleaning, typing, filtering, grouping, merges for brewery market research (`Data_Wrangling.ipynb`, continued in `Data_Visualization.ipynb`)
- **Visualization:** seaborn/matplotlib/plotly charts; groupby aggregations by brewery, style, and beer; time-based review patterns (`Data_Visualization.ipynb`)
- **Text processing and sentiment:** nested dict/JSON parsing (TripAdvisor hotel reviews); `Counter`, regex (`re`), NLTK stopwords/bigrams; TextBlob polarity on review sentences (`Program_Structure_Text_Processing_File_IO_Sentiment_Analysis.ipynb`)
- **Error handling:** `try`/`except`/`else`/`finally`; specific vs. generic exceptions; `raise`; logging with tracebacks; SQLite connection demo with guarded resource cleanup (`Functions_Handlind_Errors_Data_Structures.ipynb`)
- **Object-oriented programming:** class definitions, attributes, methods, instances (Circle and related examples) (`Classes_and_Objects.ipynb`)
- **Capstone EDA project:** multi-source police violence datasets (Fatal Encounters, Mapping Police Violence, FBI National Use-of-Force); PDF table extraction (`tabula`); data harmonization, profiling, and demographic analysis notebook (`Crowd_Sourcing_Police_Violence.ipynb`)

**Data dependencies:** beer review CSVs, hotel JSON, police violence CSVs, and FBI extracts referenced in notebooks are not all bundled in the repository [VERIFY which data files are required locally vs. downloaded during assignment]

**Out of scope for this repo:** algorithm analysis coursework (see **Data-Engineering-Algorithms**); inferential statistics in R (see **Statistics**); cloud-scale ingestion (see **Systems-Engineering**, **Data-Miners**); production web services (see **Analytics-Applications-Engineering**).

---

## 3. Stack

| Layer | Tools |
|-------|-------|
| Language | Python 3 |
| Environment | Jupyter Notebook |
| Data | pandas, NumPy |
| Visualization | matplotlib, seaborn, plotly |
| Text / NLP | NLTK, TextBlob, `re`, `collections.Counter` |
| File formats | CSV, JSON, TXT, XLSX (`xlsxwriter`), SQLite |
| PDF parsing | tabula (capstone) |
| Standard library | `math`, `json`, `logging`, `sqlite3`, `string`, `itertools` |

---

## 4. Structure

```
Programming-with-Python/
├── Python.ipynb
├── Data_Types_Conditionals_Operators.ipynb
├── Loops_Functions.ipynb
├── File_IO_and_Modules.ipynb
├── Data_Wrangling.ipynb
├── Data_Visualization.ipynb
├── Program_Structure_Text_Processing_File_IO_Sentiment_Analysis.ipynb
├── Functions_Handlind_Errors_Data_Structures.ipynb
├── Classes_and_Objects.ipynb
├── Crowd_Sourcing_Police_Violence.ipynb
└── README.md
```

- **Organization:** ten module notebooks progressing from language basics to applied EDA and capstone; beer-review thread spans Modules 5–6; police-violence project is standalone
- **Reusable modules:** none packaged; helper functions and OOP classes defined inline per assignment
- **Engineering practice:** defensive input handling, structured exception paths, multi-format ingestion, reproducible EDA with saved intermediate datasets, sentiment and token-frequency analysis over semi-structured JSON

---

**Course context:** Northwestern University, M.S. in Data Science, Data Engineering specialization (MSDS 430)  
**Repository:** https://github.com/EAName/Programming-with-Python
