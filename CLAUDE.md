# Reinos del Hierro - Instrucciones de Desarrollo

## Descripción del Proyecto

Sitio web en español sobre Iron Kingdoms: Requiem para servir como referencia en campañas de rol.
Se alojará en GitHub Pages.

## Proyectos Relacionados

- **RdH** (`/Users/ludo/code/RdH/`) - Este proyecto: documentación y personajes en español
- **ik** (`/Users/ludo/code/ik/`) - Módulo Foundry VTT con compendios (inglés y español)
- **dnd5e** (`/Users/ludo/code/dnd5e/`) - Sistema D&D 5e de Foundry (referencia)

## Estado Actual

- **Fase**: Datos extraídos, pendiente de crear la página web
- **Contenido fuente**: `content/spanish_by_bookmarks/` (301 secciones del libro)
- **Página web**: No creada todavía
- **Personajes**: 12 personajes pregenerados en `docs/aventuras/`

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
├── docs/                         # GitHub Pages
│   ├── aventuras/                # Aventuras con personajes pregenerados
│   │   ├── aldea-de-espiritus/   # 6 personajes (Khador)
│   │   └── cabeza-ferrea/        # 6 personajes (estación de tren enana)
│   └── personajes/               # Referencia de opciones de personaje
│       └── trasfondos/           # Trasfondos IK con tablas de personalidad
├── books/                        # PDFs fuente (excluido de git)
├── tools/                        # Scripts Python de extracción
├── assets/                       # Imágenes (futuro)
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

## Personajes Pregenerados

### Ubicación de los personajes

Los personajes en markdown están en `docs/aventuras/`:
- `aldea-de-espiritus/` - 6 personajes para aventura en Khador
- `cabeza-ferrea/` - 6 personajes para aventura en estación de tren enana

### Generar JSONs para Foundry VTT

Los JSONs se generan con el script `characterConverter.py` del proyecto **ik**:

```bash
cd /Users/ludo/code/ik/tools

# Convertir todos los personajes de una aventura
python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/aldea-de-espiritus -o /Users/ludo/code/ik/docs/foundry_actors

python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/cabeza-ferrea -o /Users/ludo/code/ik/docs/foundry_actors

# Convertir un solo personaje
python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/aldea-de-espiritus/yuri-koskov.md -o /Users/ludo/code/ik/docs/foundry_actors
```

Los JSONs generados van a `/Users/ludo/code/ik/docs/foundry_actors/`.

### Formato del Markdown de Personaje

Ver `/Users/ludo/code/ik/CLAUDE.md` para el formato completo. Secciones clave:

- `# Nombre` - Nombre del personaje
- `**Clase N** · Raza` - Clase, nivel y raza
- `## Características` - Tabla con FUE, DES, CON, INT, SAB, CAR
- `## Atributos de Combate` - CA, PG, Velocidad
- `## Trucos` - Trucos para conjuradores (formato: `- **Nombre** — descripción`)
- `## Conjuros Preparados > ### Nivel 1` - Conjuros de nivel 1
- `## Equipo` - Tabla con objetos y peso
- `## Trasfondo: Nombre > ### Personalidad` - Rasgos de personalidad
- `## Notas` - Esencia, Aspecto, Historia

### Personajes Actuales

**Aldea de Espíritus:**
| Personaje | Clase | Raza | Conjurador |
|-----------|-------|------|------------|
| Dmitri Volkov | Paladín 1 | Humano Khadorano | Sí |
| Gorluk Tharok | Guerrero 1 | Ogrun | No |
| Ivan Starov | Bardo 1 | Humano Khadorano | Sí |
| Misha Krasnov | Pícaro 1 | Humano Khadorano | No |
| Grindar Bloodborn | Explorador 1 | Troloide | Sí |
| Yuri Koskov | Clérigo 1 | Humano Khadorano | Sí |

**Cabeza Férrea:**
| Personaje | Clase | Raza | Conjurador |
|-----------|-------|------|------------|
| Brokk Steadfast | Clérigo 1 | Troloide Urbano | Sí |
| Elias Dunford | Pícaro 1 | Humano Cygnarano | No |
| Torgun Ironbid | Guerrero 1 | Ogrun Rhúlico | No |
| Nola Carvalo | Maga Pistolera 1 | Humana Órdica | Sí |
| Gek-Darabin | Alquimista 1 | Gobo | No |
| Hilda Forgebloom | Mecánika 1 | Enana Rhúlica | No |

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
