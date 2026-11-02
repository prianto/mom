# Convert Meeting Notes to Word Documents

## Objective

Convert raw meeting notes (_notula_) written in Bahasa Indonesia from `.txt` files into properly formatted Microsoft Word `.docx` documents.

## Folder Structure

```text
root/
├── AGENTS.md
├── notula.dotx
├── text/
│   ├── meeting1.txt
│   ├── meeting2.txt
│   └── ...
└── docs/
    ├── internal_meeting_20260911.docx
    └── ...
```

## Instructions

1. Read all `.txt` files inside the `text/` folder.
2. Convert each `.txt` file into a separate `.docx` document.
3. Use `notula.dotx` as the template for every generated document.
4. Extract the meeting date from the contents of each text file.
5. Keep the output in Bahasa Indonesia.
6. Correct obvious spelling, punctuation, capitalization, and grammatical errors where appropriate.
7. Organize the notes into appropriate paragraphs, headings, lists, and numbered items.
8. Maintain a formal and professional meeting-minutes style (_notula rapat_).
9. Preserve the original meaning and all important information.
10. Do not invent information, decisions, participants, dates, or other details.
11. If information is unclear or incomplete, preserve the available information rather than making assumptions.
12. Save each generated document inside the `docs/` folder.

## Output Filename Convention

Generate filenames using this format:

`internal_meeting_YYYYMMDD.docx`

The date must be extracted from the meeting date written inside the corresponding `.txt` file.

Examples:

```text
text/meeting1.txt
  Meeting date found: 11 September 2026
  Output: docs/internal_meeting_20260911.docx

text/meeting2.txt
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
- Process every `.txt` file in the `text/` folder.
- If there are no `.txt` files, report that no source files were found.

## Completion Report

After processing, provide a concise summary containing:

- Number of `.txt` files found.
- Number of `.docx` files successfully generated.
- List of generated filenames.
- Files that could not be processed and the reasons.
- Any missing or ambiguous meeting dates.
- Any filename conflicts.

All generated documents must be saved in the `docs/` folder.
