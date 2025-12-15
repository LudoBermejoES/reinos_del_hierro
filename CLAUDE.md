# Reinos del Hierro - Instrucciones de Desarrollo

## Descripción del Proyecto

Sitio web en español sobre Iron Kingdoms: Requiem para servir como referencia en campañas de rol.
Se alojará en GitHub Pages.

## Estado Actual

- **Fase**: Datos extraídos, pendiente de crear la página web
- **Contenido fuente**: `content/spanish_by_bookmarks/` (301 secciones del libro)
- **Página web**: No creada todavía

## Estructura del Proyecto

```
/Users/ludo/code/RdH/
├── content/                      # DATOS FUENTE (NO es la web)
│   └── spanish_by_bookmarks/     # Texto extraído del PDF por capítulos
│       ├── _index.md             # Índice de todo el contenido
│       ├── 01_1-los-reinos-de-hierro/    # Cap 1: Ambientación
│       ├── 88_2-opciones-de-personaje/   # Cap 2: Razas, clases, etc.
│       ├── 183_3-magia/                  # Cap 3: Conjuros
│       ├── 268_4-equipo-y-mecánika/      # Cap 4: Equipo
│       ├── 283_5-siervos-de-vapor/       # Cap 5: Warjacks
│       └── 295_6-guía-del-director-de-juego/  # Cap 6: DJ
├── books/                        # PDFs fuente (excluido de git)
├── tools/                        # Scripts Python de extracción
├── assets/                       # Imágenes (futuro)
├── docs/                         # GitHub Pages (FUTURO - no existe aún)
├── README.md
└── CLAUDE.md
```

## Herramientas Disponibles

### pdfToText.py
Extrae texto completo de un PDF.
```bash
python tools/pdfToText.py books/archivo.pdf -o output.txt
```

### extractByBookmarks.py
Extrae contenido organizado por la estructura de marcadores/TOC del PDF.
```bash
python tools/extractByBookmarks.py books/archivo.pdf -o content/output -v
python tools/extractByBookmarks.py books/archivo.pdf --toc  # Solo ver TOC
python tools/extractByBookmarks.py books/archivo.pdf -o output --flat  # Sin directorios anidados
```

### foundryExporter.py
Genera YAML para Foundry VTT (heredado del proyecto ik5e, puede no ser necesario aquí).

## Contenido del Libro (referencia rápida)

### Capítulo 1: Los Reinos del Hierro
- Cosmología, Historia antigua
- Naciones: Cygnar, Khador, Llael, Ord, Protectorado de Menoth, Cryx, Rhul, Ios
- Ciudades principales de cada nación

### Capítulo 2: Opciones de Personaje
- **Esencias**: Intelectual, Poderosa, Ágil, Dotada, Piadosa
- **Razas**: Gobo, Humano (6 nacionalidades), Iosano, Nyss, Ogrun, Enano Rhúlico, Troloide
- **Clases IK**: Alquimista, Mago Pistolero, Tirador, Mecániko, Conjurador de Guerra
- **Subclases**: Para Bardo, Clérigo, Guerrero, Monje, Paladín, Explorador, Pícaro
- **Trasfondos**: 22 trasfondos específicos del setting
- **Compañías aventureras**: Tipos de grupos de PJs

### Capítulo 3: Magia
- Componentes rúnicos, Canalizar
- ~70 conjuros nuevos
- Objetos mágicos

### Capítulo 4: Equipo y Mecánika
- Armas de fuego (pistolas, rifles, escopetas)
- Armaduras (incluyendo armadura de vapor)
- Armas cuerpo a cuerpo específicas
- Equipo de aventurero
- Sistema de Mecánika (condensadores, placas rúnicas)

### Capítulo 5: Siervos de Vapor
- Anatomía: Chasis, Motor, Córtex, Armamento
- Mejoras y equipamiento
- Reglas de reparación y control
- Imprimaciones (bonding)

### Capítulo 6: Guía del DJ
- Magia en el mundo
- Ideas de aventuras
- Estadísticas de criaturas

## Próximos Pasos (cuando se cree la web)

1. Decidir estructura de la página web
2. Elegir generador estático (Jekyll para GitHub Pages, o similar)
3. Crear plantillas y diseño
4. Procesar contenido de `content/` para generar páginas
5. Configurar GitHub Pages

## Dependencias Python

```bash
pip install pymupdf pyyaml
```

## Notas

- El contenido en `content/` es texto extraído automáticamente, puede necesitar limpieza
- Los archivos .md en content tienen marcadores de página (`<!-- Página X -->`)
- El índice `content/spanish_by_bookmarks/_index.md` tiene enlaces a todas las secciones
