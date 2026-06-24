# Unidad-6-Stopwords-y-limpieza-de-texto

# 🧪 NLP Lab: Text Cleaning & Stopword Optimization (Amazon Dataset)

¡Hola! En este módulo de laboratorio me enfoqué en el diseño e implementación de un pipeline robusto para la **limpieza de texto, tokenización y optimización adaptativa de Stopwords** utilizando la librería avanzada de NLP **spaCy**.

El objetivo principal fue transformar un corpus de reseñas de clientes de Amazon (datos altamente no estructurados) en características limpias y de alta relevancia estadística, listas para alimentar modelos predictivos.

---

## 🛠️ Desafíos de Ingeniería Resueltos (Control de Calidad del Dato)

Durante la implementación inicial, se detectó una anomalía crítica de *Data Drift* donde los términos más frecuentes eran números de años y meses en lugar de palabras con carga semántica de e-commerce.

* **El Problema (Column Shifting):** Se identificó que el parámetro estricto `quoting=3` en el parseo del CSV hacía que Pandas fragmentara el texto ante la presencia de comas gramaticales dentro de las reseñas, empujando los metadatos de fechas hacia la columna principal de texto.
* **La Solución:** Se reestructuró la ingesta eliminando dicha restricción, se migró el pipeline nativamente al modelo en inglés (`en_core_web_sm`) y se incorporaron filtros booleanos estrictos (`token.is_digit` y `token.like_num`) para purgar por completo el ruido numérico residual.

---

## 🔄 Fases del Pipeline de Procesamiento

El script ejecuta de forma secuencial tres estrategias de análisis de frecuencia para comparar el impacto de la limpieza:

1. **Corpus Crudo (Baseline):** Tokenización básica aislando únicamente signos de puntuación. El vocabulario es dominado por conectores funcionales sin valor analítico (*"the"*, *"i"*, *"and"*).
2. **Filtrado Estándar (Idioma):** Aplicación del diccionario de Stopwords nativo de spaCy para remover el ruido gramatical general del inglés.
3. **Filtrado Avanzado (Dominio E-commerce):** Inyección personalizada de Stopwords de negocio (*"product"*, *"amazon"*, *"shipping"*, *"seller"*). Términos que, aunque son válidos en el idioma, aparecen de forma ubiqua en el dominio de compras sin aportar varianza ni diferenciar el sentimiento.

---

## 📈 Impacto Metodológico en Modelos Avanzados

* **Optimización para TF-IDF:** La eliminación previa de Stopwords reduce drásticamente la dimensionalidad de la matriz dispersa (*sparse matrix*), evitando la "maldición de la dimensionalidad" y acelerando la convergencia de clasificadores supervisados.
* **Fidelidad en Word Embeddings:** Al limpiar el ruido gramatical, los modelos de contexto local (como Word2Vec o FastText) aprenden relaciones vectoriales legítimas entre conceptos de negocio (ej. asociar adjetivos directamente a los atributos del producto) en lugar de saturar las ventanas de atención con conectores.

---

## ⚙️ Tecnologías Utilizadas

* **Python 3.x**
* **spaCy** (Pipeline `en_core_web_sm` para Tokenización avanzada y filtrado léxico)
* **Pandas & NumPy** (Ingesta y manipulación de estructuras tabulares)
* **Kagglehub** (Ingesta automatizada del dataset `dongrelaxman/amazon-reviews-dataset`)

---

## 🚀 Cómo Ejecutar el Laboratorio

1. Clonar el repositorio:
   ```bash
   git clone [https://github.com/TU_USUARIO/TU_REPOSITORIO.git](https://github.com/TU_USUARIO/TU_REPOSITORIO.git)

   pip install pandas spacy kagglehub
python -m spacy download en_core_web_sm

