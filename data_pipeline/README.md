# ClaraFold data cleaning and preprocessing pipeline

This repository provides the **data cleaning pipeline** used for preparing training data for ClaraFold — a transformer-based framework for RNA secondary structure prediction, including pseudoknots.

---

## Pipeline overview

The pipeline processes RNA sequence datasets to ensure high-quality, non-redundant, and biologically consistent data for model training.

The cleaning stages include:

1. **Remove N from `.ct` files**  
   Filters out `.ct` files where column 2 contains ambiguous nucleotides (`N`).

2. **Remove N from `.seq` files**  
   Filters out `.seq` files where the actual RNA sequence contains any `N`.

3. **Remove duplicate sequences**  
   Identifies and removes exact duplicates across `.seq` files, retaining one representative per duplicate group.

4. **Subunit Filtering**  
   Detects sub-sequences contained within longer sequences. Subunits, full molecules, and clean unique sequences are separated into independent folders.

---

## Directory Structure
```bash
clarafold-training-pipeline/
├── data
│   ├── ArchiveII_gt.txt
│   ├── ArchiveII_probknot.txt
│   ├── cleaned
│   └── raw                                 # Place your raw datasets here
│       ├── archiveII
│       └── RNAStrAlign
├── data_pipeline                           # Full pipeline code
│   ├── cleaning
│   │   ├── remove_duplicate_sequences.py
│   │   ├── remove_n_in_seq.py
│   │   ├── remove_n.py
│   │   └── subunit_filter.py
│   ├── preprocessing
│   │   ├── build_vocab.py
│   │   ├── encode_labels.py
│   │   ├── other_preprocessing_stage.py
│   │   └── tokenize_sequences.py
│   └── README.md                           # Data cleaning README file
├── notebook
│   └── ClaraFoldTraining.ipynb
├── pipeline.py                             # Master script to run the full cleaning pipeline
└── README.md                               # General README file
```


## Usage

1. **Prepare your input data**
    Place your datasets inside the `data/raw/`. The pipeline expects .seq and .ct files inside these folders.
2. **Install requirements**
   This pipeline uses only the Python standard library — no external dependencies are required for the cleaning stage.
3. **Run the pipeline**
   ```bash
   python pipeline.py -i <input_dir> -o <output_dir>
   ```
   **Example:**
   ```bash
   python pipeline.py -i data/raw/archiveII -o data/cleaned/archiveII
   ```
4. The cleaned data will be generated inside:
   ```bash
   data/cleaned/
   ```
   Each stage will store intermediate results under:
   - `data/cleaned/stage1_remove_n/`
   - `data/cleaned/stage2_remove_n_in_seq/`
   - `data/cleaned/stage3_remove_duplicates/`
   - `data/cleaned/stage4_subunit_filter/`

## Related Repository
> The core ClaraFold inference framework is available here: https://github.com/iglabari/ClaraFold

## Contact

For questions, suggestions, or collaborations:

- [garcialabari@cifasis-conicet.gov.ar](mailto:garcialabari@cifasis-conicet.gov.ar)
- [alejandrofloreslamas@cinvestav.mx](mailto:alejandrofloreslamas@cinvestav.mx)
