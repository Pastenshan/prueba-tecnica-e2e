# [cite_start]WindyCity Cabs - E2E Data Pipeline (Senior Technical Challenge) [cite: 1, 2]

[cite_start]Este proyecto implementa un flujo de datos completo para la empresa **WindyCity Cabs**, utilizando el dataset público de taxis de Chicago[cite: 2]. [cite_start]El objetivo es transformar datos crudos de una API pública en un producto de datos analítico que habilite la toma de decisiones financieras y operacionales[cite: 3, 4, 5].

## [cite_start]1. Arquitectura del Pipeline [cite: 4, 6]
El diseño sigue una arquitectura de capas para garantizar la trazabilidad y la calidad del dato:

```mermaid
graph TD
    API[Chicago Data Portal API] -->|Ingesta Incremental| RAW[(Capa RAW: JSON)]
    RAW -->|Limpieza y Tipado| STG[(Capa Staging: Parquet)]
    STG -->|Agregación de Negocio| GOLD[(Capa Gold: SQLite/MySQL)]
    GOLD -->|Exportación| BI[Looker Studio Dashboards]
	
	
Ingesta (Capa Raw): Descarga desde la API con paginación y filtrado por trip_start_timestamp (Watermark).
Procesamiento (Capa Staging): Limpieza de datos, manejo de tipos y eliminación de duplicados para asegurar idempotencia.
Modelo Analítico (Capa Gold): Creación de tablas agregadas (daily_kpis, zone_kpis, seasonal_kpis) para optimizar el rendimiento del BI.
Definición de Métricas de Negocio El sistema responde a las siguientes preguntas críticas de negocio:¿Cuál es la salud financiera diaria y su mix de pagos? 
Métrica: Revenue Total y % de Revenue por método de pago.¿Qué zonas presentan baja eficiencia operativa? 
Métrica: Propina promedio por milla recorrida (Tip/Mile).
¿Cómo influye la estacionalidad en la demanda? 
Métrica: Volumen de viajes por hora y día de la semana.
Dashboard,Audiencia,KPIs Principales,Decisiones que habilita
Dirección/Finanzas #1,CFO / Socios,"Total Revenue, Fare vs Tips",Evaluación de márgenes netos.
Dirección/Finanzas #2,Tesorería,Ticket promedio por Pago,Gestión de comisiones bancarias.
Operación #1,Gerente Ops,Viajes por Hora (Heatmap),Planificación de turnos punta.
Operación #2,Logística,Ranking de Zonas (Eficiencia),Estrategia de cobertura geográfica.

4. Calidad de Datos (MVP) 

Se implementaron los siguientes controles automatizados:

Unicidad: Validación de llave primaria mediante trip_id.

Integridad: Eliminación de registros con montos o distancias negativas.

Manejo de Duplicados: Lógica de filtrado en staging para evitar sobreconteo.

5. Decisiones Técnicas y Trade-offs 

Almacenamiento: Se utilizó SQLite en la fase de desarrollo para garantizar la reproducibilidad inmediata en Google Colab.

Uso de Tablas Agregadas: Se priorizó alimentar el BI con resúmenes pre-calculados para garantizar dashboards ligeros.

Python: Lenguaje obligatorio para asegurar la manipulación robusta de datos.

6. Uso de IA (Transparencia Obligatoria) 
Herramientas: Gemini 3 Flash.

Influencia: Diseño de arquitectura del pipeline, generación de scripts de transformación, depuración de errores y estructura de la documentación.

Prompts: "Diseña un flujo de ingesta incremental con watermark en Python", "Generar diagrama Mermaid para un pipeline ETL de 3 capas".

7. Backlog (Próximos Pasos) 

Migrar el almacenamiento a un clúster de MySQL productivo.

Implementar orquestación formal con Prefect o Airflow.

Añadir pruebas unitarias y observabilidad.

Instrucciones de Ejecución 

Clonar el repositorio.

Instalar dependencias: pip install pandas requests sqlalchemy pymysql.

Ejecutar el notebook: Tarea.ipynb
