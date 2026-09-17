# 1.Entorno de Análisis de Datos Profesional

## El orden previo garantiza el éxito del resultado

# 1.1. Arquitectura y Configuración del Entorno

Para garantizar reproducibilidad, portabilidad y seguridad, el proyecto sigue las mejores prácticas de la industria en Visual Studio Code (https://code.visualstudio.com/):

## Workspace (`pre.code-workspace`):

Mesa de trabajo unificada. Permite organizar carpetas de datos, notebooks y código fuente bajo una misma vista, guardando reglas de visualización y configuración de extensiones.

## Entorno Virtual (`.venv/`):

Contenedor aislado de dependencias. Evita colisiones entre librerías instaladas globalmente en el sistema operativo y las necesarias para este proyecto. _(Excluido en `.gitignore`)_.

## Manifiesto de Dependencias (`requirements.txt`):

Lista exacta de librerías y versiones para replicar el entorno con un solo comando (`pip install -r requirements.txt`).

## Variables de Entorno y Secretos (`.env` vs `.env.example`)

- `.env`: Almacena credenciales, tokens o rutas de archivos locales en la máquina actual. _(Protegido en `.gitignore`)_.
- `.env.example`: Plantilla pública versionada en Git para indicar qué variables requiere el sistema.

## Manejo de Rutas y Secretos en Python (`os`, `load_dotenv` y `os.getenv`):

- `python-dotenv` (`load_dotenv`): Carga los pares clave-valor del archivo `.env` en la memoria del proceso al iniciar.
- `os.getenv("VARIABLE")`: Función segura para consultar las variables inyectadas sin romper el programa si la variable no existe (retornando `None` o un valor por defecto).
- `os.path.join()`: Construye rutas compatibles tanto con Windows (`\`) como con Linux/macOS (`/`), evitando rutas fijas (_hardcodeadas_)..

---

# 1.2. Analogía

Para entender por qué cada componente existe y qué rol cumple en el código, podemos pensar el proyecto como la preparación de una cena de alta cocina en casa. Si un chef cocina con la mesada llena de cosas del desayuno, ingredientes mezclados de compras viejas y gritando las claves de su despensa en el comedor, el resultado será caótico e imposible de repetir.

<u><b>Workspace (pre.code-workspace)</b></u>

Analogía: Es la mesada de trabajo principal, despejada y zonificada antes de cocinar. En lugar de tener los ingredientes dispersos por toda la casa, el chef delimita su estación: la tabla de picar en el centro, el recetario fijado a la altura de los ojos y los recipientes organizados por función.

En la aplicación: Es el archivo de configuración que unifica el proyecto en Visual Studio Code. Permite abrir y visualizar bajo una misma ventana las carpetas data/ (datos crudos), src/ (código fuente), los notebooks de análisis y la documentación. Guarda las configuraciones de la interfaz, qué extensiones deben activarse para este trabajo y cómo se presentan los paneles, evitando mezclar este proyecto con otros que tengas en tu computadora.

<u><b>Entorno Virtual (.venv/)</b></u>

Analogía: Una caja organizadora con insumos apartados exclusivamente para esta receta. En una alacena compartida por varias personas, alguien puede usar tu aceite para freír o reemplazar una especia por otra más vieja. Al apartar tus frascos en una caja hermética propia, garantizás que nada externo contamine tu preparación ni que tus ingredientes afecten las comidas de los demás.

En la aplicación: Es un directorio aislado que contiene un intérprete de Python propio y las versiones exactas de las librerías necesarias (como pandas, numpy o jupyter). Su objetivo es evitar "colisiones" de dependencias: si otro proyecto de tu máquina requiere una versión más vieja o más nueva de una librería, el entorno virtual asegura que este proyecto funcione de forma independiente y estable. Se excluye del repositorio (.gitignore) porque cada computadora debe generar su propio contenedor.

<u><b>Manifiesto de Dependencias (requirements.txt)</b></u>

Analogía: La lista de compras precisa con marcas, calidades y gramajes exactos. No dice simplemente "comprar queso y harina", sino "Harina 000 marca X (1 kg), Levadura seca en sobre (10 g)". Si le entregás esa lista a otra persona, irá al mercado y conseguirá la base exacta para que el plato final tenga el mismo sabor y consistencia.

En la aplicación: Es un archivo de texto plano que lista formalmente cada paquete instalado y su número de versión (por ejemplo, pandas==2.2.0). Permite que cualquier colega o evaluador clone tu repositorio y, con una sola orden en la terminal (pip install -r requirements.txt), descargue exactamente el mismo software sin tener que adivinar qué librerías usaste.

<u><b>Variables de Entorno y Secretos (.env vs .env.example)</b></u>

Analogía:

.env: La combinación del candado de la cava de vinos escrita en un papelito guardado en tu bolsillo. Solo vos tenés acceso a esa bodega privada mientras cocinás.

.env.example: El renglón de la receta pública que aclara: "Para este paso se requiere: CLAVE_BODEGA=tu_clave_aqui". Cualquiera que lea la receta sabe que necesita una clave de acceso, pero jamás se entera de cuál es la combinación privada de tu casa.

En la aplicación:

.env: Archivo local que almacena información sensible, rutas específicas de tu máquina o tokens de autenticación. Jamás se sube al control de versiones (queda bloqueado por .gitignore) para no exponer datos confidenciales en la web.

.env.example: Plantilla de texto que sí se sube a GitHub. Sirve como instructivo público para indicar los nombres de las variables que el código espera encontrar configuradas localmente.

<u>Manejo de Rutas y Secretos en Python (os, load_dotenv y os.getenv)</u>

Analogía:

load_dotenv: El momento exacto en que sacás el papelito del bolsillo al entrar a la cocina y memorizás los números para el servicio del día.

os.getenv: El acto de consultar mentalmente esa clave frente al candado. Si por alguna razón la cava está vacía, el chef no entra en crisis ni cancela la cena; el procedimiento contempla usar un vino de la mesa por defecto o continuar la cocción sin frenar la cocina.

os.path.join: Dar directivas de movimiento relativas al espacio ("el frasco está dos estantes por encima de la mesada") en vez de usar coordenadas geográficas fijas de tu casa. De esa forma, las instrucciones funcionan en cualquier cocina del mundo.

En la aplicación:

load_dotenv(): Función de la librería python-dotenv que busca el archivo .env en la raíz del proyecto e inyecta esas configuraciones en la memoria del proceso de ejecución de Python.

os.getenv("VARIABLE"): Método seguro para leer una variable del entorno en memoria. A diferencia del acceso directo por diccionario (os.environ["VARIABLE"]), si la variable no existe, no interrumpe el script con un error fatal (KeyError), sino que devuelve un valor seguro (None o un valor predeterminado que definas).

os.path.join(): Utilidad del módulo os que une partes de una ruta de archivos usando el separador nativo del sistema operativo donde se ejecuta el código (\ en Windows, / en Linux y macOS). Esto evita errores de rutas fijas (hardcodeadas) y garantiza que el pipeline cargue los datasets correctamente sin importar si corre en tu máquina local o en un servidor.1.2.

---

# 1.3. Estructura del Repositorio

```text
pre/
├── .venv/                   # Entorno virtual local (ignorado en git)
├── data/                    # Datasets en crudo
│   ├── clientes.csv
│   ├── marketing.csv
│   └── ventas.csv
├── src/                     # Código fuente y scripts auxiliares
├── .env                     # Variables de entorno locales (ignorado en git)
├── .env.example             # Plantilla pública de variables de entorno
├── .gitignore               # Reglas de exclusión para Git (.venv, .env, etc.)
├── pre.code-workspace       # Configuración del espacio de trabajo en VS Code
├── preentrega_1.ipynb       # Notebook con el análisis y pipeline de datos
├── README.md                # Documentación del proyecto
└── requirements.txt         # Dependencias del proyecto

```

---

# 2.Dataset: df_ventas (Transacciones)

| Dimensión Inicial | Dimensión Final | Filas Descartadas |
| ----------------- | --------------- | ----------------- |
| 3,000 registros   | 2,998 registros | 2 filas (0.067%)  |

- **Detección y Eliminación de Duplicados:** Se auditaron registros idénticos a nivel de fila completa mediante `.duplicated().sum()` y se removieron filas redundantes para evitar sesgos en el volumen transaccional.
- **Normalización de Tipos de Datos:**
  - **Moneda a Numérico (`precio`):** Se limpiaron los caracteres alfanuméricos y símbolos monetarios, casteando la columna a tipo `float64`.
  - **Parseo Temporal (`fecha_venta`):** Se transformó de texto plano a formato canónico `datetime64[ns]` para permitir agrupaciones temporales.
- **Tratamiento de Valores Faltantes (Missing Values):**
  - Se identificaron 2 valores nulos en `precio` y 2 en `cantidad`.
  - Se comprobó que ambos nulos coincidían en las mismas transacciones (`id_venta`: 627 y 171).
  - Al no contar con precio ni volumen vendido, y al representar menos del 0.07% del dataset, se descartó la imputación heurística y se procedió a la eliminación mediante `.dropna()`, consolidando **2,998 transacciones válidas**.

# 3.Dataset: df_clientes

- **Dimensiones:** 567 filas × 5 columnas.
- **Auditoría Estructural:** 0 duplicados y 0 valores nulos en todas las dimensiones (`id_cliente`, `nombre`, `edad`, `ciudad`, `ingresos`).
- **Coherencia de Tipos:** Identificadores en `int64`, campos categóricos en `str` e ingresos salariales en `float64`. Sin necesidad de modificaciones.

# 4.Dataset: df_marketing

- **Dimensiones:** 90 filas × 6 columnas.
- **Auditoría Estructural:** 0 duplicados y 0 valores nulos.
- **Normalización Temporal:** Las columnas `fecha_inicio` y `fecha_fin` se encontraban en formato texto (`str`). Se ejecutó la conversión a `datetime64[ns]` utilizando el parámetro `dayfirst=True`.

# 5.Métrica de ingreso por transacción

Se genera la métrica analítica total_venta como variable clave para el análisis estadístico y de negocio de la compañía.

Esta columna permitirá calcular ingresos totales consolidados, el ticket promedio por transacción e identificar la distribución de ventas por producto, categoría o períodos temporales.

```python
# Creación de la métrica de ingreso por transacción
df_ventas['total_venta'] = (df_ventas['precio'] * df_ventas['cantidad']).round(2)

```

# 6. Estado Final de los Datasets

Validación de la integridad y dimensiones de las tres estructuras

```python
df_ventas:    (2998, 7)  | Columnas: ['id_venta', 'producto', 'precio', 'cantidad', 'fecha_venta', 'categoria', 'total_venta']
df_clientes:  (567, 5)   | Columnas: ['id_cliente', 'nombre', 'edad', 'ciudad', 'ingresos']
df_marketing: (90, 6)    | Columnas: ['id_campanha', 'producto', 'canal', 'costo', 'fecha_inicio', 'fecha_fin']

---
```

# Glosario (en desarrollo)

## Pipeline

En este contexto, un pipeline (tubería o canal de procesamiento) es la secuencia ordenada y automatizada de pasos que transforman los datos desde su estado original "en bruto" hasta dejarlos limpios, estructurados y listos para ser analizados.

Pensalo como una línea de montaje de fábrica: los datos entran crudos por un extremo y salen depurados por el otro.

En tu proyecto, el pipeline abarca exactamente estas etapas sucesivas:

<u><b>Extracción/Carga</b></u>: Lectura de los archivos CSV originales (ventas.csv, clientes.csv, marketing.csv).

<u><b>Auditoría e Inspección</b></u>: Detección de valores faltantes (NaN), duplicados e inconsistencias de tipos de datos.

<u><b>Limpieza y Normalización</b></u>: Casteo de tipos (ej. parseo a fechas con dayfirst=True, monedas a float64) y eliminación de registros inválidos con .dropna().

<u><b>Transformación e Ingeniería de Atributos</b></u>: Creación de nuevas métricas de negocio (como calcular total_venta).

<u><b>Salida/Consolidación</b></u>: Obtención de los dataframes limpios listos para modelar, visualizar o calcular KPIs.

Se le llama pipeline porque cada transformación depende de la anterior: la salida del paso 1 alimenta directamente al paso 2.
