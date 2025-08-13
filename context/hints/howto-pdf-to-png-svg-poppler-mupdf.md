# How to convert PDF figures to PNG or SVG with Poppler and MuPDF

This guide shows reliable CLI commands to convert academic figure PDFs into PNG (raster) or SVG (vector) on Windows, using Poppler's pdftocairo and MuPDF's mutool.

## When to choose PNG vs SVG
- Use SVG when you need infinite scaling on slides/web and the PDF is mostly vector graphics or text.
- Use PNG when you need a fixed-resolution image, or the source has effects that don’t translate well to SVG.

## Install on Windows
- Chocolatey:
```powershell
choco install poppler mupdf
```
- Scoop:
```powershell
scoop install poppler mupdf
```
- Winget (MuPDF nightly may vary; Poppler is typically via choco/scoop):
```powershell
winget install MuPDF.MuPDF
```

Poppler provides pdftocairo. MuPDF provides mutool.

---

## Poppler: pdftocairo
Official docs: https://www.mankier.com/1/pdftocairo and https://manpages.debian.org/testing/poppler-utils/pdftocairo.1.en.html

### PDF → PNG (high quality)
- Single page (page 1), 600 DPI, single output file:
```powershell
pdftocairo -png -r 600 -f 1 -l 1 -singlefile "input.pdf" "output"
```
- Transparent background (PNG only):
```powershell
pdftocairo -png -r 600 -transp -f 1 -l 1 -singlefile "input.pdf" "output"
```
- All pages to numbered PNGs:
```powershell
pdftocairo -png -r 300 "input.pdf" "output"
# Produces output-1.png, output-2.png, ...
```

Notes:
- Increase `-r` (e.g., 600–1200) for print-quality or detailed plots.
- Omit `-singlefile` to get one PNG per page; otherwise use `-f/-l` to pick a page.

### PDF → SVG (preserve vectors)
- Export (each page becomes a separate SVG):
```powershell
pdftocairo -svg "input.pdf" "output.svg"
# Produces output.svg for single-page, or output-1.svg, output-2.svg, ... for multi-page.
```

---

## MuPDF: mutool
Docs: https://mupdf.readthedocs.io/en/latest/mutool-draw.html and https://mupdf.readthedocs.io/en/latest/mutool-convert.html

### PDF → PNG
- Single page (1), 600 DPI:
```powershell
mutool draw -o "output.png" -r 600 "input.pdf" 1
```
- All pages to numbered files:
```powershell
mutool draw -o "output-%03d.png" -r 300 "input.pdf"
```

### PDF → SVG
- Using mutool draw (format inferred from extension):
```powershell
mutool draw -o "output.svg" "input.pdf" 1
```
- Using mutool convert (also infers by extension):
```powershell
mutool convert -o "output.svg" "input.pdf" 1
```

Notes:
- `-r` controls raster DPI for image outputs; it doesn’t affect SVG geometry but can affect embedded image sampling.
- Some PDFs (complex transparency, filters) may rasterize parts of the SVG.

---

## Common tasks

### Crop away white margins first (optional but common for figures)
Using TeX Live’s pdfcrop:
```powershell
pdfcrop --margins 0 "input.pdf" "cropped.pdf"
```
Then convert `cropped.pdf` with pdftocairo or mutool as above.

### Batch convert a folder of PDFs
- All PDFs to 600‑DPI PNG (first page only) with Poppler:
```powershell
Get-ChildItem -Filter *.pdf | ForEach-Object {
  $name = [System.IO.Path]::GetFileNameWithoutExtension($_.FullName)
  pdftocairo -png -r 600 -f 1 -l 1 -singlefile $_.FullName ("$name")
}
```
- All PDFs to SVG (first page only) with MuPDF:
```powershell
Get-ChildItem -Filter *.pdf | ForEach-Object {
  $out = $_.BaseName + ".svg"
  mutool draw -o $out $_.FullName 1
}
```

---

## Tips for quality and fidelity
- DPI: Use 300 for slides, 600–1200 for print or dense plots.
- Transparency: `pdftocairo -png -transp` gives a transparent background when possible; SVG is inherently transparent.
- Fonts/text: If text in the SVG becomes outlines, try a different converter (e.g., mutool vs pdftocairo) or test `pdf2svg`.
- Vector vs raster: Prefer SVG when the source is vector; for heavy raster or filters, PNG may look closer to the original.

## Related tools (optional)
- pdf2svg (Poppler/Cairo-based): https://github.com/dawbarton/pdf2svg

## References
- Poppler pdftocairo manual: https://www.mankier.com/1/pdftocairo
- Poppler utils manpage: https://manpages.debian.org/testing/poppler-utils/pdftocairo.1.en.html
- MuPDF mutool draw: https://mupdf.readthedocs.io/en/latest/mutool-draw.html
- MuPDF mutool convert: https://mupdf.readthedocs.io/en/latest/mutool-convert.html
- pdf2svg: https://github.com/dawbarton/pdf2svg
