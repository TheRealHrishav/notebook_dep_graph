# Notebook Dependency Graph Builder

## Overview

This tool is a static analysis CLI designed to parse Jupyter notebooks (`.ipynb`), extract logical data objects, build a dependency graph, and write out various visual and programmatic artifacts. It traces data lineage through SQL execution, pandas DataFrame operations, and dynamic string templating.

## Features

* **AST-Based Python Parsing**: Extracts logical objects—such as tables, views, DataFrames, and dictionary-query entries—from Python notebook cells.


* **SQL Analysis**: Analyzes SQL queries (specifically targeting the SQLite dialect) using `sqlglot` to statically extract read/written tables and Common Table Expressions (CTEs).


* **Dynamic Template Resolution**: Bounded static loop expansion safely resolves dict-of-queries, filters, and `.replace()` template chains (e.g., `TK_04`/`TK_06` patterns) without executing the notebook code.


* **Dual Graph Generation**: Builds an overarching object dependency graph as well as a derived cell-to-cell execution graph.


* **Confidence Scoring**: Edges and objects are assigned confidence tiers (`high`, `medium`, `low`) based on how deterministically the AST and SQL could be statically resolved.



## Exported Artifacts

The CLI generates several outputs in the specified output directory to facilitate both programmatic filtering and visual review:

* **JSON**: Full dependency graph output matching an `edge_metadata` schema.


* **CSV**: Two tabular files, `objects.csv` and `dependency_edges.csv`, enabling pandas-based filtering by notebook, object kind, or confidence.


* **GraphML**: Structured output of both the object graph and the cell graph for external graph analysis tools.


* **Mermaid Flowchart**: A text-based `cell_graph.mmd` rendering the notebook cell dependencies.


* **SVG/DOT**: A visual diagram of the object graph rendered via Graphviz.


* **Markdown Summary**: A `SUMMARY.md` detailing extraction counts, external dependencies, and low-confidence edges requiring manual review.



## Requirements

* Python 3.8+
* `networkx`

* `pandas`

* `sqlglot`

* **Optional**: Graphviz (`dot` binary) must be installed on the system to render `.svg` diagrams. If absent, the pipeline falls back to writing only the `.dot` file.



## Usage

Run the CLI script directly to parse the default repository structure, or provide explicit directories:

```bash
# Basic execution
python scripts/build_dep_graph.py

# Execution with custom directories
python scripts/build_dep_graph.py --notebooks-dir /path/to/notebooks --out-dir /path/to/output

```

* **`--notebooks-dir`**: The directory containing `.ipynb` files. Defaults to `../notebooks` relative to the repository root.


* **`--out-dir`**: The output directory for the generated artifacts. Defaults to `../output`.
