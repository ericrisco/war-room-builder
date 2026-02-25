# PRD — war-room-builder
**Versión:** 0.1  
**Estado:** Draft  
**Repositorio target:** github.com/[org]/war-room-builder  
**Distribuible:** `.skill` para Skills.sh

---

## 1. Resumen

`custom-war-room` es una skill de Claude que permite dos cosas:

1. **Crear** un war room personalizado: genera los tres perfiles de expertos y los archivos de configuración en una carpeta local reutilizable.
2. **Llamar** a un war room ya existente referenciando su carpeta, activando el debate con los perfiles precargados.

El resultado puede ser **efímero** (debate de una sola tirada, sin guardar nada) o **persistente** (carpeta con perfiles que puede invocarse en futuras sesiones).

---

## 2. Problema que resuelve

La skill `panel-guerra` actual genera el debate en el momento pero no tiene memoria entre sesiones. Si quieres repetir el mismo panel (mismos expertos, mismo contexto de marca, mismo dominio), tienes que reconfigurarlo cada vez. `custom-war-room` externaliza los perfiles a archivos, haciendo el panel reproducible, versionable y compartible.

---

## 3. Casos de uso

| Caso | Descripción |
|------|-------------|
| **One-shot** | El usuario pasa un tema, el sistema propone o acepta perfiles, debate, fin. No se guarda nada. |
| **War room persistente** | El usuario crea una carpeta con perfiles definidos. En futuras sesiones invoca `@mi-war-room` y el debate arranca con esos perfiles sin reconfigurar. |
| **War room de equipo** | La carpeta se versiona en git y se comparte. Todo el equipo llama al mismo panel con los mismos perfiles. |
| **War rooms múltiples** | Un usuario tiene varios war rooms para distintos contextos: `brand-war-room`, `tech-war-room`, `copy-war-room`. Cada uno con sus tres expertos propios. |

---

## 4. Estructura de archivos

```
custom-war-room/                  ← repositorio / skill base
├── SKILL.md                      ← instrucciones base para Claude
├── scripts/
│   └── create_war_room.py        ← scaffolding de nueva carpeta
└── references/
    └── profile-template.md       ← plantilla de perfil de experto

[nombre-del-war-room]/            ← carpeta generada por el usuario
├── war-room.config.json          ← metadata: nombre, tema, modo
├── profiles/
│   ├── expert-1.md               ← perfil experto 1
│   ├── expert-2.md               ← perfil experto 2
│   └── expert-3.md               ← perfil experto 3
└── SKILL.md                      ← skill autónoma que referencia los perfiles
```

---

## 5. Flujo de usuario

### 5a. Crear un war room nuevo

```
Usuario: "crea un war room para analizar estrategia de contenido en LinkedIn"

Claude:
  1. Detecta intención de crear (no de debatir)
  2. Pregunta: ¿tienes perfiles en mente o te los propongo?
  3. Propone o acepta tres perfiles con tensión natural entre ellos
  4. Pregunta: ¿modo efímero (solo este debate) o persistente (guardar carpeta)?
  5a. Efímero → arranca el debate directamente, fin
  5b. Persistente → genera la carpeta con los archivos, confirma, arranca
```

### 5b. Llamar a un war room existente

```
Usuario: "activa mi war room en ./brand-war-room sobre esta imagen"

Claude:
  1. Lee war-room.config.json
  2. Carga los tres expert-N.md
  3. Presenta los perfiles brevemente
  4. Arranca el debate con el material que el usuario ha pasado
```

### 5c. One-shot sin persistencia

```
Usuario: "panel de guerra sobre esto" (sin mencionar war room)

Claude:
  → Cae en MODO B del panel-guerra base
  → Debate directo, nada se guarda
```

---

## 6. Formato de los archivos generados

### `war-room.config.json`
```json
{
  "name": "brand-war-room",
  "topic": "estrategia de contenido LinkedIn",
  "mode": "persistent",
  "created": "2026-02-25",
  "experts": ["expert-1.md", "expert-2.md", "expert-3.md"],
  "max_rounds": 10,
  "language": "es"
}
```

