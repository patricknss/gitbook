# Magick.NET

Magick.NET é o binding .NET do ImageMagick para processamento de imagens.

## Quando usar

- Redimensionar imagens no backend.
- Converter formato (`png`, `jpeg`, `webp`, `pdf`).
- Comprimir e otimizar arquivos antes de armazenar.
- Aplicar watermark, crop, rotate e ajustes de qualidade.

## Instalação (NuGet)

```bash
dotnet add package Magick.NET-Q8-AnyCPU
```

> Para melhor qualidade de cor, pode usar variações Q16 (`Magick.NET-Q16-AnyCPU`).

## Exemplo: redimensionar e converter para WebP

```csharp
using ImageMagick;

public static byte[] ConverterParaWebp(byte[] imagemOriginal)
{
    using var image = new MagickImage(imagemOriginal);

    image.Resize(1280, 0); // mantém proporção
    image.Quality = 80;
    image.Format = MagickFormat.WebP;

    return image.ToByteArray();
}
```

## Exemplo: gerar thumbnail

```csharp
public static byte[] GerarThumbnail(byte[] imagemOriginal)
{
    using var image = new MagickImage(imagemOriginal);

    image.Resize(new MagickGeometry(300, 300) { IgnoreAspectRatio = false });
    image.Crop(300, 300, Gravity.Center);
    image.Quality = 75;
    image.Format = MagickFormat.Jpeg;

    return image.ToByteArray();
}
```

## Boas práticas

- Validar tamanho e formato antes de processar.
- Limitar dimensões máximas para evitar alto consumo de memória.
- Processar em background quando volume for alto.
- Manter presets de qualidade por contexto (thumbnail, preview, original).
