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
- `aldea-de-espiritus/` - 6 PJs para aventura en Khador
- `aldea-de-espiritus/taberna/` - 6 NPCs de la taberna local
- `aldea-remota/` - 10 NPCs de aldea de montaña
- `cabeza-ferrea/` - 6 PJs para aventura en estación de tren enana

### Paso 1: Crear Personajes en Markdown

Crear archivos `.md` siguiendo las templates de abajo. El nombre del archivo debe ser en minúsculas con guiones: `nombre-apellido.md`.

### Paso 2: Generar JSONs para Foundry VTT

Los JSONs se generan con el script `characterConverter.py` del proyecto **ik**:

```bash
cd /Users/ludo/code/ik/tools

# Convertir todos los personajes de una aventura
python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/aldea-de-espiritus -o /Users/ludo/code/ik/tools/foundry_export_es/actors/aldea-de-espiritus

python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/cabeza-ferrea -o /Users/ludo/code/ik/tools/foundry_export_es/actors/cabeza-ferrea

# Convertir un solo personaje
python3 characterConverter.py /Users/ludo/code/RdH/docs/aventuras/aldea-de-espiritus/yuri-koskov.md -o /Users/ludo/code/ik/tools/foundry_export_es/actors/aldea-de-espiritus
```

El script carga automáticamente:
- **Armas y armaduras** de D&D 5e (en inglés) y de IK (en español)
- **Conjuros** de D&D 5e y de IK
- **Razas, clases y trasfondos** de IK

Items reconocidos mostrarán `[+] Matched`, items no reconocidos crean loot genérico `[-] No match`.

### Paso 3: Generar Packs LevelDB para Foundry VTT

Después de generar los JSONs, crear el pack de Foundry:

```bash
cd /Users/ludo/code/ik

# Crear directorio del pack si no existe
mkdir -p foundry_packs_es/ik-actors-NOMBRE-AVENTURA

# Empaquetar los JSONs en LevelDB
fvtt package pack ik-actors-NOMBRE-AVENTURA \
  --in tools/foundry_export_es/actors/NOMBRE-AVENTURA \
  --out foundry_packs_es/ik-actors-NOMBRE-AVENTURA \
  --type Actor

# IMPORTANTE: fvtt crea un subdirectorio anidado, mover los archivos:
cd foundry_packs_es/ik-actors-NOMBRE-AVENTURA
mv ik-actors-NOMBRE-AVENTURA/* .
rmdir ik-actors-NOMBRE-AVENTURA
```

### Paso 4: Registrar el Pack en module.json

Editar `/Users/ludo/code/ik/module.json`:

1. Añadir entrada en `packs`:
```json
{
  "name": "ik-actors-NOMBRE-AVENTURA",
  "label": "RdH: Personajes - Nombre Aventura",
  "path": "foundry_packs_es/ik-actors-NOMBRE-AVENTURA",
  "type": "Actor",
  "ownership": {
    "PLAYER": "OBSERVER",
    "ASSISTANT": "OWNER"
  },
  "system": "dnd5e",
  "flags": {}
}
```

2. Añadir el nombre del pack a `packFolders[0].packs`.

---

## Templates de Personajes

### Template: Personaje Jugador (PJ)

```markdown
---
layout: default
title: Nombre Apellido - Clase
---

# Nombre Apellido

**Clase N** · Raza (Nacionalidad)

---

## Características

| Característica | Puntuación | Modificador | Notas |
|----------------|------------|-------------|-------|
| Fuerza | 10 | +0 | Base X + racial |
| Destreza | 10 | +0 | |
| Constitución | 10 | +0 | |
| Inteligencia | 10 | +0 | |
| Sabiduría | 10 | +0 | |
| Carisma | 10 | +0 | |

*Point buy: X+X+X+X+X+X = 27 pts*

---

## Atributos de Combate

| Atributo | Valor |
|----------|-------|
| **Clase de Armadura** | X (tipo armadura) |
| **Puntos de Golpe** | X (dado + CON) |
| **Velocidad** | 30 pies |
| **Bonificador de Competencia** | +2 |
| **Iniciativa** | +X |

---

## Rasgos Raciales (Raza)

- **Rasgo 1:** Descripción
- **Idiomas:** Idioma1, Idioma2

---

## Rasgos de Clase (Clase N)

### Rasgo Principal
- Descripción del rasgo

---

## Trucos

- **Nombre del truco** — Descripción breve

---

## Conjuros Preparados

### Nivel 1 (X espacios)
- **Nombre del conjuro** — Descripción breve

---

## Competencias

### Tiradas de Salvación
- Característica (+X)

### Habilidades
- Habilidad (+X) — fuente

### Armas y Armaduras
- Lista de competencias

### Herramientas
- Lista de herramientas

---

## Equipo

| Objeto | Peso |
|--------|------|
| Objeto 1 | X lb. |
| **Total** | **X lb.** |

**Monedas:** X po

---

## Ataques

| Arma | Ataque | Daño | Propiedades |
|------|--------|------|-------------|
| Arma 1 | +X | XdX+X tipo | Propiedades |

---

## Trasfondo: Nombre Trasfondo

- **Competencias de habilidad:** Hab1, Hab2
- **Rasgo:** Nombre del rasgo

### Personalidad

- **Rasgo:** Texto del rasgo de personalidad.
- **Ideal:** Texto del ideal. (Alineamiento)
- **Vínculo:** Texto del vínculo.
- **Defecto:** Texto del defecto.

---

## Notas

**Esencia:** Nombre (bonus)

**Aspecto:** Descripción física detallada.

**Historia:** Trasfondo narrativo del personaje.

---

← [Volver al Grupo](index.md)
```

