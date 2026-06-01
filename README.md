# Análisis pangenómico de Botrytis: Selección de características genómicas asociadas a la especificidad de huésped

Este repositorio contiene el flujo de trabajo computacional (pipeline bioinformático y modelado estadístico) desarrollado para la caracterización molecular y la predicción de la especificidad de huésped en el género *Botrytis*. El flujo combina el preprocesamiento genómico a gran escala y el aprendizaje automático supervisado.

## Contenido del Repositorio

El repositorio se compone de un flujo de trabajo para análisis masivo en la nube y una serie de *scripts/notebooks* modulares para la interpretación predictiva local:

* **`Galaxy-Workflow-TFM.ga`**: Archivo de flujo de trabajo exportable para la plataforma bioinformática Galaxy. Centraliza de forma reproducible el pipeline de análisis genómico (comparaciones homólogas con OrthoFinder, anotaciones de ortología funcional mediante eggNOG-mapper y minería de clústeres de genes biosintéticos con antiSMASH).
* **Códigos Modulares (`códigos_tfm_módulo_X`)**: Conjunto de archivos (optimizados para su ejecución en entornos como Google Colab) que comprenden el post-procesado de datos, la reducción de redundancia proteica, la vectorización matricial y el modelado predictivo de *Machine Learning*.

---

## Estructura de Trabajo Computacional

### Fase 1: Preprocesamiento Genómico (Galaxy)

A partir de los ensamblajes genómicos iniciales en formato FASTA, el flujo de trabajo integrado en `Galaxy-Workflow-TFM.ga` realiza:

* La delimitación del espacio pangenómico y asignación de ortogrupos.
* La anotación funcional y asignación ontológica de las secuencias peptídicas deducidas.
* El análisis masivo en paralelo para la identificación de Clústeres de Genes Biosintéticos (BGCs).

### Fase 2: Procesamiento de Matrices y Aprendizaje Automático

El código en Python está estructurado de forma secuencial en los siguientes archivos independientes según su bloque analítico:

* **`códigos_tfm_módulo_1`**: Extracción y tipado de BGCs a partir de los *outputs* crudos comprimidos de antiSMASH, incluyendo filtros funcionales (Core, Accessory, Transporter, Regulator) e integración remota con la API de HMMER.
* **`códigos_tfm_módulo_2`**: Agrupamiento homólogo inter-cepa utilizando el algoritmo CD-HIT (`-c 0.7 -n 4`) y renderizado visual de sintenia genómica.
* **`códigos_tfm_módulo_3`**: Construcción y transformación algebraica de matrices de frecuencia pangenómica utilizando ponderación TF-IDF y detección de anomalías basales mediante Isolation Forest.
* **`códigos_tfm_módulo_4`**: Entrenamiento del clasificador multiclase XGBoost (`objective='multi:softprob'`) para predecir estilos de vida (generalistas vs. especialistas) y atribución causal de variables mediante valores SHAP.

---

## Requisitos del Entorno

Para la ejecución de los módulos de análisis estadístico y *Machine Learning*, se requieren las siguientes dependencias de Python:

* `biopython`
* `pandas`
* `numpy`
* `scikit-learn`
* `xgboost`
* `shap`
* `matplotlib`
* `seaborn`
* `requests`

---

Si necesitas que los archivos modulares lleven la extensión `.ipynb` (por ejemplo, `códigos_tfm_módulo_1.ipynb`) o `.py` explícitamente en el texto, dímelo y lo ajusto en un momento.
