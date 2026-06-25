# NPOI

NPOI e uma biblioteca .NET para leitura e escrita de arquivos Excel (`.xls` e `.xlsx`) sem depender do Microsoft Office instalado.

## Quando usar

- Gerar planilhas de relatorio.
- Ler planilhas importadas por usuarios.
- Automatizar exportacoes em jobs e APIs.

## Instalacao

```bash
dotnet add package NPOI
```

## Exemplo basico (gerando Excel)

```csharp
using NPOI.SS.UserModel;
using NPOI.XSSF.UserModel;

IWorkbook workbook = new XSSFWorkbook();
ISheet sheet = workbook.CreateSheet("Produtos");
IRow row = sheet.CreateRow(0);

row.CreateCell(0).SetCellValue("Codigo");
row.CreateCell(1).SetCellValue("Nome");

using var file = File.Create("produtos.xlsx");
workbook.Write(file);
```

## Veja tambem

- Trilha geral de backend: [Backend (Fundamentos e APIs)](../back-end/backend/README.md)
- Geracao de documentos PDF: [IronPDF](iron-pdf.md)
- Consultas e dados relacionais: [Banco de Dados](../data-science/sql/README.md)