### Template: NPC Plebeyo

```markdown
---
layout: default
title: Nombre Apellido - Rol
---

# Nombre Apellido

**Plebeyo** · Raza Nacionalidad

---

## Características

| Característica | Puntuación | Modificador |
|----------------|------------|-------------|
| Fuerza | 10 | +0 |
| Destreza | 10 | +0 |
| Constitución | 10 | +0 |
| Inteligencia | 10 | +0 |
| Sabiduría | 10 | +0 |
| Carisma | 10 | +0 |

---

## Atributos de Combate

| Atributo | Valor |
|----------|-------|
| **Clase de Armadura** | 10 |
| **Puntos de Golpe** | 4 (1d8) |
| **Velocidad** | 30 pies |
| **Iniciativa** | +0 |

---

## Rasgos Raciales (Nacionalidad)

- **Rasgo racial:** Descripción
- **Idiomas:** Idioma

---

## Habilidades

- Habilidad (+X)

---

## Equipo

| Objeto | Peso |
|--------|------|
| Objeto | X lb. |
| **Total** | **X lb.** |

**Monedas:** X pc

---

## Ataques

| Arma | Ataque | Daño | Propiedades |
|------|--------|------|-------------|
| Garrote | +2 | 1d4 contundente | — |

---

## Trasfondo: Rol en la Comunidad

Descripción del rol del NPC en la comunidad.

### Personalidad

- **Rasgo:** Texto.
- **Ideal:** Texto. (Alineamiento)
- **Vínculo:** Texto.
- **Defecto:** Texto.

---

## Notas

**Aspecto:** Descripción física.

**Historia:** Trasfondo breve.

---

← [Volver al Índice](index.md)
```

---

## Mapeo de Items Español → Inglés

El script `characterConverter.py` reconoce automáticamente estos nombres en español:

### Armas (mapean a D&D 5e)
| Español | Inglés |
|---------|--------|
| Daga/Dagas | Dagger |
| Espada corta | Shortsword |
| Espada larga | Longsword |
| Espadón | Greatsword |
| Hacha de batalla/guerra | Battleaxe |
| Martillo de guerra | Warhammer |
| Maza | Mace |
| Lanza | Spear |
| Jabalina/Jabalinas | Javelin |
| Arco corto | Shortbow |
| Arco largo | Longbow |
| Ballesta ligera | Light Crossbow |
| Cimitarra | Scimitar |
| Alabarda | Halberd |
| Bastón | Quarterstaff |

### Armaduras (mapean a D&D 5e)
| Español | Inglés |
|---------|--------|
| Armadura de cuero | Leather Armor |
| Cuero tachonado | Studded Leather Armor |
| Camisa de malla | Chain Shirt |
| Cota de malla | Chain Mail |
| Cota de escamas | Scale Mail |
| Escudo | Shield |

### Items de IK (mapean al compendio de IK)
| Español | Item IK |
|---------|---------|
| Pistola magifusil | Pistola de Cerrojo Mágico |
| Rifle magifusil | Rifle de Cerrojo Mágico |
| Kit de alquimia de campo | Kit de Alquimia de Campo |
| Delantal blindado | Delantal Blindado |
| Máscara de gas | Máscara de Gas |
| Pistola de remaches | Pistola de Remaches |

### Equipo Aventurero (mapean a D&D 5e)
| Español | Inglés |
|---------|--------|
| Flechas | Arrows |
| Aljaba/Carcaj | Quiver |
| Herramientas de ladrón | Thieves' Tools |
| Mochila | Backpack |
| Antorcha | Torch |
| Cuerda/Soga | Rope |

---

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
