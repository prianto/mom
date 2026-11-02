# Convert Meeting Notes to Word Documents

## Objective

Convert raw meeting notes (_notula_) written in Bahasa Indonesia from `.txt` files into properly formatted Microsoft Word `.docx` documents. Each source file is converted only once; files that were already converted are skipped on later runs.

## Folder Structure

```text
root/
├── AGENTS.md
├── notula.dotx
├── processed.json        <- conversion log (the only file besides docs/ output that the agent may write)
├── raw/
│   ├── meeting1.txt
│   ├── meeting2.txt
│   └── ...
└── docs/
    ├── internal_meeting_20260911.docx
    └── ...
```

## Instructions

1. Read `processed.json` in the root folder (see "Conversion Log" below). If it does not exist, treat it as an empty log.
2. List all `.txt` files inside the `raw/` folder.
3. For each file, run the "Skip Check" below. Only files that pass it are converted.
4. Convert each remaining `.txt` file into a separate `.docx` document.
5. Use `notula.dotx` as the template for every generated document.
6. Extract the meeting date from the contents of each text file.
7. Keep the output in Bahasa Indonesia.
8. Correct obvious spelling, punctuation, capitalization, and grammatical errors where appropriate.
9. Organize the notes into appropriate paragraphs, headings, lists, and numbered items.
10. Maintain a formal and professional meeting-minutes style (_notula rapat_).
11. Preserve the original meaning and all important information.
12. Do not invent information, decisions, participants, dates, or other details.
13. If information is unclear or incomplete, preserve the available information rather than making assumptions.
14. Save each generated document inside the `docs/` folder.
15. After each successful conversion, immediately update `processed.json` (see below).

## Conversion Log (`processed.json`)

`processed.json` records every source file that has been converted successfully. It is the single source of truth for "already converted".

Format:

```json
{
  "version": 1,
  "files": [
    {
      "source": "raw/meeting1.txt",
      "sha256": "<SHA-256 hash of the .txt file contents>",
      "output": "docs/internal_meeting_20260911.docx",
      "meeting_date": "2026-09-11",
      "converted_at": "2026-11-02T22:11:00+07:00"
    }
  ]
}
```

Rules:

- Compute `sha256` from the raw bytes of the `.txt` file (for example `sha256sum` or Python `hashlib`). Do not use file modification time; it is unreliable.
- Add an entry only after the `.docx` was written successfully. Never log failed, skipped, or conflicting files as converted.
- Update the log after each file, not only at the end, so an interrupted run does not cause re-conversion.
- Write the log safely: write to a temporary file, then rename it over `processed.json`. Keep valid JSON at all times.
- If `processed.json` exists but is not valid JSON, do not overwrite it. Stop and report the problem.
- Never delete entries from the log.

## Skip Check

Evaluate in this order for every `.txt` file in `raw/`. Stop at the first match.

1. **Same source, same content** — the log has an entry with the same `source` path and the same `sha256`: **skip**. Report as "already converted". Do not even re-extract the date.
2. **Same content, different name** — the log has an entry with the same `sha256` but a different `source` path (the file was renamed or copied): **skip**. Report as "duplicate of `<logged source>`".
3. **Same source, changed content** — the log has an entry with the same `source` path but a different `sha256` (the file was edited after conversion): **do not convert and do not overwrite**. Report as "source modified since conversion" and ask for explicit permission to regenerate. If permission is given, follow "Re-conversion With Permission" below.
4. **Not in the log, but target exists** — the file is not in the log, but after extracting its meeting date the target `docs/internal_meeting_YYYYMMDD.docx` already exists (for example, documents created before the log existed): **do not overwrite**. Report as a filename conflict and ask for clarification. Do not add it to the log automatically.
5. **Otherwise** — the file is new: convert it.

## Re-conversion With Permission

Only when the user explicitly approves regenerating a specific file:

- Never overwrite the existing `.docx`. Ask the user to choose between a new name (for example `internal_meeting_YYYYMMDD_v2.docx`) or an explicit overwrite approval for that specific file.
- After conversion, update that file's entry in `processed.json` with the new `sha256`, `output`, and `converted_at`.

## Output Filename Convention

Generate filenames using this format:

`internal_meeting_YYYYMMDD.docx`

The date must be extracted from the meeting date written inside the corresponding `.txt` file.

Examples:

```text
raw/meeting1.txt
  Meeting date found: 11 September 2026
  Output: docs/internal_meeting_20260911.docx

raw/meeting2.txt
  Meeting date found: 15 September 2026
  Output: docs/internal_meeting_20260915.docx
```

Additional rules:

- Use the actual meeting date, not the date the file was created or processed.
- Normalize the date into `YYYYMMDD` format.
- If multiple dates appear, identify the date that represents the meeting itself, not deadlines or dates mentioned incidentally.
- If the meeting date cannot be determined confidently, do not guess. Report the source filename and ask for clarification.
- If multiple source files produce the same target filename, do not overwrite an existing document. Report the conflict and ask for clarification.

## Formatting Requirements

- Follow the formatting, styles, layout, headers, footers, and other document settings defined in `notula.dotx`.
- Use the template's existing styles whenever possible.
- Do not replace the template with a newly created document using default Word formatting.
- Ensure the resulting documents are clean, consistent, and ready to open in Microsoft Word.

## Important Rules

- Never modify or overwrite the original `.txt` files.
- Never modify `notula.dotx`.
- Do not delete existing files in `docs/`.
- Never overwrite existing `.docx` files without explicit permission.
- Never convert a file that the Skip Check says to skip.
- The only existing file the agent may modify is `processed.json`.
- Process every `.txt` file in the `raw/` folder that passes the Skip Check.
- If there are no `.txt` files, report that no source files were found.
- If all `.txt` files were skipped, report that there is nothing new to convert.

## Completion Report

After processing, provide a concise summary containing:

- Number of `.txt` files found.
- Number of `.docx` files successfully generated.
- List of generated filenames.
- Files skipped because they were already converted (with the existing output filename).
- Files skipped as duplicates of another source file.
- Files whose content changed since conversion and need permission to regenerate.
- Files that could not be processed and the reasons.
- Any missing or ambiguous meeting dates.
- Any filename conflicts.

All generated documents must be saved in the `docs/` folder.
