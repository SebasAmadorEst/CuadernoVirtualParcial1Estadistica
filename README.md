# Cuaderno Virtual de Estadística

Aplicación educativa para explorar estadística descriptiva mediante datos numéricos, tablas, medidas resumen y gráficos. Está pensada para estudiantes que empiezan a estudiar análisis de datos y para docentes que quieran mostrar cómo cambian los resultados al variar las observaciones.

## Empezar

1. Abre `index.html` con un navegador actualizado, por ejemplo Firefox, Chrome o Edge. No hace falta instalar paquetes ni compilar el proyecto.
2. Al abrirse, el cuaderno carga un conjunto de demostración de tiempos de respuesta en milisegundos. Sus resultados son solo un ejemplo; no representan datos de una investigación real.
3. Usa **Abrir**, **Siguiente**, **Anterior**, **Inicio** o el selector de páginas para recorrer el contenido. En una pantalla grande se muestran dos páginas a la vez; en una pantalla pequeña se muestra una.
4. En la página 1 puedes escribir observaciones o importar un archivo y pulsar **Analizar datos**. Los resultados se actualizan en todas las páginas.

La hoja de cálculo y los cálculos funcionan en el navegador. Las gráficas necesitan que Chart.js pueda cargarse desde jsDelivr; las tipografías personalizadas también se descargan de Google Fonts. Si no hay conexión, puede que las gráficas o esas fuentes no estén disponibles.

## Recorrido del cuaderno

| Página | Tema | Qué presenta |
| --- | --- | --- |
| Cubierta | Portada | Identificación visual del cuaderno. |
| 1 | Gestión e identificación | Ingreso manual o CSV/TXT, tipo de conjunto, tipo de variable y resumen inicial. |
| 2 | Organización | Orden ascendente y descendente, tabla de frecuencias y agrupación opcional por intervalos. |
| 3 | Tendencia central | Media, mediana y moda. |
| 4 | Posición | Cuartiles Q₁, Q₂ y Q₃, y rango intercuartílico. |
| 5 | Dispersión | Mínimo, máximo, rango, varianza, desviación estándar y coeficiente de variación. |
| 6 | Posibles atípicos | Límites de Tukey y observaciones fuera de esos límites. No se borran datos automáticamente. |
| 7 | Momentos | Momentos centrales, asimetría y exceso de curtosis. |
| 8 | Gráficos | Frecuencias, caja y bigotes, y proporciones. |

## Ingresar datos

### Escribir observaciones

En la página 1 se aceptan números separados por comas, espacios, punto y coma o saltos de línea. Usa punto para separar decimales. Por ejemplo:

```text
12.4, 15.8, 16.1
18.2 20.5
```

Los valores que no se puedan interpretar como números se informan y no se incluyen en el análisis. Revisa el aviso para confirmar que no se haya omitido un dato por error.

### Importar un archivo

Se admiten archivos CSV y TXT. Si hay varias columnas, elige la que quieres analizar. La aplicación intenta reconocer el separador y los encabezados, y puede sugerir la columna con más valores numéricos.

- Se admiten separadores coma, punto y coma, tabulación o espacios.
- En archivos separados por punto y coma se interpreta la coma como separador decimal. Comprueba que el formato del archivo coincide con esta convención.
- Las celdas vacías se omiten; las celdas no numéricas se cuentan como inválidas y se notifican.
- El archivo se lee en el navegador; no se carga a un servidor de esta aplicación.

### Opciones

- **Población completa / muestra:** define la fórmula de varianza y desviación estándar. La opción inicial es población.
- **Tipo de variable:** puede inferirse a partir de los valores o elegirse como discreta o continua. La inferencia automática solo mira si los valores son enteros; confirma el tipo según el significado de la variable.
- **Tabla de frecuencias:** puede agruparse con Sturges, mantenerse sin agrupar o dejarse en modo automático. En automático, la tabla se agrupa cuando hay más de 14 valores distintos.
- El límite es de 50 000 observaciones por análisis.

## Fórmulas y convenciones

La aplicación analiza variables **cuantitativas**. Para población usa `N`, media poblacional `μ` y varianza poblacional. Para muestra usa `n`, media muestral `x̄` y varianza muestral con divisor `n − 1`.

### Frecuencias

- Número inicial de clases: $k = 1 + 3.322\log_{10}(N)$, redondeado a un entero.
- Rango: $R = x_{max} - x_{min}$; amplitud aproximada: $a = R/k$.
- Marca de clase: $X_i = (L_i + U_i)/2$.
- Frecuencia relativa: $h_i = f_i/N$; $F_i$ y $H_i$ son las frecuencias acumuladas.
- Las clases agrupadas se usan para resumir frecuencias. Las medidas estadísticas se calculan a partir de los valores individuales originales, no de las marcas de clase.

