# Sistema de Recomendación para Comiquería

Proyecto Capstone de Machine Learning (IBM). Este proyecto presenta un sistema de recomendación híbrido diseñado para una plataforma de e-commerce de mangas. El objetivo principal es mejorar la experiencia del usuario a través de recomendaciones personalizadas y relevantes.

---

## Descripción del Proyecto

- Este proyecto presenta el desarrollo y la evaluación de un sistema de recomendación de manga. Se implementaron múltiples enfoques, desde modelos basados en contenido y popularidad hasta un sistema híbrido con técnicas modernas de machine learning.
- El objetivo es ofrecer recomendaciones personalizadas y relevantes, abordando problemas comunes como el "arranque en frío" (cold start) para nuevos usuarios.

## Análisis y Metodología

### El proyecto se estructura en las siguientes fases:

1. **Análisis Exploratorio de Datos (EDA):**
   - Se realizó una limpieza inicial de los datos, eliminando columnas irrelevantes y manejando valores nulos.
   - Se analizaron las distribuciones de variables clave como `score`, y se visualizaron los mangas y géneros más populares utilizando Seaborn y Matplotlib.
   - Se estudió la correlación entre las variables numéricas a través de un heatmap para entender sus relaciones.

2. **Modelo 1: Sistema Basado en Contenido**
   - Técnica: Se utilizó la técnica TF-IDF (Term Frequency-Inverse Document Frequency) para vectorizar las características textuales de los mangas (géneros, temas, sinopsis, autores, etcétera).
   - Similitud: Se calculó la similitud del coseno entre los vectores de los mangas para encontrar los más parecidos.
   - Resultado: Un recomendador que, dado un título de manga, sugiere otros con características de contenido similares.

3. **Modelo 2: Sistema Basado en Popularidad (Calificación Ponderada)**
   - Técnica: Para solucionar el problema del "arranque en frío", se implementó un ranking basado en la fórmula de Calificación Ponderada (Weighted Rating), similar a la utilizada por IMDb.
   - Fórmula: WR = (v / (v + m)) * R + (m / (v + m)) * C
      - R: Puntuación promedio del manga.
      - v: Número de votos (`scored_by`).
      - m: Umbral mínimo de votos para ser considerado (percentil 90).
      - C: Puntuación promedio de todos los mangas.
   - Resultado: Una lista de los mejores mangas, balanceando popularidad y calificación, ideal para nuevos usuarios.

4. **Modelo 3: Sistema Híbrido**
   - Se combinaron los dos modelos anteriores en un sistema híbrido tipo "switcher":
     - Si el usuario está viendo un manga específico, el sistema recomienda mangas similares usando el modelo de contenido.
     - Si el usuario es nuevo o está en la página principal, se muestran los mangas más populares según el ranking de calificación ponderada.

5. **Clustering con K-Means:**
   - Objetivo: Descubrir "comunidades" o grupos temáticos de mangas de forma no supervisada.
   - Técnica: Se aplicó el algoritmo K-Means sobre la matriz TF-IDF. Se utilizó el Método del Codo para determinar que k=6 era un número óptimo de clusters.
   - Resultado: Cada manga fue asignado a uno de los 6 clusters, lo que permite un nuevo tipo de recomendación (ejemplo: "otros mangas populares de este mismo cluster").

6. **Embeddings con Redes Neuronales (Autoencoder):**
   - Objetivo: Mejorar la representación de características de los mangas utilizando embeddings densos en lugar de los vectores dispersos de TF-IDF.
   - Técnica: Se construyó y entrenó un Autoencoder con TensorFlow/Keras. La capa "encoder" de la red aprendió a comprimir los vectores de 20,000 dimensiones de TF-IDF en embeddings densos de 128 dimensiones.
   - Resultado: Se generó un nuevo sistema de recomendación basado en contenido utilizando la similitud del coseno sobre estos embeddings, capturando relaciones semánticas más complejas entre los mangas.

---

## Tecnologías Utilizadas

- Python 3.11
- Pandas & NumPy: Para manipulación y análisis de datos.
- Matplotlib & Seaborn: Para visualización de datos.
- Scikit-learn: Para TF-IDF, Similitud del Coseno y K-Means.
- TensorFlow & Keras: Para la construcción y entrenamiento del Autoencoder.

---

## Instrucciones de Instalación

1. **Clona este repositorio**
   ```bash
   git clone https://github.com/geronimo290/Sistema-de-Recomendaci-n-Comiquer-a.git
   cd Sistema-de-Recomendaci-n-Comiquer-a
   ```

2. **Crea un entorno virtual (opcional pero recomendado)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # En Windows: venv\Scripts\activate
   ```

3. **Instala las dependencias**
   ```bash
   pip install -r requirements.txt
   ```

   > Si el archivo `requirements.txt` no existe, instala manualmente las librerías principales:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn tensorflow keras
   ```

4. **Descarga o prepara el dataset**
   - Asegúrate de tener los datos requeridos en la ruta esperada por los notebooks/scripts (consulta el código para más detalles).

5. **Ejecuta los notebooks o scripts**
   - Puedes abrir y ejecutar los notebooks con Jupyter Notebook:
     ```bash
     jupyter notebook
     ```
   - O bien, ejecutar scripts específicos con:
     ```bash
     python nombre_del_script.py
     ```
