import csv
import json
import argparse
from pathlib import Path


def csv_to_json(input_file, output_file):
    source = Path(input_file)
    destination = Path(output_file)

    if not source.is_file():
        raise FileNotFoundError(f"CSV file not found: {source}")

    with source.open("r", encoding="utf-8-sig", newline="") as file:
        reader = csv.DictReader(file)

        if not reader.fieldnames:
            raise ValueError("CSV file has no header row")

        data = list(reader)

    with destination.open("w", encoding="utf-8") as file:
        json.dump(data, file, indent=2, ensure_ascii=False)

    return len(data)


if __name__ == "__main__":
    parser = argparse.ArgumentParser(
        description="Convert a CSV file to JSON."
    )
    parser.add_argument("input", help="Path to the source CSV file")
    parser.add_argument("output", help="Path for the output JSON file")
    args = parser.parse_args()

    try:
        rows = csv_to_json(args.input, args.output)
        print(f"Converted {rows} rows into {args.output}")
    except (OSError, ValueError, csv.Error) as error:
        print(f"Error: {error}")
