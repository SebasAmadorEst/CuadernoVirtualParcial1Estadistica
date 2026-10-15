# Estadística Descriptiva y Momentos Estadísticos

## 1. Alcance y notación
La estadística descriptiva organiza y resume observaciones del conjunto estudiado; no establece por sí sola causalidad ni generaliza automáticamente a otra población. En este proyecto se considera la población completa de interés, de tamaño `N`, salvo que el enunciado indique explícitamente una muestra. Se usan parámetros poblacionales como `\mu` y `\sigma^2`; para una muestra se deben emplear notación y denominadores muestrales apropiados.

Sean `x_1, x_2, ..., x_N` observaciones cuantitativas. Si una observación `x_i` tiene frecuencia `f_i`, entonces `\sum_i f_i = N`. En datos agrupados, la marca de clase aproxima todas las observaciones del intervalo y, por tanto, los resúmenes calculados son aproximaciones.

## 2. Gestión y clasificación de datos

### Naturaleza de variables
- **Cualitativa nominal:** categorías sin orden inherente (por ejemplo, sistema operativo).
- **Cualitativa ordinal:** categorías con orden, pero cuyas distancias no tienen significado numérico asegurado (por ejemplo, nivel de satisfacción).
- **Cuantitativa discreta:** conteos que toman valores separados, usualmente enteros (por ejemplo, número de fallos).
- **Cuantitativa continua:** mediciones que pueden tomar cualquier valor de un intervalo, según precisión y unidad (por ejemplo, tiempo de respuesta).

La escala y el significado determinan qué resúmenes son válidos. Para categorías suelen usarse conteos, proporciones y moda; para variables cuantitativas pueden calcularse medidas de centro y dispersión. No se debe tratar un identificador o código de categoría como una medición.

### Validación y limpieza
Al importar CSV o Excel, verificar existencia y legibilidad del archivo, encabezados, columnas requeridas, delimitador, codificación, separador decimal, tipos, unidades, filas vacías, duplicados según la regla del dominio y valores faltantes. Validar rangos plausibles sin borrar observaciones automáticamente. Toda exclusión, conversión o imputación debe quedar documentada, incluyendo conteos antes y después; conservar los datos originales cuando sea posible.

## 3. Organización y tablas de frecuencia

Para datos cuantitativos, una distribución puede resumir valores individuales o agruparlos en intervalos. Para datos cualitativos se cuentan categorías; no se crean intervalos numéricos.

### Número y amplitud de clases
Una regla inicial frecuente es Sturges:

$$k = 1 + 3.322\log_{10}(N)$$

Se redondea `k` a un entero según el criterio declarado. Es una guía, no una ley: la forma de los datos y el propósito pueden requerir otro número de clases. El rango es `R = x_{max} - x_{min}` y la amplitud aproximada es:

$$a = \frac{R}{k}$$

Puede redondearse a un valor práctico, pero los intervalos resultantes deben cubrir todas las observaciones, no solaparse y tener una regla clara para los extremos. En mediciones decimales, es preferible definir clases semiabiertas `[l_i, u_i)` y hacer cerrada la última clase por la derecha, evitando asignar una observación a dos clases o dejarla fuera. Si `R=0`, no se divide entre cero: hay un único valor observado y se informa el caso degenerado.

### Frecuencias
Para la clase o categoría `i`:
- Frecuencia absoluta: `f_i`, número de observaciones.
- Frecuencia relativa: `h_i = f_i/N`.
- Frecuencia absoluta acumulada: `F_i = \sum_{j=1}^{i} f_j`.
- Frecuencia relativa acumulada: `H_i = \sum_{j=1}^{i} h_j`.

Comprobaciones: `\sum_i f_i=N`, `\sum_i h_i=1` (salvo tolerancia numérica), la acumulada no decrece, `F_{última}=N` y `H_{última}=1`. Mostrar proporciones como porcentaje requiere multiplicar `h_i` por 100 y etiquetarlo. En variables continuas agrupadas puede estimarse densidad como frecuencia relativa dividida por amplitud si las clases no tienen el mismo ancho; el área, no necesariamente la altura, representa la proporción.

## 4. Tendencia central

### Media poblacional

$$\mu = \frac{1}{N}\sum_{i=1}^{N}x_i$$

Con valores distintos y frecuencias `f_i`:

$$\mu = \frac{1}{N}\sum_i f_i x_i$$

En datos agrupados por intervalos, sustituir `x_i` por la marca de clase `m_i` da `\hat{\mu}=\frac{1}{N}\sum_i f_i m_i`, una estimación basada en la tabla, no la media exacta de los datos originales.

### Mediana y moda
Ordenar las observaciones. La mediana `\tilde{x}` es el centro: si `N` es impar, el dato de posición `(N+1)/2`; si es par, el promedio de los datos de posiciones `N/2` y `N/2+1`. La moda `Mo` es el valor o categoría de mayor frecuencia; puede no existir una moda única o puede haber varias. Para clases agrupadas, la clase modal identifica la mayor frecuencia, pero no equivale a una moda exacta.

La media usa todas las magnitudes y es sensible a valores extremos; la mediana es más robusta. La elección depende del tipo de variable, distribución y propósito.

## 5. Posición: cuartiles y RIC
Los cuartiles dividen los datos ordenados en cuatro partes. `Q_1`, `Q_2` y `Q_3` corresponden a los percentiles 25, 50 y 75; `Q_2` coincide con la mediana.

No existe una única convención computacional para interpolar cuantiles en conjuntos finitos. Para reproducibilidad, este material usa por defecto la posición `r=1+(N-1)p` en indexación desde 1 (`p=0.25, 0.50, 0.75`), interpolando linealmente entre observaciones adyacentes cuando `r` no es entero. Una implementación puede usar otra convención si la declara y la mantiene consistentemente.

