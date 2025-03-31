# Perbandingan Format File yang Didukung: Mistral OCR vs Microsoft Markitdown

| Kategori        | Format File               | Mistral OCR | Microsoft Markitdown |
| --------------- | ------------------------- | ----------- | -------------------- |
| **Dokumen**     | PDF (.pdf)                | ✅          | ✅                   |
|                 | Word (.docx)              | ✅          | ✅                   |
|                 | RTF (.rtf)                | ✅          | ❓                   |
|                 | OpenDocument Text (.odt)  | ✅          | ❓                   |
|                 | DocBook XML (.xml)        | ✅          | ❓                   |
|                 | EPUB (.epub)              | ✅          | ❓                   |
| **Presentasi**  | PowerPoint (.pptx)        | ✅          | ✅                   |
| **Spreadsheet** | Excel (.xlsx)             | ❌          | ✅                   |
| **Gambar**      | JPG/JPEG (.jpg, .jpeg)    | ✅          | ✅                   |
|                 | PNG (.png)                | ✅          | ✅                   |
|                 | GIF (.gif)                | ✅          | ✅                   |
|                 | BMP (.bmp)                | ✅          | ❓                   |
|                 | TIFF (.tiff)              | ✅          | ❓                   |
|                 | SVG (.svg)                | ✅          | ❓                   |
| **Audio**       | Audio files               | ❌          | ✅                   |
| **Akademik**    | LaTeX (.latex)            | ✅          | ❓                   |
|                 | BibTeX (.bibtex)          | ✅          | ❓                   |
|                 | BibLaTeX (.biblatex)      | ✅          | ❓                   |
|                 | Endnote XML (.xml)        | ✅          | ❓                   |
|                 | JATS XML (.xml)           | ✅          | ❓                   |
| **Web/Text**    | HTML                      | ❌          | ✅                   |
|                 | Markdown                  | ❌          | ✅                   |
|                 | CSV                       | ❌          | ✅                   |
|                 | JSON                      | ❌          | ✅                   |
|                 | XML                       | ✅          | ✅                   |
|                 | Plain text (.txt)         | ✅          | ✅                   |
|                 | Dokuwiki (.txt)           | ✅          | ❓                   |
|                 | Jupyter Notebook (.ipynb) | ✅          | ❓                   |
|                 | FictionBook (.fb2)        | ✅          | ❓                   |
|                 | OPML (.opml)              | ✅          | ❓                   |
|                 | Troff (.troff)            | ✅          | ❓                   |
| **Arsip**       | ZIP                       | ❌          | ✅                   |

## Catatan:

- ✅: Format didukung
- ❌: Format tidak didukung
- ❓: Tidak ada informasi yang jelas apakah format didukung

## Kesimpulan:

1. **Mistral OCR**: Lebih fokus pada dokumen formal, file gambar, dan format akademik.
2. **Microsoft Markitdown**: Menawarkan dukungan yang lebih luas untuk format file office, multimedia (termasuk audio), dan format web/teks terstruktur.
3. **Perbedaan utama**: Microsoft Markitdown mendukung Excel (.xlsx), audio, HTML, dan file arsip ZIP sementara Mistral OCR tidak mendukungnya.
