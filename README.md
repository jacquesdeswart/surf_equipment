# Surf Equipment Database

This repository turns the data in `surf_equipment.xlsx` into a SQLite database and an entity relationship diagram (ERD).

## Repository contents

- `surf_equipment.xlsx`: source workbook. Its first 9 sheets are master data tables; its final 2 sheets are transactional tables.
- `surf_equipment.ipynb`: interactive Python notebook that reads the workbook, creates the database, validates keys, and generates the ERD.
- `surf_equipment.db`: generated SQLite database containing the 11 tables, primary keys, and foreign-key constraints.
- `surf_equipment_erd.svg`: generated visual ERD.
- `surf_equipment_erd.dot`: Graphviz source for the ERD.

The first column of every worksheet becomes that table's primary key. A transactional column whose name matches a table primary key becomes a foreign key. This includes relationships between transactional tables, such as `rig_board.rig_id` referencing `rigs.rig_id`. ERD arrows show `1` at the referenced table and `∞` when multiple child rows may reference it.

## User guide

### Prerequisites

- Python 3
- Jupyter Notebook or JupyterLab
- Python packages: `pandas`, `openpyxl`, `graphviz`, and `nbclient`
- Graphviz system executable (`dot`) to render the SVG

On Ubuntu, install the system dependency with:

```bash
sudo apt-get update
sudo apt-get install graphviz
```

Install the Python packages with:

```bash
python -m pip install pandas openpyxl graphviz nbclient
```

### Run the notebook

1. Open `surf_equipment.ipynb` in VS Code, JupyterLab, or Jupyter Notebook.
2. Ensure `surf_equipment.xlsx` is in the same working directory.
3. Run all cells from top to bottom.
4. Review the validation output. It checks primary keys, row counts, foreign keys, and SQLite referential integrity.

Running the notebook recreates `surf_equipment.db` and overwrites the existing generated ERD files. The ERD is written to `surf_equipment_erd.svg`; `surf_equipment_erd.dot` remains available as editable Graphviz source.