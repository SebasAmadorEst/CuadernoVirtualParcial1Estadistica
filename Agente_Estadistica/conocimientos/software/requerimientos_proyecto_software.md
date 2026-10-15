# Requerimientos del proyecto de software estadístico

## Propósito y alcance
El sistema permite ingresar o importar una población de interés, validarla y generar resúmenes descriptivos reproducibles. Debe poder implementarse en Python, Java, JavaScript o C#. El agente debe comprobar requisitos, lógica, casos límite e interpretación, sin imponer una interfaz o biblioteca particular.

### Reglas transversales
- Mantener explícito el alcance poblacional y usar denominador `N` en la varianza poblacional.
- Separar los datos originales de los datos transformados; registrar exclusiones y conversiones.
- Validar entradas antes de calcular y ofrecer mensajes de error accionables, sin fallas silenciosas.
- Distinguir cualitativas y cuantitativas, y discretas y continuas; impedir operaciones no válidas para el tipo de variable.
- Declarar convenciones de cuartiles, clases, extremos de intervalos, bigotes y curtosis.
- Permitir cálculo no agrupado exacto y agrupado aproximado cuando el requisito y los datos lo permitan.
- Manejar conjunto vacío, una observación, valores constantes, nulos, infinitos, datos no numéricos y desbordamiento/división por cero de forma definida.
- Presentar valores con precisión configurable, sin redondear prematuramente cálculos intermedios.
- Facilitar pruebas unitarias de las funciones estadísticas y pruebas de integración de importación, cálculo y visualización.

## Módulo 1: Importación/Ingreso y Validación
**Funciones:** ingreso manual o importación de CSV/Excel; selección de hoja/columna si aplica; identificación del tipo de variable y unidad.

**Validaciones:** archivo legible y extensión admitida, esquema/encabezados, filas vacías, delimitador y decimal, conversión a tipo, valores ausentes, `NaN`/infinitos, dominios/rangos configurables y datos duplicados según regla de negocio. No asumir que duplicados son errores. Resumir total de filas, valores válidos, faltantes y rechazados, con razón por registro o resumen descargable. No modificar el original silenciosamente.

**Aceptación:** ante una columna numérica con una celda no convertible, informar esa celda y permitir corregirla o excluirla con confirmación; los cálculos posteriores usan el conjunto y `N` reportados.

## Módulo 2: Tablas de Distribución de Frecuencias
**Funciones:** tablas por categorías/valores y agrupación cuantitativa en intervalos; configurar número de clases o usar Sturges; mostrar límites, marca de clase, `f_i`, `h_i`, `F_i`, `H_i` y porcentajes.

**Reglas:** intervalos exhaustivos y no solapados, política de extremos explícita, ancho positivo, tratamiento de rango cero, frecuencias no negativas y exactitud de acumulados. Para anchos desiguales, considerar densidad al graficar histogramas.

**Aceptación:** comprobar `sum(f_i)=N`, `sum(h_i)≈1`, `F_final=N` y `H_final≈1`; cada observación válida queda asignada exactamente una vez.

## Módulo 3: Medidas de Tendencia Central
**Funciones:** media poblacional `mu`, mediana y moda; frecuencias y datos agrupados cuando corresponda.

**Reglas:** ordenar para mediana; especificar cómo se informa moda multimodal o inexistente; calcular media ponderada con `f_i`. Para intervalos, usar marcas de clase y etiquetar resultado como aproximado.

**Aceptación:** datos vacíos producen resultado no disponible con mensaje, y datos constantes devuelven media, mediana y moda coherentes. Validar con cálculos manuales pequeños.

## Módulo 4: Medidas de Posición
**Funciones:** cuartiles `Q1`, `Q2`, `Q3` y rango intercuartílico `RIC`.

**Reglas:** método de cuantiles seleccionable o documentado; valor predeterminado sugerido: posición `r=1+(N-1)p` e interpolación lineal. `Q2` debe coincidir con la mediana bajo la convención adoptada; informar limitaciones en datos agrupados.

**Aceptación:** para datos ordenados se cumple `Q1≤Q2≤Q3` y `RIC≥0`; el mismo método se usa en tablas, boxplot y filtro de posibles atípicos.

## Módulo 5: Medidas de Dispersión
**Funciones:** mínimo, máximo, rango, varianza y desviación estándar poblacionales, coeficiente de variación cuando sea interpretable.

**Reglas:** `sigma^2=sum((x_i-mu)^2)/N`; para frecuencias, ponderar por `f_i`. No confundir con varianza muestral de divisor `n-1`. En datos agrupados, resultados aproximados mediante marcas de clase. El CV debe advertir cuando la media es cero/cercana a cero o la escala no admite comparación relativa significativa.

**Aceptación:** dispersión no negativa; un conjunto constante tiene varianza, desviación y rango cero. Mantener unidades: varianza en unidades al cuadrado y desviación en unidades originales.

## Módulo 6: Detección y Filtro de Valores Atípicos
**Funciones:** calcular límites de Tukey `Q1-1.5*RIC` y `Q3+1.5*RIC`, listar posibles atípicos y ofrecer filtro reversible/configurable.

**Reglas:** límites y método de cuartiles visibles; conservar referencia a registros originales; cualquier exclusión requiere confirmación y registrar criterio, filas afectadas y `N` antes/después. No presentar el indicador como diagnóstico de error.

**Aceptación:** pruebas en los límites exactos y fuera de ellos; registros no desaparecen sin acción explícita; el análisis puede repetirse incluyendo o excluyendo casos y comparar resultados.

## Módulo 7: Momentos Estadísticos (Asimetría y Curtosis)
**Funciones:** momentos centrales `mu_1` a `mu_4`, asimetría de Fisher `gamma_1=mu_3/sigma^3`, curtosis de Pearson `beta_2=mu_4/sigma^4` y exceso `gamma_2=beta_2-3`.

**Reglas:** usar momentos poblacionales y reportar fórmulas/divisor; distinguir Pearson de exceso; declarar si se aplica alguna corrección muestral. En datos agrupados, indicar aproximación con marcas de clase. Si `sigma=0`, devolver no definido con explicación. Evitar afirmaciones categóricas basadas únicamente en signo o valor de un coeficiente.

**Aceptación:** `mu_1≈0`, `mu_2=sigma^2`; para datos constantes, coeficientes no definidos; comprobar resultados en conjuntos pequeños y un conjunto simétrico.

## Módulo 8: Visualización Gráfica
**Funciones:** histogramas, diagramas de caja y bigotes, barras y sectores, seleccionados según tipo de variable.

**Reglas:** etiquetas, título, unidades, población y método; respetar naturaleza ordinal/categórica, política de intervalos, anchos desiguales y cuartiles declarados. Boxplot debe explicar convención de bigotes y marcar posibles atípicos sin ocultar observaciones. Evitar sectores con muchas categorías y escalas engañosas.

**Aceptación:** datos faltantes o vacíos producen estado comprensible; los totales y escalas corresponden a la tabla de frecuencias; visualizaciones accesibles con etiquetas legibles y una alternativa textual/resumen.

## Criterios globales de validación
1. Cada resumen puede rastrearse hasta los datos válidos y sus parámetros de configuración.
2. Pruebas con población vacía, `N=1`, constantes, empates, negativos, extremos, faltantes y límites de clase.
3. Las funciones de cálculo son separables de presentación/importación para facilitar pruebas.
4. Resultados mostrados tienen nombre, valor, unidad cuando aplica, fórmula o método y advertencia de aproximación.
5. La comparación antes/después de filtrar atípicos conserva trazabilidad y no altera el conjunto fuente.
