# Reinos del Hierro - Guía de Campaña

Sitio web de referencia para campañas de rol en el mundo de Iron Kingdoms: Requiem, en español.

## Estado del Proyecto

**En desarrollo** - Actualmente contiene los datos extraídos del libro. La página web se creará próximamente.

## Estructura del Proyecto

```
RdH/
├── content/                 # Datos extraídos del PDF (fuente para la web)
│   └── spanish_by_bookmarks/  # Contenido organizado por capítulos
├── books/                   # PDFs fuente (no incluidos en git)
├── tools/                   # Scripts de extracción Python
├── assets/                  # Imágenes y recursos (futuro)
├── docs/                    # Sitio web GitHub Pages (futuro)
├── README.md
└── CLAUDE.md
```

## Contenido Disponible

El directorio `content/spanish_by_bookmarks/` contiene el libro extraído y organizado:

- **01_1-los-reinos-de-hierro/** - Historia, geografía, naciones y ciudades
- **88_2-opciones-de-personaje/** - Razas, clases, subclases, trasfondos
- **183_3-magia/** - Conjuros y objetos mágicos
- **268_4-equipo-y-mecánika/** - Armas, armaduras, mecánika
- **283_5-siervos-de-vapor/** - Steamjacks y warjacks
- **295_6-guía-del-director-de-juego/** - Aventuras y criaturas

## Herramientas

- `tools/pdfToText.py` - Extrae texto completo de PDFs
- `tools/extractByBookmarks.py` - Extrae contenido organizado por marcadores/TOC

### Uso

```bash
# Crear entorno virtual
python3 -m venv .venv
source .venv/bin/activate
pip install pymupdf pyyaml

# Extraer por marcadores
python tools/extractByBookmarks.py books/requiem-iron-kingdoms-ebook_compress.pdf -o content/spanish_by_bookmarks -v
```

## Licencia

Referencia no oficial para uso personal. Iron Kingdoms es propiedad de Privateer Press.