### `profiles/expert-N.md`
```markdown
# [Nombre del experto]

## Identidad
Quién es, por qué es icónico en este dominio.

## Obsesiones
Qué le quita el sueño. Qué defiende a muerte.

## Manera de hablar
Cómo argumenta. Sus muletillas. Su ritmo.

## Puntos ciegos
Qué no ve aunque lo tenga delante.

## Por qué choca con los otros dos
Tensión específica con experto-2 y experto-3.

## Agenda oculta
Qué quiere realmente más allá del debate.
```

### `SKILL.md` (dentro de la carpeta del war room)
Skill autónoma que:
- Referencia los tres perfiles de su carpeta
- Contiene las reglas de debate
- Se puede instalar directamente como skill independiente en Claude

---

## 7. Reglas de debate (invariantes en todos los modos)

- Se interrumpen sin pedir permiso
- Se citan mal entre ellos a propósito
- Cambian de posición si el argumento lo justifica
- Forman alianzas que rompen cuando conviene
- Tienen agendas ocultas que afloran con las rondas
- Debate hasta consenso genuino, máximo 10 rondas
- Si hay consenso antes, se acaba antes
- Si llegan a la 10 sin acuerdo: tregua honesta con concesiones explícitas

**Veredicto final:** cada experto declara su posición final y qué ha tenido que tragar.

---

## 8. Selección de perfiles — jerarquía

1. **Famosos reales mundialmente reconocibles** cuyas filosofías choquen de verdad → primera opción siempre
2. **Arquetipos ficticios hiperespecíficos** si no hay famosos adecuados → nunca genéricos

Test de validación de perfiles: *¿estas tres personas se odiarían profesionalmente?* Si no, cambiar.

---

## 9. Componentes técnicos

### `scripts/create_war_room.py`
Script Python que:
- Recibe nombre, tema, lista de perfiles (JSON o argumentos)
- Crea la estructura de carpetas
- Genera los archivos con el contenido proporcionado por Claude
- Output: ruta de la carpeta creada

### `references/profile-template.md`
Plantilla que Claude usa para generar cada `expert-N.md` de forma consistente.

### `SKILL.md` base
Instrucciones para Claude sobre cómo:
- Detectar el modo (crear / llamar / one-shot)
- Generar perfiles
- Invocar el script de scaffolding
- Leer una carpeta existente y arrancar el debate

---

## 10. Interfaz de activación (frases que disparan la skill)

| Frase | Modo |
|-------|------|
| "crea un war room para X" | Crear persistente |
| "war room efímero sobre X" | One-shot |
| "activa mi war room en ./carpeta" | Llamar existente |
| "panel de guerra sobre X" | One-shot (herencia panel-guerra) |
| "carga el war room de marca" | Llamar existente (búsqueda por nombre) |

---

## 11. Distribución como `.skill`

El repositorio genera un `.skill` instalable via Skills.sh que incluye:
- `SKILL.md` base
- `scripts/create_war_room.py`
- `references/profile-template.md`

Las carpetas de war rooms personalizadas **no** se incluyen en el `.skill` — son locales del usuario y pueden versionarse en su propio repo.

---

## 12. Estructura del repositorio GitHub

```
custom-war-room/
├── README.md
├── SKILL.md
├── LICENSE
├── scripts/
│   └── create_war_room.py
├── references/
│   └── profile-template.md
├── examples/
│   ├── brand-war-room/          ← ejemplo de war room persistente
│   └── tech-war-room/           ← otro ejemplo
└── .github/
    └── workflows/
        └── package-skill.yml    ← genera el .skill en cada release
```

---

## 13. Criterios de aceptación

- [ ] `SKILL.md` base detecta correctamente los tres modos sin preguntar al usuario cuál quiere
- [ ] En modo crear, genera los tres perfiles con tensión validable
- [ ] En modo llamar, carga los perfiles de la carpeta y arranca sin reconfiguración
- [ ] En modo one-shot, debate sin generar archivos
- [ ] `create_war_room.py` genera la estructura correcta de carpetas y archivos
- [ ] La SKILL.md dentro del war room generado funciona como skill autónoma instalable
- [ ] El `.skill` resultante se instala correctamente via Skills.sh
- [ ] Los ejemplos en `/examples` son funcionales y demostrables

---

## 14. Fuera de alcance (v0.1)

- UI gráfica para gestionar war rooms
- Sincronización automática con carpetas remotas
- Memoria entre debates del mismo war room
- Más de tres expertos por war room