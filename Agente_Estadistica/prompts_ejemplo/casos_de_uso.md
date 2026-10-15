# Casos de uso y prompts de prueba

Los siguientes intercambios sirven para evaluar si el agente explica, orienta y valida sin hacer el proyecto completo por el estudiante. Los datos numéricos de los ejemplos son hipotéticos.

## 1. Interpretación del tercer y cuarto momento

**Estudiante:** ¿Qué significan físicamente el tercer y cuarto momento central? Tengo un `gamma_1` positivo y un exceso de curtosis alto.

**Agente:** El tercer momento central `mu_3=\frac{1}{N}\sum_i(x_i-\mu)^3` conserva el signo de las desviaciones y, al estandarizarlo, produce `gamma_1=mu_3/sigma^3`. Un valor positivo suele señalar una cola derecha más larga o más influyente; no significa que todas las observaciones grandes sean errores ni que la media necesariamente sea mayor en cualquier conjunto. El cuarto momento central `mu_4=\frac{1}{N}\sum_i(x_i-\mu)^4` da mucho peso a desviaciones lejanas por la potencia cuarta. La curtosis de Pearson es `beta_2=mu_4/sigma^4`, mientras que el exceso es `gamma_2=beta_2-3`; confirma cuál reporta tu programa. Un exceso alto indica mayor influencia relativa de valores alejados bajo esta medida, no un conteo de atípicos. ¿Puedes compartir tamaño, unidad y un histograma o resumen de cuartiles para interpretar el resultado en contexto?

## 2. Ayuda de código: caja y bigotes en Python

**Estudiante:** ¿Cómo identifico posibles atípicos con Pandas y hago el boxplot? Mi columna es `tiempo_ms`.

**Agente:** Primero confirma que `tiempo_ms` es cuantitativa, expresa una unidad consistente y que definiste el método de cuartiles. Un esquema breve para calcular Tukey es:

```python
valores = df["tiempo_ms"].dropna().astype(float)
q1 = valores.quantile(0.25, interpolation="linear")
q3 = valores.quantile(0.75, interpolation="linear")
ric = q3 - q1
limite_inferior = q1 - 1.5 * ric
limite_superior = q3 + 1.5 * ric
posibles_atipicos = valores[
    (valores < limite_inferior) | (valores > limite_superior)
]
```

Pandas puede usar convenciones de cuantiles configurables; verifica que corresponda a la posición `1+(N-1)p` si quieres mantener la convención de esta guía. Para visualizar, puedes usar `df.boxplot(column="tiempo_ms")` o la biblioteca de gráficas del proyecto. La detección no debe borrar datos: muestra filas marcadas, revisa unidades/captura y registra cualquier exclusión aprobada. ¿Qué comportamiento quieres cuando la columna contiene texto no convertible?

## 3. Validación de intervalos agrupados

**Estudiante:** Uso Sturges, calculo `k=1+3.322*log10(N)` y pongo el ancho como rango dividido por `k`. ¿Ya está correcto?

**Agente:** Es una base razonable, pero revisemos tres detalles que pueden dejar observaciones fuera. `k` debe convertirse a entero con un criterio declarado; el ancho debe ser positivo; y los intervalos deben cubrir `x_min` hasta `x_max` con extremos inequívocos. Por ejemplo, puedes usar `[l_i,u_i)` y cerrar el último intervalo por la derecha. Prueba primero un conjunto pequeño cuyos extremos conoces: verifica que cada dato se asigne exactamente a una clase y que `sum(f_i)=N`. ¿Qué pasa en tu implementación si `R=0` o si `x_max` cae justo en un límite de clase? Compárteme esa parte del algoritmo y la revisamos.

## 4. Interpretación de CV y asimetría

**Estudiante:** La población de tiempos de resolución tiene `CV=18%` y `gamma_1=0.9`. ¿Qué concluyo en el informe?

**Agente:** Puedes decir que la desviación estándar equivale al 18 % de la magnitud de la media, siempre que la media sea distinta de cero, los tiempos tengan cero significativo y todas las mediciones usen la misma unidad. No hay un umbral universal que convierta 18 % automáticamente en variabilidad “alta” o “baja”; compáralo con el contexto o un criterio del curso. `gamma_1=0.9` sugiere asimetría positiva: algunos tiempos altos podrían alargar la cola derecha. Antes de afirmarlo, revisa histograma y boxplot y verifica posibles errores sin eliminar observaciones automáticamente. Como se trata de población, indica `N`, periodo y definición del tiempo. ¿Cuál es la unidad y cuál fue el criterio de inclusión?

## 5. Cuestionario para sustentar el proyecto

**Estudiante:** Prepárame para sustentar. No me des todas las respuestas todavía.

**Agente:** Perfecto. Responde una por una y te daré retroalimentación antes de pasar a la siguiente:

1. ¿Cómo distingues una variable cualitativa ordinal de una cuantitativa discreta, y qué resúmenes usarías para cada una?
2. ¿Qué validaciones harías al importar una columna numérica desde CSV con valores faltantes y separador decimal distinto?
3. Si agrupas observaciones en clases, ¿por qué la media calculada con marcas de clase es aproximada? ¿Cómo compruebas que la tabla cubre todos los datos?
4. ¿Por qué la varianza poblacional divide entre `N` y cuándo sería incorrecto usarla?
5. ¿Qué convención de cuantiles implementaste y cómo garantizas que el boxplot y el filtro de Tukey la compartan?
6. ¿Qué diferencia hay entre curtosis de Pearson `beta_2` y exceso `gamma_2`? ¿Qué valor de exceso corresponde a la normal?
7. ¿Qué hace tu programa cuando `RIC=0`, `sigma=0` o la población está vacía?
8. ¿Cómo explicas un posible atípico y qué registro dejas si el usuario decide excluirlo?

Empieza por la pregunta 2 y describe tanto una validación como un caso de prueba que la comprobaría.

## 6. Prueba adicional: media o mediana

**Estudiante:** Tengo sueldos con dos valores muy grandes. ¿Debo reportar solo la media?

**Agente:** No necesariamente. La media poblacional `mu` incorpora todas las magnitudes y puede desplazarse por esos valores; la mediana `x~` es menos sensible. Reportar ambas junto con un histograma, `Q1`, `Q3` y una revisión de los registros puede describir mejor la población. Primero confirma que los valores altos son legítimos y comparables (misma moneda, periodo y unidad). ¿Tu objetivo es resumir el valor típico, el total promedio por persona u otra pregunta de negocio?
