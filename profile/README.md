# Dokmatiq

**Professional document and spreadsheet generation as a service.** Convert HTML, Markdown, and templates into pixel-perfect PDFs, DOCX, e-invoices, and Excel workbooks -- with a single API call.

---

## What is DocGen?

DocGen is a powerful document and spreadsheet generation API built for developers who need reliable, high-quality output at scale. Whether you're generating invoices, contracts, reports, data exports, or certificates -- DocGen handles the complexity so you can focus on your application.

### Core Capabilities

| Feature | Description |
|---------|-------------|
| **Document Generation** | HTML, Markdown, and ODT templates to PDF/DOCX with field substitution, images, QR codes, tables, and barcodes |
| **Multi-Part Composition** | Combine multiple document parts into a single output with shared watermarks and stationery |
| **PDF Operations** | Merge, split, rotate, extract text, read/write metadata, convert to PDF/A |
| **Digital Signatures** | Sign PDFs with PKCS#12 certificates, verify existing signatures |
| **PDF Forms** | Inspect and fill PDF form fields programmatically |
| **ZUGFeRD / Factur-X** | Embed and extract structured invoice data in PDF invoices (EN16931, XRechnung) |
| **XRechnung** | Generate, parse, validate, and transform e-invoices (CII and UBL formats) |
| **Excel Generation** | Create styled XLSX workbooks from JSON, CSV, or templates -- with formulas, freeze panes, print areas, and cell styling |
| **Excel Conversion** | Convert between XLSX, CSV, and JSON; fill Excel templates with data |
| **Watermarks & Stationery** | Add text watermarks and PDF letterhead backgrounds |
| **Preview** | Render PDF pages as PNG images for thumbnail generation |
| **Async Processing** | Long-running jobs with polling and webhook callbacks |

---

## SDKs

We provide official SDKs with fluent builder APIs, typed models, automatic retries, and comprehensive error handling:

<table>
<tr>
<td align="center" width="25%">

### Python
`pip install dokmatiq-docgen`

```python
from docgen import DocGen

dg = DocGen("your-api-key")
pdf = dg.html_to_pdf("<h1>Hello!</h1>")
```

</td>
<td align="center" width="25%">

### TypeScript
`npm install @dokmatiq/docgen`

```typescript
import { DocGen } from "@dokmatiq/docgen";

const dg = new DocGen({ apiKey: "your-api-key" });
const pdf = await dg.htmlToPdf("<h1>Hello!</h1>");
```

</td>
<td align="center" width="25%">

### Java
Maven: `com.dokmatiq:docgen-sdk`

```java
var dg = new DocGen("your-api-key");
byte[] pdf = dg.htmlToPdf("<h1>Hello!</h1>");
```

</td>
<td align="center" width="25%">

### PHP
`composer require dokmatiq/docgen-sdk`

```php
use Dokmatiq\DocGen\DocGen;

$dg = new DocGen("your-api-key");
$pdf = $dg->htmlToPdf("<h1>Hello!</h1>");
```

</td>
</tr>
</table>

All SDKs live in our [docgen-sdks](https://github.com/dokmatiq/docgen-sdks) repository.

---

## Fluent Builder API

Every SDK provides an intuitive, chainable builder for complex documents:

```python
pdf = (dg.document()
    .html("<h1>Invoice {{nr}}</h1>")
    .template("invoice.odt")
    .field("nr", "RE-2026-001")
    .field("date", "2026-04-12")
    .image("logo", "logo.png")
    .qr_code("payment", "BCD\n002\n1\nSCT\n...")
    .table("items", TableData(
        columns=[ColumnDef("Article", 80), ColumnDef("Price", 30)],
        rows=[["Widget", "9.99"], ["Gadget", "24.99"]],
    ))
    .watermark("DRAFT")
    .as_pdf()
    .generate())
```

---

## E-Invoicing (ZUGFeRD / XRechnung)

Built-in support for German and European e-invoicing standards:

```python
invoice = (dg.invoice()
    .number("RE-2026-001")
    .date("2026-04-12")
    .seller(Party("ACME GmbH", "Musterstr. 1", "10115", "Berlin"))
    .buyer(Party("Kunde AG", "Kundenweg 5", "20095", "Hamburg"))
    .item(InvoiceItem("Consulting", 120.0, quantity=8, unit=InvoiceUnit.HOUR))
    .bank(BankAccount("DE89370400440532013000"))
    .build())

# Embed ZUGFeRD XML in PDF
zugferd_pdf = dg.zugferd.embed("invoice.pdf", invoice)

# Generate XRechnung XML
xrechnung_xml = dg.xrechnung.generate(invoice)
```

---

## Excel Workbook Generation

Create fully styled Excel workbooks programmatically -- or convert between CSV, JSON, and XLSX:

```python
# Generate a styled workbook from structured data
xlsx = dg.excel.generate({
    "sheets": [{
        "name": "Sales Q1",
        "columns": [
            {"header": "Product", "width": 25},
            {"header": "Revenue", "width": 15, "format": "#,##0.00 €"},
        ],
        "rows": [
            {"values": ["Widget", 12500.50]},
            {"values": ["Gadget", 8300.00]},
        ],
        "formulas": [
            {"cell": "B4", "formula": "SUM(B2:B3)", "label": "Total"}
        ],
        "freezePane": {"row": 1, "col": 0},
        "autoFilter": True,
    }]
})

# Quick CSV → Excel conversion
xlsx = dg.excel.from_csv(csv_data, has_header=True)

# Extract Excel data as JSON
data = dg.excel.to_json(excel_base64)

# Fill an existing Excel template
xlsx = dg.excel.fill_template(template_base64, values={"B2": "Q1 2026"})
```

---

## AI Integration

DocGen integrates seamlessly with AI workflows:

- **MCP Server** -- Use DocGen as a tool in Claude Desktop, Claude Code, or any MCP-compatible AI assistant
- **Claude Code Skill** -- Natural language document generation directly from your terminal

---

## Getting Started

1. **Create an account** at the [Dokmatiq Developer Portal](https://developer.dokmatiq.com)
2. **Generate an API key** in your dashboard
3. **Install an SDK** and start generating documents in minutes

```bash
# Python
pip install dokmatiq-docgen

# TypeScript / Node.js
npm install @dokmatiq/docgen

# Java (Maven)
# Add com.dokmatiq:docgen-sdk to your pom.xml

# PHP
composer require dokmatiq/docgen-sdk
```

---

## Links

- [Developer Portal](https://developer.dokmatiq.com) -- Sign up, manage API keys, view usage
- [API Documentation](https://docs.dokmatiq.com) -- Full REST API reference
- [SDK Repository](https://github.com/dokmatiq/docgen-sdks) -- Source code for all SDKs
- [Website](https://dokmatiq.com) -- Product information and pricing

---

<p align="center">
  <strong>Built in Germany. Made for developers.</strong>
  <br>
  <sub>Questions? Reach us at <a href="mailto:developer@dokmatiq.com">developer@dokmatiq.com</a></sub>
</p>
