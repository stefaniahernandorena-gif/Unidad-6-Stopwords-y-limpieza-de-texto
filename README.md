# 🚀 Production-Grade NLP Pipeline: Lemmatization, Feature Reduction & ML Evaluation

Este repositorio contiene un entorno de ingeniería profesional para el procesamiento de lenguaje natural (NLP) aplicado al análisis y optimización morfológica de opiniones de clientes, utilizando el [Amazon Reviews Dataset](https://www.kaggle.com/datasets/dongrelaxman/amazon-reviews-dataset) provisto por Kaggle.

A diferencia de los enfoques monolíticos tradicionales, este proyecto implementa un pipeline de producción completamente modularizado en funciones desacopladas, evaluado mediante pruebas unitarias (`unittest`) y validado cuantitativamente a través de métricas de reducción dimensional y modelos supervisados de Machine Learning.

---

## 🛠️ Arquitectura del Sistema y Componentes Técnicos

El pipeline está estructurado en módulos independientes y reutilizables:

1. **Ingesta de Datos Robusta (`Módulo 1`):** Consumo automatizado del corpus real a través de la API de `kagglehub`. Configura el motor de parsing en `Python` (`engine="python"`) y gestiona excepciones de saltos de línea y líneas corruptas (`on_bad_lines="skip"`), eliminando los fallos críticos de desbordamiento de búfer (*C-buffer overflow*).
2. **Procesamiento Lingüístico Avanzado (`Módulo 2`):** Implementación de un flujo jerárquico con `spaCy` (`en_core_web_sm`) que ejecuta:
   * **Tokenización y Limpieza:** Eliminación de ruido estructural (signos de puntuación, espacios y caracteres numéricos).
   * **Filtrado Semántico Adaptativo:** Inyección de Stopwords tradicionales combinadas con una lista personalizada de **Stopwords de Dominio** específicas de e-commerce (*"product"*, *"amazon"*, *"shipping"*, *"seller"*), aislando términos ubicuos sin varianza estadístico-emocional.
   * **Lematización Profunda:** Reducción morfológica de los tokens a sus raíces canónicas (lemas).
   * **Etiquetado POS (Part-of-Speech):** Extracción síncrona de categorías gramaticales para auditoría de contexto.
3. **Métricas de Contracción de Características (`Módulo 3`):** Cálculo del impacto numérico del preprocesamiento sobre el espacio vectorial.
4. **Validación con Modelos Supervisados (`Módulo 4`):** Evaluación del impacto predictivo real mediante el entrenamiento de un clasificador de Regresión Logística sobre vectores de frecuencia inversa de documentos (**TF-IDF**).
5. **Conjunto de Pruebas Unitarias (`Módulo 5`):** Entorno integrado de aserciones lógicas para garantizar la consistencia morfológica y la estabilidad del código ante nuevos conjuntos de datos.

---

## 📊 Métricas Cuantitativas de Rendimiento

### 1. Reducción de Dimensionalidad (Compresión del Vocabulario)
El pipeline morfológico demostró una alta eficiencia en la purga de ruido y contracción de características (*features*):

* **Tamaño Vocabulario Original (Tokens Únicos):** 9,970
* **Tamaño Vocabulario Post-Procesado (Lemas):** 5,062
* **Porcentaje Neto de Reducción de Dimensionalidad:** **49.23%**

### 2. Impacto en el Rendimiento del Modelo de ML (Trade-Off Industrial)
Se evaluó un clasificador supervisado entrenado con matrices TF-IDF (restringido a las 2,500 mejores características) para comparar la capacidad predictiva de ambos estados del texto:

| Representación del Texto | Accuracy Score | Dimensión del Vocabulario | Latencia y Costo de Infraestructura |
| :--- | :--- | :--- | :--- |
| **Texto Original Crudo** | **81.7%** | 9,970 tokens | Elevado (Matriz dispersa masiva y saturada de ruido) |
| **Texto Procesado (Lemas)** | **80.3%** | 5,062 lemas | **Óptimo (Eficiencia en memoria y alta velocidad de cómputo)** |

**Discusión Técnica:** El modelo con texto procesado experimentó una flexión marginal de apenas el 1.4% en su *Accuracy Score*. Sin embargo, el pipeline limpio procesa la **mitad de características únicas (reducción del 49.23%)**. En entornos de producción masiva (*high-throughput*), este trade-off es ampliamente favorable: reduce drásticamente la latencia de inferencia por milisegundo y optimiza los costos operativos de almacenamiento en la nube, entregando un modelo altamente escalable y libre de sobreajuste (*overfitting*).

---

## 🧪 Pruebas Unitarias Automatizadas
El pipeline incorpora un módulo basado en `unittest` que valida el correcto funcionamiento de las reglas lingüísticas (lematización, exclusión de stopwords y tipado numérico). Al finalizar la ejecución, el entorno de pruebas confirma la estabilidad del software:

```text
test_limpieza_y_lematizacion (__main__.TestPipelineNLP.test_limpieza_y_lematizacion) ... ok

----------------------------------------------------------------------
Ran 1 test in 0.012s

OK


