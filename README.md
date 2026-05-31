# Análisis pangenómico de *Botrytis*: Selección de características genómicas asociadas a la especificidad de huésped

Este repositorio contiene el flujo de trabajo computacional (pipeline bioinformático y modelado estadístico) desarrollado para la caracterización molecular y la predicción de la especificidad de huésped en el género *Botrytis*. El flujo combina el preprocesamiento genómico a gran escala y el aprendizaje automático supervisado.

## Contenido del Repositorio

El repositorio se compone de dos archivos principales que cubren las fases de análisis masivo en la nube e interpretación predictiva local:

1. **`Galaxy-Workflow-TFM.ga`**: Archivo de flujo de trabajo exportable para la plataforma bioinformática Galaxy. Centraliza de forma reproducible el pipeline de análisis genómico (comparaciones homólogas con OrthoFinder, anotaciones de ortología funcional mediante eggNOG-mapper y minería de clústeres de genes biosintéticos con antiSMASH).
2. **`Códigos_TFM.ipynb`**: Jupyter Notebook (optimizado para su ejecución en Google Colab) que comprende el post-procesado de datos, la reducción de redundancia proteica por homología (CD-HIT), la vectorización matricial mediante TF-IDF y el modelado predictivo de Machine Learning (XGBoost, Isolation Forest e interpretabilidad mediante valores SHAP).

---

## Estructura de Trabajo Computacional

### Fase 1: Preprocesamiento Genómico (Galaxy)
A partir de los ensamblajes genómicos iniciales en formato FASTA, el flujo de trabajo integrado en `Galaxy-Workflow-TFM.ga` realiza:
* La delimitación del espacio pangenómico y asignación de ortogrupos.
* La anotación funcional y asignación ontológica de las secuencias peptídicas deducidas.
* El análisis masivo en paralelo para la identificación de Clústeres de Genes Biosintéticos (BGCs).

### Fase 2: Procesamiento de Matrices y Aprendizaje Automático (`Códigos_TFM.ipynb`)
El notebook de Python está estructurado de forma secuencial en los siguientes bloques analíticos:
* **Módulo 1:** Extracción y tipado de BGCs a partir de los outputs crudos comprimidos de antiSMASH, incluyendo filtros funcionales (Core, Accessory, Transporter, Regulator) e integración remota con la API de HMMER.
* **Módulo 2:** Agrupamiento homólogo inter-cepa utilizando el algoritmo CD-HIT (`-c 0.7 -n 4`) y renderizado visual de sintenia genómica.
* **Módulo 3:** Construcción y transformación algebraica de matrices de frecuencia pangenómica utilizando ponderación TF-IDF y detección de anomalías basales mediante Isolation Forest.
* **Módulo 4:** Entrenamiento del clasificador multiclase XGBoost (`objective='multi:softprob'`) para predecir estilos de vida (generalistas vs. especialistas) y atribución causal de variables mediante valores SHAP.

---

## Requisitos del Entorno

Para la ejecución del script de análisis estadístico y Machine Learning (`Códigos_TFM.ipynb`), se requieren las siguientes dependencias de Python:

```text
biopython
pandas
numpy
scikit-learn
xgboost
shap
matplotlib
seaborn
requests
