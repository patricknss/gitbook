# IronPDF

IronPDF e uma biblioteca .NET para geracao e manipulacao de PDF, incluindo conversao de HTML para PDF.

## Quando usar

- Gerar contratos, boletos e comprovantes em PDF.
- Criar PDFs a partir de templates HTML/CSS.
- Mesclar ou proteger documentos PDF.

## Instalacao

```bash
dotnet add package IronPdf
```

## Exemplo basico (HTML para PDF)

```csharp
using IronPdf;

var renderer = new ChromePdfRenderer();
PdfDocument pdf = renderer.RenderHtmlAsPdf("<h1>Relatorio</h1><p>Gerado com IronPDF.</p>");
pdf.SaveAs("relatorio.pdf");
```

## Veja tambem

- Geracao de planilhas: [NPOI](npoi.md)
- Trilha principal de backend: [Backend (Fundamentos e APIs)](../back-end/backend/README.md)
- Arquitetura de dominio: [DDD (Arquitetura Autoglass)](../back-end/ddd/README.md)
