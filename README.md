# Surf Equipment Database

This repository turns the data in `surf_equipment.xlsx` into a validated SQLite database, an entity relationship diagram (ERD), enriched board and rig datasets, overview tables, and PNG fact sheets.

## Repository contents

- `surf_equipment.xlsx`: source workbook. Its first 9 sheets are master data tables; its final 2 sheets are transactional tables.
- `surf_equipment.ipynb`: interactive Python notebook that reads the workbook, creates the database, validates keys, generates the ERD, builds overviews and fact sheets, and exports PNG files.
- `surf_equipment.db`: generated SQLite database containing the 11 tables, primary keys, and foreign-key constraints.
- `output/ERD/`: generated ERD files: `surf_equipment_erd.dot`, `surf_equipment_erd.svg`, and `surf_equipment_erd.png` when Graphviz rendering is available.
- `output/boards/`: `boards_overview.png` and one PNG fact sheet per board.
- `output/rigs/`: `rigs_overview.png` and one PNG fact sheet per rig.

The first column of every worksheet becomes that table's primary key. A transactional column whose name matches a table primary key becomes a foreign key. This includes relationships between transactional tables, such as `rig_board.rig_id` referencing `rigs.rig_id`. ERD arrows show `1` at the referenced table and `∞` when multiple child rows may reference it.

## Notebook workflow

The notebook executes these stages:

1. Load the workbook and classify its master and transactional tables.
2. Create and populate SQLite tables with primary and foreign-key constraints.
3. Validate primary keys, row counts, foreign keys, and referential integrity.
4. Generate the ERD in DOT, SVG, and PNG formats when Graphviz is installed.
5. Build `boards_enriched` with compatible rigs from `rig_board`.
6. Build `rigs_enriched` with all related master-table fields and compatible boards from `rig_board`.
7. Display transposed board and rig overviews using the notebook's styled table format.
8. Create individual fact sheets, omitting missing values and duplicate master primary-key fields.
9. Export the overviews and fact sheets as PNG images with Playwright.

The overviews use boards or rigs as columns. The rig overview places each master-table field directly after its corresponding foreign key and includes each foreign key only once. The board overview truncates the `story` field to 10 characters followed by `...` when needed.

## User guide

### Prerequisites

- Python 3
- Jupyter Notebook or JupyterLab
- Python packages listed in `requirements.txt`: `pandas`, `openpyxl`, `graphviz`, `playwright`, `nbclient`, `ipykernel`, and `jinja2`
- Graphviz system executable (`dot`) to render the SVG

On Ubuntu, install the system dependency with:

```bash
sudo apt-get update
sudo apt-get install graphviz
```

Install the Python packages listed in `requirements.txt` with:

```bash
python -m pip install -r requirements.txt
```

For PNG export, install the Playwright browser used by the notebook if it is not already available:

```bash
python -m playwright install chromium
```

### Run the notebook

1. Open `surf_equipment.ipynb` in VS Code, JupyterLab, or Jupyter Notebook.
2. Ensure `surf_equipment.xlsx` is in the same working directory.
3. Run all cells from top to bottom.
4. Review the validation output. It checks primary keys, row counts, foreign keys, and SQLite referential integrity.

Running the notebook recreates `surf_equipment.db`, overwrites the generated ERD files in `output/ERD/`, and regenerates the overview and fact-sheet PNG files in `output/boards/` and `output/rigs/`.