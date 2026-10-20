# Clasificador Bayesiano Ingenuo para el dataset *digits*

## Descripción

El clasificador bayesiano ingenuo asigna a cada muestra `X` la clase `y` que maximiza una cantidad proporcional a la probabilidad a posteriori:

```
P(y | X) ∝ P(y) · P(x₀ | y) · P(x₁ | y) · … · P(xₙ | y)
```

La forma de calcular cada `P(xᵢ | y)` depende de la distribución que sigan los datos. En este proyecto se determina cuál se ajusta mejor al dataset *digits* y se estudia cómo influye el parámetro de suavizado α.

## Dataset

[`load_digits`](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_digits.html) de Scikit-learn:

- **1797** imágenes de dígitos del 0 al 9 (10 clases).
- Cada imagen es de **8×8 = 64 píxeles** (64 características).
- Cada píxel toma valores enteros de **0 (blanco) a 16 (negro)**.

## Metodología

1. **Carga y descripción** del dataset.
2. **División** en 80 % entrenamiento y 20 % prueba (`stratify=y`, `random_state=4`).
3. **Análisis de la distribución** de los píxeles: los datos son discretos, no normales, y un mismo píxel se distribuye distinto según la clase, por lo que se modela con una **distribución categórica** de 17 valores.
4. **Comparación de clasificadores** con validación cruzada K-fold (k = 4) sobre el conjunto de entrenamiento, usando accuracy, precision y recall (promedio macro).
5. **Suavizado de Laplace** sobre el clasificador elegido: evaluación en prueba y barrido de α entre 10⁻¹⁰ y 1000.

## Resultados

### Comparación de clasificadores (K-fold = 4, α = 1)

| Clasificador   | Accuracy |
|----------------|----------|
| **Categórico** | **0.9116** |
| Multinomial    | 0.8991   |
| Bernoulli      | 0.8518   |
| Gaussiano      | 0.8399   |
| Complementario | 0.8044   |

El **Bayesiano Ingenuo Categórico** (`CategoricalNB`) es el que mejor se adapta a los datos, lo cual coincide con la distribución observada en el análisis exploratorio.

### Evaluación en el conjunto de prueba (Categórico, α = 1)

| Indicador | Valor  |
|-----------|--------|
| Accuracy  | 0.9167 |
| Precision | 0.9246 |
| Recall    | 0.9165 |

Los resultados son similares a los de K-fold, por lo que el modelo generaliza bien. Los dígitos 0, 2 y 7 se clasifican sin errores; las confusiones más frecuentes son 8 → 1, 5 → 9 (4 veces cada una) y 1 → 9 (3 veces).

### Efecto de α

- **α muy pequeño (10⁻¹⁰):** accuracy de 0.8330, porque los valores no vistos en entrenamiento tienen probabilidad 0 y descartan la clase correcta.
- **α entre 0.01 y 2:** mejores resultados. El máximo está en α = 0.5 (0.9158) y α = 1 (Laplace) queda muy cerca (0.9116).
- **α grande (≥ 5):** el desempeño cae hasta 0.6221 con α = 1000, ya que las probabilidades tienden a 1/17 y los píxeles dejan de diferenciar las clases.

## Tecnologías

- Python 3
- NumPy
- Pandas
- Matplotlib
- Scikit-learn

## Cómo ejecutarlo

1. Clona el repositorio:

   ```bash
   git clone https://github.com/<usuario>/<repositorio>.git
   cd <repositorio>
   ```

2. Instala las dependencias:

   ```bash
   pip install numpy pandas matplotlib scikit-learn jupyter
   ```

3. Abre el notebook:

   ```bash
   jupyter notebook TALLER9_Clasificador_Bayesiano.ipynb
   ```

## Estructura del repositorio

```
.
├── TALLER9_Clasificador_Bayesiano.ipynb   # Notebook con todo el análisis
└── README.md
```