El rango intercuartílico mide la dispersión del 50 % central:

$$RIC = Q_3 - Q_1$$

En datos agrupados, los cuantiles se estiman interpolando dentro de la clase que contiene la posición acumulada; documentar la fórmula adoptada y recordar que la agrupación reduce precisión.

## 6. Dispersión y atípicos

### Rango, varianza y desviación estándar poblacionales

$$R = x_{max} - x_{min}$$

$$\sigma^2 = \frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2$$

$$\sigma = \sqrt{\sigma^2}$$

Con frecuencias, la suma de cuadrados se pondera por `f_i`; con intervalos agrupados se usan marcas de clase y el resultado es aproximado. Dividir entre `N` corresponde a la población completa; si los datos son una muestra para estimar varianza poblacional, se suele usar `n-1` y la notación muestral correspondiente. No mezclar ambas convenciones.

### Coeficiente de variación
Para una variable cuantitativa con media distinta de cero:

$$CV = \frac{\sigma}{|\mu|}\times 100\%$$

El valor absoluto hace explícita la escala relativa. El CV no es informativo o puede ser inestable si la media es cero o cercana a cero; requiere escala de razón con cero significativo y no debe compararse sin contexto entre magnitudes de distinta naturaleza. Un CV mayor indica mayor dispersión relativa bajo condiciones comparables, no necesariamente peor calidad.

### Regla de Tukey para posibles atípicos

$$L_{inf}=Q_1-1.5\,RIC, \qquad L_{sup}=Q_3+1.5\,RIC$$

Los valores fuera de `[L_inf, L_sup]` se marcan como **posibles atípicos**. Revisar errores de captura, unidades y contexto; la regla no demuestra que el dato sea erróneo y no justifica eliminarlo automáticamente. Si `RIC=0`, informar cómo se comporta la regla y revisar los valores distintos de la concentración principal.

## 7. Momentos, asimetría y curtosis
El momento central poblacional de orden `k` es:

$$\mu_k = \frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^k$$

En particular, `\mu_1=0`, `\mu_2=\sigma^2`, `\mu_3` expresa información direccional sobre la forma y `\mu_4` pondera fuertemente desviaciones alejadas. En frecuencias, se pondera cada término por `f_i`; en intervalos se usan marcas de clase, por lo que momentos, asimetría y curtosis son aproximados. Las unidades de `\mu_k` son las unidades de `x` elevadas a `k`.

### Asimetría de Fisher

$$\gamma_1 = \frac{\mu_3}{\sigma^3}$$

Es adimensional. `\gamma_1=0` indica que el tercer momento central estandarizado es cero, pero no prueba por sí solo simetría perfecta. Un valor positivo suele corresponder a una cola derecha más larga o más influyente; uno negativo, a una cola izquierda. Debe interpretarse junto con histograma, caja y bigotes y medidas de centro. Si `\sigma=0`, no está definida.

### Curtosis
La curtosis de Pearson y su exceso se distinguen explícitamente:

$$\beta_2 = \frac{\mu_4}{\sigma^4}, \qquad \gamma_2 = \beta_2 - 3$$

`\beta_2` es la curtosis de Pearson; `\gamma_2` es el exceso de curtosis de Fisher. Para una distribución normal, `\beta_2=3` y `\gamma_2=0`. El exceso positivo indica mayor peso relativo en observaciones alejadas según esta medida, y el negativo, menor; no debe reducirse la interpretación a una descripción aislada de la altura del pico. Curtosis alta no equivale automáticamente a más atípicos. Si la varianza es cero, no está definida. Implementaciones muestrales pueden aplicar correcciones de sesgo y producir resultados distintos; especificar la fórmula, divisor y corrección que se usa.

## 8. Visualización y lectura analítica
- **Histograma:** variable cuantitativa; barras contiguas para intervalos. Describir forma, centro aproximado, dispersión, asimetría, huecos y posibles valores alejados. Indicar intervalos y escala del eje vertical; con anchos desiguales usar densidad cuando corresponda.
- **Caja y bigotes:** variable cuantitativa; comunica mediana, cuartiles, RIC y observaciones potencialmente atípicas según una regla declarada. Los bigotes varían según la convención de la biblioteca; indicar si llegan al extremo o al dato más extremo dentro de `1.5 RIC`.
- **Barras:** frecuencias o proporciones de categorías (o valores discretos claramente etiquetados); las barras suelen estar separadas. Usar categorías legibles y denominador claro.
- **Sectores:** composición de un total categórico cuando las categorías son mutuamente excluyentes, cubren el total y son pocas; incluir porcentajes/etiquetas y evitar comparar sectores pequeños con precisión aparente.

Toda gráfica debe llevar título informativo, nombres y unidades de ejes cuando apliquen, leyenda solo si aporta, fuente o población analizada y tratamiento de faltantes relevante. No truncar ejes de forma que distorsione la comparación ni inferir causalidad a partir de patrones descriptivos.

## 9. Lista de comprobación
1. ¿Se identificó población o muestra y se registró `N` válido?
2. ¿El tipo y escala de variable son compatibles con el cálculo y la gráfica?
3. ¿Se declararon convención de cuantiles, intervalos y curtosis?
4. ¿Las frecuencias suman `N` y las proporciones suman uno?
5. ¿Se usó el denominador poblacional cuando corresponde?
6. ¿Los resultados agrupados se etiquetaron como aproximaciones?
7. ¿Los posibles atípicos se investigaron sin borrado automático?
8. ¿La interpretación incluye contexto, unidades y limitaciones?
