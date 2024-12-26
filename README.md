# Eagle Exporter

**Eagle Exporter** is a command-line utility (and Python library) to parse image metadata JSON files from an [Eagle](https://en.eagle.cool/) library directory and export them to either a local Parquet file or a Hugging Face Dataset repository.

## Features

- Recursively scans an Eagle library's `images/` folder for JSON files.
- Extracts tags, star ratings, palette info, etc.
- Optionally merges with [s5cmd](https://github.com/peak/s5cmd) logs to provide S3 URIs for each file.
- Exports to either:
  - A local `.parquet` file  
  - A Hugging Face dataset (can be public or private).

## Installation

1. Install Poetry if you haven't already:
   ```bash
   pip install poetry
	```

2. Clone this repo or download the source, then inside the folder:

```bash
   poetry install
```

3. (Optional) You can install it system-wide using:

   ```
   pipx install .
   ```

   Then you'll have the `eagle-export` command available globally.

## Usage

Example command to export an Eagle library to Hugging Face:

   ```bash
eagle-export path/to/my_eagle.library --to myuser/my_hf_dataset --hf-private
   ```

Or export to a local `.parquet` file:

```bash
eagle-export path/to/my_eagle.library --to /tmp/output.parquet
```

Optionally include an s5cmd file to attach S3 URIs:

```bash
eagle-export path/to/my_eagle.library --s5cmd /path/to/s5cmd.txt --to /tmp/output.parquet
```

### Command-Line Arguments

- `EAGLE_DIR` (positional): The path to your Eagle library directory (the folder that has `images/` inside).

- `--s5cmd <FILE>` (optional): Path to the s5cmd log file (with lines like `cp localfile s3://bucket/...`)

- `--to <DEST>` (required):
  - If `<DEST>` ends with `.parquet`, will export to Parquet.
  - Otherwise, treats `<DEST>` as a Hugging Face dataset name (e.g. `username/datasetname`).
  
- `--hf-private` (optional): If exporting to Hugging Face, mark it as private.

- `--help`: Show the help message.

## Developer Notes

- The core functionality resides in `src/eagle_export/core.py`.
- The CLI is in `src/eagle_export/cli.py`.
- The library uses `click` for the command line, `pandas` for data manipulation, and `datasets` for pushing to Hugging Face.