# Resolución del Set Covering Problem (SCP) mediante Simulated Annealing

Implementación de la metaheurística de **Simulated Annealing (SA)** para resolver el
*Set Covering Problem* (SCP), siguiendo la heurística de búsqueda local de
**Jacobs & Brusco (1995)**. Proyecto de la **Tarea 2** del curso de Investigación de
Operaciones (Metaheurísticas), Profesora Leslie Pérez Cáceres.

El algoritmo trabaja sobre una representación binaria de la solución y un vecindario de
*remoción y reparación* parametrizado por `ρ1` y `ρ2`, con criterio de aceptación de
Metropolis y enfriamiento geométrico. Se evalúan tres instancias de complejidad creciente
con 10 réplicas independientes y reproducibles por instancia.

---

## Integrantes

| Nombre | RUT |
|--------|-----|
| Eva Ponce | 21643488-9 |
| Eduardo Sandoval | 21951263-5 |
| Juan Geraldo | 20180417-5 |

---

## Estructura del repositorio

```
.
├── codigo_SCP_SA.ipynb              # Notebook con la implementación completa del SA
├── Set_Covering_Problem_SA.pdf      # Informe final
├── instancias/
│   ├── 01_facil.txt                 # Instancia 1 (n=50,  m=500)
│   ├── 02_medio.txt                 # Instancia 2 (n=200, m=1000)
│   └── 03_dificil.txt               # Instancia 3 (n=200, m=2000)
├── figuras/                         # Figuras de diagnóstico y comparación (opcional)
└── README.md
```

> Ajusta esta estructura a la organización real de tu repositorio. Las instancias se
> proporcionan en el aula virtual del curso.

---

## Requisitos

- **Python 3.8 o superior.**
- **Biblioteca estándar** (no requiere instalación): `random`, `math`, `time`, `sys`,
  `statistics`. El núcleo del algoritmo y el cálculo de estadísticas corren solo con esto.
- **matplotlib** (únicamente para generar las figuras de diagnóstico y comparación):

```bash
pip install matplotlib
```

El código **no instala nada** ni descarga dependencias externas.

---

## Instrucciones de ejecución

El notebook espera las instancias en las rutas `/content/01_facil.txt`,
`/content/02_medio.txt` y `/content/03_dificil.txt` (rutas de Google Colab).

### Opción A — Google Colab (recomendada)

1. Abrir `codigo_SCP_SA.ipynb` en Google Colab.
2. Subir las tres instancias (`01_facil.txt`, `02_medio.txt`, `03_dificil.txt`) al
   directorio `/content/` mediante el panel de archivos (icono de carpeta → *Subir*).
3. Ejecutar todas las celdas en orden: **Entorno de ejecución → Ejecutar todo**.

### Opción B — Jupyter / VS Code local

1. Tener Python 3.8+ y, opcionalmente, `matplotlib` instalado.
2. Colocar las instancias en `/content/` **o** editar las rutas en la celda principal
   (las llamadas a `main(...)` y `diagnostico_metaheuristica(...)`) y en el diccionario
   `INSTANCIAS` de la sección GRÁFICAS, apuntándolas a la ubicación local
   (por ejemplo `instancias/01_facil.txt`).
3. Ejecutar todas las celdas en orden, de arriba hacia abajo.

### ¿Qué hace cada parte?

- **PARTE A** — estructuras de datos binarias (lectura de instancias, función objetivo,
  factibilidad).
- **PARTE B** — metaheurística: módulos `CONSTRUCT`, `SEARCH` (`generar_vecino`) y el ciclo
  `recocido_simulado`.
- **PARTE C** — `main(ruta)`: ejecuta las **10 corridas** de una instancia e imprime las
  funciones objetivo, la **media**, la **desviación estándar**, el mínimo/máximo y la mejor
  solución encontrada.
- **PARTE D** — `diagnostico_metaheuristica(ruta)`: vuelve a ejecutar con telemetría y
  guarda la figura de diagnóstico (`diagnostico_sa.png`): convergencia, trayectoria de
  Metropolis e histograma de la construcción inicial.
- **GRÁFICAS** — genera la figura comparativa del proceso entre instancias
  (`comparacion_proceso.png`). Por defecto usa telemetría embebida
  (`USAR_TELEMETRIA_EMBEBIDA = True`); para recalcularla desde cero dentro del notebook,
  cambiar a `False`.

### Ejecutar una sola instancia

```python
main("/content/02_medio.txt")                 # 10 corridas + estadísticas
diagnostico_metaheuristica("/content/02_medio.txt")   # figura de diagnóstico
```

La reproducibilidad está garantizada: cada corrida usa la semilla `12345 + c`.

---

## Parámetros utilizados

| Parámetro | Valor | Rol |
|-----------|-------|-----|
| `ρ1` | 0.4 | Fracción de subconjuntos a retirar (magnitud del vecindario) |
| `ρ2` | 2.0 | Cota de costo para reingreso (profundidad de la reparación) |
| `T0` | 1.3 | Temperatura inicial |
| `CF` | 0.9 | Factor de enfriamiento geométrico |
| `TL` | 100 | Iteraciones por nivel de temperatura |
| `max_evaluaciones` | 1000 | Presupuesto de cómputo (criterio de parada) |
| `max_tiempo` | 60 s | Criterio de parada alternativo |
| `num_corridas` | 10 | Réplicas independientes por instancia |
| `semilla_base` | 12345 | La corrida `c` usa la semilla `12345 + c` |

---

## Resultados esperados

| Instancia | n | m | Mejor solución Z(S\*) | Desv. estándar |
|-----------|---|---|-----------------------|----------------|
| 01_facil | 50 | 500 | 5 | 0.0 |
| 02_medio | 200 | 1000 | 433 | 0.0 |
| 03_dificil | 200 | 2000 | 269 | 0.0 |

Las 10 corridas de cada instancia convergen al mismo valor óptimo (desviación estándar
nula), lo que evidencia la estabilidad del método.

---

## Referencia

Jacobs, L. W., & Brusco, M. J. (1995). A local-search heuristic for large set-covering
problems. *Naval Research Logistics, 42*(7), 1129–1140.
