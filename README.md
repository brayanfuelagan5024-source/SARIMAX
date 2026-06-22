# SARIMAX_REPLICA_ECUADOR

Proyecto reproducible, multiagente y subagente para replicar un modelo SARIMAX/ARIMAX de generación hidroeléctrica mensual en Ecuador y desplegar resultados mediante un dashboard en Vercel.

## Paper base

Barzola-Monteses, J., Martínez-López, M., Espinoza-Andaluz, M., Gómez-Romero, J., & Fajardo, W. (2019). *Time Series Analysis for Predicting Hydroelectric Power Production: The Ecuador Case*. **Sustainability, 11**(23), 6539. DOI: `10.3390/su11236539`.

El paper usa datos mensuales 2000–2015 de producción hidroeléctrica y precipitación. Estima con 2000–2014, valida con 2015 y reporta como mejor pronóstico un ARIMAX `(1,1,1)(1,0,0)[12]` con precipitación de la cuenca Napo.

Consulte `references/PRIMARY_PAPER.md` para el protocolo de réplica y sus límites.

## Abrir en VS Code

```bash
unzip SARIMAX_REPLICA_ECUADOR.zip
cd SARIMAX_REPLICA_ECUADOR
code .
```

En la terminal integrada:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

## Agente de literatura

```bash
python scripts/literature_agent.py --config config/literature_search.yaml
```

Genera una matriz CSV, una lista corta en Markdown y resultados JSON auditables consultando Crossref y OpenAlex. Los registros quedan como `metadata_only` hasta que el subagente validador revise el texto completo.

## Datos requeridos

- Generación hidroeléctrica mensual: fuente original ARCONEL, 2000–2015.
- Precipitación mensual por estación: INAMHI, cuencas Napo, Pastaza y Santiago.

Las plantillas y reglas están en `data/raw/hydropower/` y `data/raw/precipitation/`. Si las series originales no se recuperan, el proyecto debe declararse réplica reconstruida o conceptual.

## Ejecutar la réplica

Cuando exista el dataset analítico:

```bash
python scripts/run_pipeline.py --config config/paper_hydropower_ecuador.yaml
python scripts/export_dashboard.py
```

El caso sintético permite comprobar el software mientras llegan los datos oficiales:

```bash
python scripts/generate_demo_data.py
python scripts/run_pipeline.py --config config/demo.yaml
```

## Dashboard

```bash
cd dashboard
npm install
npm run dev
```

Para Vercel, conecte el repositorio GitHub y seleccione `dashboard` como **Root Directory**.

## Publicar en GitHub

```bash
git init
git add .
git commit -m "feat: initialize Ecuador hydropower SARIMAX replication"
git branch -M main
git remote add origin git@github.com:USUARIO/sarimax-replica-ecuador.git
git push -u origin main
```

## Estructura principal

- `PROJECT.md`: alcance y reglas científicas.
- `tasks/TASKS.md`: backlog completo.
- `agents/`: agentes y subagentes.
- `config/`: configuración de réplica y literatura.
- `scripts/`: adquisición, búsqueda, modelación y exportación.
- `data/`: datos brutos, intermedios y procesados.
- `references/`: paper base, matriz y registros de búsqueda.
- `paper/`: manuscrito Quarto y bibliografía.
- `dashboard/`: aplicación Next.js para Vercel.
- `.github/workflows/`: integración continua.

## Principio rector

Una cifra que no puede rastrearse no es un resultado; es decoración estadística.
