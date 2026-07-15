# diagnostico-de-salud-de-Maquinaria
Este repositorio contiene el desarrollo y las conclusiones de la evaluación de salud de maquinaria basada en datos de sensores (`sensor_operaciones.csv`).

📊 Práctica de Phyton: Análisis Operativo y Procesamiento de Telemetría en Bombas de Inyección Industrial
En este proyecto el objetivo fue procesar volúmenes variables de telemetría para luego clasificar los estados operativos, extraer las métricas de riesgo y finalmente se determino los factores críticos que provocan las fallas en equipos industriales.

🎯 1. Objetivos del Proyecto
El sistema automatiza el monitoreo de condiciones operativas mediante el procesamiento secuencial de: 
   
   * **Procesamiento de Flujo Lineal ($O(N)$):** Diseñar un algoritmo iterativo capaz de procesar archivos de datos de cualquier tamaño       sin saturar la memoria. 
    *   **Aislamiento Estadístico:** Segregar los registros de estrés hídrico y térmico (anomalías) para calcular métricas enfocadas          únicamente en las zonas de riesgo. 
    *   **Análisis de Ingeniería:** Evaluar las variables del sistema para determinar la causa raíz del desgaste crítico y proponer         mejoras basadas en el comportamiento físico de los equipos industriales. 
    
---

⚙️ 2. Arquitectura de Reglas Operacionales: 
La clasificación del estado de la maquinaria se realiza bajo un modelo matricial de tres niveles, priorizando siempre las condiciones de riesgo combinado.

--- 

🐍 3. Mapa del Flujo de Datos: 
El algoritmo está estructurado para optimizar la memoria y procesar de manera lineal (O(n)) filas de datos infinitas o desconocidas:
* **Lectura Secuencial:** Captura fila por fila del archivo transaccional de sensores.
* **Evaluación de Estado:** Ejecución de la lógica booleana sobre los parámetros de presión y temperatura.
* **Filtrado y Segregación:**
    * Si se detecta un estado Normal, se incrementa su contador.
    * Si se detecta Precaución o Crítico, se extraen los valores numéricos y se insertan dinámicamente en estructuras de almacenamiento aislado (Listas de Anomalías).
* **Agregación Estadística:** Cálculo final de porcentajes globales y promedios aritméticos enfocados únicamente en la zona de riesgo.

--- 

📈 4. Conclusiones y Métricas del Tablero: 
Tras el procesamiento completo del archivo de sensores, se obtuvieron las siguientes métricas clave de diagnóstico:
* **Distribución del Estado Frecuencial (Diccionario Conteo)**
El volumen principal de operación se mantiene en rangos seguros, aunque con un margen de alerta considerable que requiere atención de mantenimiento.
* **Indicadores de Desempeño en Condiciones de Riesgo**
Los cálculos finales derivados de las listas aisladas revelan las siguientes medias operacionales durante fallas y pre-fallas:
    * Porcentaje Total de Anomalías: Suma ponderada de eventos críticos y de precaución sobre el gran total de lecturas.
    * Promedio de Presión en Riesgo: Nivel medio de fuerza hidrodinámica registrada exclusivamente durante las desviaciones de umbral.
    * Promedio de Temperatura en Riesgo: Nivel térmico medio en el que la bomba experimentó sobrecarga.

* **Métricas Ejecutivas Consolidadas**
    * Diccionario de Conteo: {"Crítico": 11, "Precaución": 70, "Normal": 19}
    * Porcentaje de Anomalías: 81.00% (Suma de los estados de Precaución y Crítico).
    * Promedio de Presión (En Anomalía): 2733.47 psi
    * Promedio de Temperatura (En Anomalía): 136.76 °C
  
---

🧠 5. Análisis Técnico y de Campo (Ingeniería de Procesos) ¿Qué factor influye más para que la bomba entre en un estado crítico? 
Para clasificar al sistema en un Estado Crítico, la lógica matemática exige que se cumpla la condición combinada estricta: $Presión > 3000\text{ psi}$ y $Temperatura > 150\text{ °C}$. Sin embargo, evaluando el comportamiento físico de las variables y su comportamiento termodinámico, la Temperatura es el factor más determinante y peligroso en la operación real de campo.

---

🔍 6. Propuesta de Optimización: El Riesgo Oculto de la Cavitación: 
Como analisa de datos y de procesos, observe un vacío crítico de control en la lógica establecida por el algoritmo de software:
**El Problema:** El script se enfoca exclusivamente en la sobrepresión (valores por encima de 2500 psi). Sin embargo, en campo se registran caídas de presión extremas (cercanas a 1800 psi).
**El Fenómeno Físico:** Las caídas repentinas de presión en bombas de inyección suelen ser el síntoma principal de la cavitación (formación y colapso de burbujas de vapor dentro del fluido). La cavitación erosiona las paredes de la bomba, causa vibraciones destructivas y destruye el impulsor rápidamente.
**La Limitación del Script:** Actualmente, el algoritmo clasifica estas presiones críticas de 1800 psi como Normales, enmascarando un riesgo físico severo en los tableros de control.

* **Propuesta de Mejora (Clasificación Multivariable)** Se recomienda actualizar el script en una Versión 2.0 que evalúe umbrales mínimos de presión:
            $$\text{Si } Presión < 2000\text{ psi} \rightarrow \text{Alerta de Cavitación (Riesgo Crítico)}$$
