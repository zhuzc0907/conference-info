---
name: conference-info
description: Research, verify, normalize, and output current information about academic conferences, journals, workshops, and symposia in AI, machine learning, computer science, AI in education, educational data mining, learning analytics, and related fields. Use when the user supplies one or more venue names, acronyms, URLs, CFP snippets, or partial venue details and wants records matching the established 28-field Feishu venue database, including venue-type-specific CCF/ICORE or CAS/JCR/IF ratings, a directly pasteable table row, venue information lookup, CFP/deadline verification, or an output file such as XLSX, CSV, TSV, Markdown, or JSON.
---

# Conference Info

## Core contract

Convert each user-supplied venue into one normalized record matching the existing venue database. Research current information rather than relying on memory. Preserve uncertainty: leave unavailable fields blank and say what was not confirmed instead of inferring dates, policies, rankings, or fees.

Always read `references/table-schema.md` before researching or producing records. It defines the exact 28-column order, allowed single-select values, date format, identifier rules, rating format, and output contract.

## 1. Resolve the requested venue

Accept a full name, acronym, official URL, CFP URL, pasted description, or a list of venues. Resolve ambiguous acronyms against the supplied topic and URLs. Ask a clarifying question only when two plausible venues remain and choosing one would create a materially different record.

Treat each conference series, journal, workshop, or symposium as one venue record. Do not silently merge a main conference with a colocated workshop, education track, journal, or similarly named event.

## 2. Research current official information

Browse the web for every request because venue details and submission cycles change. Prefer sources in this order:

1. the current edition's official conference or society page;
2. the official CFP, author guide, submission page, or timetable;
3. the official publisher or proceedings page;
4. the official parent association page.

Use secondary sources only to discover an official page, not as the final authority for dates, scope, review rules, fees, or publication venue. Do not use an expired edition to fill an unannounced future edition. When current official pages disagree, prefer the page specific to the current edition and describe the conflict in `备注`.

Verify, when applicable:

- canonical English name, acronym, organizer, publisher, and proceedings venue;
- venue type, scope, suitable paper types, review/submission characteristics, and open-access or fee model;
- current edition, event dates, location, current submission state, official homepage, and CFP/submission page;
- the date on which the information was checked.

For journals, verify the official journal and author-guide pages. Leave conference-only fields blank when they do not apply. Do not invent review duration, acceptance rate, impact factor, CCF rank, JCR quartile, CAS partition, APC, or deadlines.

For the `评级` field, use the venue type as the decision rule and keep the cell concise:

- Conferences and workshops: report both CCF and ICORE, for example `CCF A；ICORE A*（2026）`. Use `CCF待核验` or `CCF未收录` only when that status is actually supported, and use `ICORE待核验` when the current ICORE/Core portal has not confirmed a rank. Keep at most one necessary year/version marker; do not append repeated notes such as “按本单位最新版目录复核”.
- Journals: report verified CAS major/minor partition, JCR quartile, and the latest verified Journal Impact Factor whenever available, for example `中科院大类2区/小类3区；JCR Q1；IF 10.1（2025）`. Missing components must be written as `待核验`, and a public IF may be included with a short source/year marker when the authoritative database value is unavailable. Do not guess.
- Do not treat IF, JCR quartile, CAS partition, CCF category, or ICORE rank as interchangeable. Preserve only the necessary year/version in the rating string; put detailed source or conflict notes in `备注`.

## 3. Normalize the record

Follow the field definitions and exact column order in `references/table-schema.md`.

Use ISO dates (`YYYY-MM-DD`) in chat, CSV, TSV, Markdown, and JSON. Use actual date values with `yyyy-mm-dd` display formatting in XLSX. Keep URLs as plain absolute URLs. Remove embedded tabs and line breaks from every cell.

Use the controlled values exactly for `一级领域`, `类型`, `投稿节奏`, `当前投稿节点`, and `信息状态`. Do not create a near-synonym. Write free-text fields in concise Chinese, retaining official English names where accuracy requires them.

Build `记录标题` as `简称｜英文全称`. Build `场所信息引用建议` as:

`English Full Name (ACRONYM), official website, accessed YYYY-MM-DD: URL`

Assign `场所编号` only when an existing destination table is supplied or accessible and the next unused number can be checked. Otherwise leave it blank; never guess a sequence number. Mention the blank identifier in the chat summary.

Use the concise generic text `待核验` in `CCF/JCR/中科院分区` unless the user explicitly requests a current ranking check and an authoritative current list is actually verified. Put the requested concise, venue-type-specific result in `评级`.

## 4. Choose the output

Honor an explicitly requested output format.

- **XLSX**: use the spreadsheet skill. Create `场所总表`, `字段与使用说明`, and `飞书单选选项`, matching the established workbook structure and style. Put the researched rows in the exact 28-column schema and preserve typed dates and URLs.
- **CSV or TSV**: create one flat UTF-8 file containing the 28-column header and one row per venue. Use TSV when the user prioritizes direct pasting into a table.
- **Markdown**: create a `.md` file with a concise result summary, sources, and a complete table. Do not drop fields because the table is wide.
- **JSON**: create a UTF-8 array of objects whose keys exactly match the 28 field names and order.
- **Other requested format**: use the appropriate artifact skill while preserving the same field semantics and source URLs.

Use a filename such as `会议期刊信息_YYYY-MM-DD.ext`. Verify that the generated file exists and that every record has exactly 28 fields. For XLSX, visually inspect every sheet before delivery.

## 5. Default chat output

When the user does not specify an output format, do not create a file. Return:

1. a short Chinese summary of identity, scope, current cycle/status, and any unconfirmed item;
2. source links placed next to the claims they support;
3. a fenced `tsv` block containing the exact 28-column header and one row per venue.

The TSV block is the pasteable record. Use literal tab characters, one physical line per record, no tabs or newlines inside cells, and empty cells for unknown or inapplicable values. Tell the user to paste it into the first cell of the destination table. Do not replace the TSV with a Markdown pipe table.

Before delivery, check:

- the venue identity is unambiguous;
- all time-sensitive facts came from current official pages;
- every row contains exactly 28 fields in the required order;
- controlled fields use only allowed options;
- dates and URLs use the required format;
- unknown facts remain blank or explicitly unconfirmed;
- the chat answer or generated artifact includes usable source links.
