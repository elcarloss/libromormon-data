# Libro de Mormón — datos (español)

Snapshot del texto del **Libro de Mormón** en español, extraído del sitio oficial de
La Iglesia de Jesucristo de los Santos de los Últimos Días.

> El texto de las Escrituras es © Intellectual Reserve, Inc. Se usa con permiso
> para fines no comerciales. Para cualquier uso comercial, consulte la
> [política oficial](https://www.churchofjesuschrist.org/legal/terms-of-use).
> Fuente: `churchofjesuschrist.org/study/scriptures/bofm?lang=spa`.

Este repositorio **sólo contiene los datos** (textos y metadatos), no la aplicación.
La aplicación que los consume está en el repo hermano
[`elcarloss/libromormon`](https://github.com/elcarloss/libromormon).

## Contenido

```
.
├── README.md
├── LICENSE
├── summary.json            ← estadísticas globales
├── books.json              ← metadatos de los 15 libros (slug, nombre, abrev, capítulos)
├── verses.jsonl            ← un versículo por línea (streaming-friendly)
├── by-chapter/             ← un JSON por capítulo: 1-ne-001.json, alma-032.json, …
└── by-book/                ← un JSON por libro con índice de capítulos y metadatos
```

**Total**: 15 libros · 239 capítulos · 6 604 versículos.

## Esquema

### `books.json`
```json
[
  { "slug": "1-ne", "nombre": "1 Nefi", "abreviacion": "1 Ne.",
    "orden": 1, "capitulos": 22 },
  …
]
```

### `verses.jsonl` (un registro por línea)
```json
{
  "libro": "1 Nefi",
  "libro_slug": "1-ne",
  "abreviacion": "1 Ne.",
  "capitulo": 1,
  "versiculo": 1,
  "texto": "Yo, Nefi, nací de buenos padres …",
  "referencia": "1 Ne. 1:1",
  "url": "https://www.churchofjesuschrist.org/study/scriptures/bofm/1-ne/1.1?lang=spa",
  "sumario": "Nefi da principio a la historia… Aproximadamente 600 a.C."
}
```

### `by-chapter/<slug>-<NNN>.json`
```json
{
  "libro": "Alma",
  "libro_slug": "alma",
  "capitulo": 32,
  "titulo": "Alma 32",
  "sumario": "Alma enseña a los pobres…",
  "url": "https://…alma/32?lang=spa",
  "versiculos": [
    { "numero": 1, "texto": "…" },
    …
  ]
}
```

### `by-book/<slug>.json`
Índice de capítulos de un libro (sin texto, sólo metadatos).

## Uso

### Descargar todo con `git clone`

```bash
git clone https://github.com/elcarloss/libromormon-data.git
```

### Consumir un archivo con `curl` (raw)

```bash
curl -sL https://raw.githubusercontent.com/elcarloss/libromormon-data/main/by-chapter/alma-032.json | jq .
curl -sL https://raw.githubusercontent.com/elcarloss/libromormon-data/main/verses.jsonl | head -5
```

### Streaming del JSONL en Python

```python
import json, urllib.request
with urllib.request.urlopen("https://raw.githubusercontent.com/elcarloss/libromormon-data/main/verses.jsonl") as r:
    for line in r:
        v = json.loads(line)
        if "fe" in v["texto"].lower():
            print(v["referencia"], v["texto"][:90], "…")
```

### Slugs canónicos

`1-ne`, `2-ne`, `jacob`, `enos`, `jarom`, `omni`, `w-of-m`, `mosiah`, `alma`,
`hel`, `3-ne`, `4-ne`, `morm`, `ether`, `moro`.

## Cómo se generaron estos datos

Una sola descarga con `libromormon.api` (parser con soporte para
`<sup class="verse-number">` y el actual `<span class="verse-number">`).
Si el sitio oficial cambia de estructura, regenera los datos con:

```bash
python3 scrape_clean.py --force
```

(`scrape_clean.py` está en el repo hermano `elcarloss/libromormon/scripts/`.)

## Licencia

- **Texto de las Escrituras**: © Intellectual Reserve, Inc. — uso no comercial
  con atribución.
- **Estructura, scripts y documentación** de este repo: MIT — ver `LICENSE`.