### Centro y posición

- Media poblacional: $\mu = \sum x_i/N$.
- Mediana: valor central de los datos ordenados; si hay un número par de observaciones, promedio de los dos valores centrales.
- Moda: valor o valores con frecuencia máxima. Si cada valor aparece una sola vez, se informa que no hay moda.
- Cuartiles: posición $r = 1 + (N - 1)p$ para $p=0.25, 0.50, 0.75$, con interpolación lineal cuando la posición no es entera. $Q_2$ es la mediana y $RIC = Q_3 - Q_1$.

### Dispersión y forma

- Rango: $R = x_{max} - x_{min}$.
- Varianza de población: $\sigma^2 = \sum(x_i-\mu)^2/N$; desviación: $\sigma = \sqrt{\sigma^2}$.
- Varianza muestral: $s^2 = \sum(x_i-\bar{x})^2/(n-1)$; desviación: $s = \sqrt{s^2}$.
- Coeficiente de variación: $CV = (\sigma/|\mu|)\times100\%$ (o usa `s` y `x̄` en modo muestra). No se informa si la media es cero o demasiado cercana a cero.
- Regla de Tukey: límites $Q_1 - 1.5RIC$ y $Q_3 + 1.5RIC$. Un valor fuera de ellos es un **posible** atípico, no un error confirmado.
- Momento central de orden $j$: $m_j = \sum(x_i-\mu)^j/N$. El primero es cero teóricamente; el segundo es la varianza descriptiva. La aplicación muestra los momentos de forma con divisor poblacional, incluso si se seleccionó muestra.
- Asimetría: $\gamma_1 = m_3/m_2^{3/2}$. El signo orienta sobre la cola con mayor influencia, pero no prueba simetría por sí solo.
- Exceso de curtosis: $\gamma_2 = m_4/m_2^2 - 3$. Para la distribución normal el exceso es cero. No equivale a contar atípicos.

Los umbrales de $0.5$ que aparecen para interpretar asimetría y curtosis son guías descriptivas, no reglas universales. Cuando la dispersión es cero, asimetría y curtosis quedan sin definir.

## Gráficos

- **Frecuencias:** barras para valores discretos o intervalos agrupados. En intervalos, las barras se dibujan contiguas.
- **Caja y bigotes:** la caja abarca Q₁ a Q₃; la línea señala la mediana. Los bigotes llegan al valor más extremo dentro de los límites de Tukey y los puntos exteriores se marcan como posibles atípicos.
- **Proporciones:** cada sector representa la fracción de observaciones de un valor o intervalo.

Los gráficos ayudan a describir patrones; no demuestran causalidad. Interpreta los resultados según las unidades, la población o muestra y el contexto de los datos.

## Privacidad y estado de los datos

Los cálculos y la lectura de archivos se realizan localmente en el navegador; esta aplicación no tiene servidor y no envía las observaciones a un servicio. Las observaciones viven en la memoria de la página: **Limpiar** las elimina y recargar la página vuelve a cargar el conjunto de ejemplo. La descarga externa de Chart.js y Google Fonts sí requiere conexión, pero no incluye los datos analizados.

## Archivos del proyecto

```text
Estadistica/
├── index.html                         Aplicación: HTML, CSS y JavaScript en un solo archivo
├── imagenes/
│   ├── portada.png                    Imagen de cubierta
│   └── hojas_cuaderno.png              Fondo de las páginas interiores
└── Agente_Estadistica/
    ├── config/                         Prompt y configuración conceptual del agente
    ├── conocimientos/                  Teoría y requerimientos estadísticos/software
    ├── prompts_ejemplo/                Casos para probar respuestas educativas
    └── README.md                       Documentación específica del material del agente
```

La carpeta `Agente_Estadistica` contiene material de referencia para un tutor educativo. No es necesaria para ejecutar el cuaderno web.

## Mapa del código

El código de la aplicación está concentrado en `index.html`:

- `<head>`: metadatos y dependencias remotas.
- `<style>`: variables de color, estilos de controles y páginas, y reglas adaptables a escritorio y móvil.
- `<main>`: cubierta, ocho páginas con sus fórmulas y controles de navegación.
- Primer bloque del `<script>`: conjunto de ejemplo, estado y formato de números.
- Funciones de entrada y cálculo: validación numérica, cuantiles, frecuencias, momentos y otras medidas.
- Funciones de presentación: actualización de resultados, avisos, tablas y gráficas.
- Funciones de importación: lectura de CSV/TXT, separación de columnas y conversión de celdas.
- Funciones de navegación: paginación, modo doble en escritorio, pantalla completa y atajos.

El mismo archivo incluye comentarios de orientación antes de los principales componentes y funciones. Al cambiar una fórmula, revisa tanto su cálculo como la explicación visible en la página y este README.
