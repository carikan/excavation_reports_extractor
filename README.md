# Excavation Reports Extractor

A lightweight Python toolkit to pull ancient placenames out of the Annual Excavation Results Turkish "Kazı Sonuçları Toplantısı" PDFs using the Pleiades gazetteer.

## What it does

1. **Scrapes** the ministry's excavation‑results page (or uses your local copies in `pdf_reports/`)  
2. **Reads** each PDF page‑by‑page (via PyPDF2, with a PyMuPDF fallback)  
3. **Detects** capitalized Turkish tokens, lemmatizes them with Zemberek, and matches against Pleiades place names within modern Türkiye  
4. **Stores** each hit with:
   - PDF file name & original URL  
   - Page number  
   - A short text snippet around the match  
   - Pleiades ID, coordinates, and link  
5. **Outputs** a single `all_reports_places.json` for easy review, manual vetting, or downstream pipelines

## Quick Start

```bash
# 1. Clone this repo:
git clone https://github.com/yourusername/excavation_reports_extractor.git
cd excavation_reports_extractor

# 2. Install dependencies:
pip install pandas requests beautifulsoup4 PyPDF2 PyPDFMuPDF zemberek-python

# 3. Drop your .pdf reports into the pdf_reports/ folder.

# 4. Run the extractor:
python extract_places.py

# 5. Inspect the output:
less all_reports_places.json
```


## Limitations & Known Issues

- **Ambiguous Tokens** Words like `Kale`, `Agora`, `Stadia`, or names such as `Muradiye` can be either genuine Pleiades places *or* generic/common names in the text (or even personal/family names). 
- **Not 100% Accurate**
  - Family or personal names may slip through (e.g. the archaeologist "Muradiye")  - 
  - False positives in other languages (e.g. Italian "sia" vs. the site Sia)
- **Manual Vetting Required** Use the page numbers, PDF filenames, and text snippets to quickly review and prune false positives. Consider maintaining a `blacklist.txt` in this repo for recurring terms.

## Tips & Next Steps

- **Blacklist common words** like "Kale" or "Agora" to reduce false positives.
- **Manual vetting**: Open the JSON in a text editor or notebook, search by PDF file or place name, and correct as needed.
- **Extend**:
  - Add language filters if you mix Italian/German reports
  - Integrate with a simple web UI (Flask/Streamlit) for quicker review
  - Hook into a GitHub Actions workflow for scheduled runs

## .gitignore suggestions

```gitignore
# don't check in downloaded gazetteer or outputs
places.csv
pdf_reports/
all_reports_places.json
```

## Contributing

Feel free to open issues or PRs especially if you fınd ambiguous place names needing blacklisting, or if you have ideas for improved snippet extraction.

