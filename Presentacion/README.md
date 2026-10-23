# Guion de la presentación

Este guion acompaña las siete diapositivas de [Analisis_Favorita.pptx](Analisis_Favorita.pptx). Duración orientativa: entre cinco y seis minutos, según las pausas y las preguntas.

## Procedencia de los datos

**Sí, descargamos datos reales de Corporación Favorita.** Descargamos el histórico completo y después seleccionamos de `train.csv` las ventas de la **tienda 44**, desde el **22 de mayo hasta el 13 de agosto de 2017**. Guardamos esas filas en `Datasets/ventas_12_semanas.csv`.

Ese archivo contiene **84 días consecutivos, 12 semanas completas, 33 familias y 2.772 registros**. El recorte se obtuvo mediante un filtro del histórico, sin fabricar ventas. El notebook incluye el código y comprueba que el resultado coincide con el CSV entregado.

La fuente original es [Kaggle, Store Sales – Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data). La descarga utilizada fue el [ZIP de esta copia pública del dataset](https://raw.githubusercontent.com/kartikajls/Store-Sales-Time-Series-Forecasting/c6eac8b828c7c8231320dafbe0257f3466005fbe/store-sales-time-series-forecasting.zip). Conservamos los siete CSV originales localmente en `Datasets/`, excluidos de Git. El repositorio incluye el recorte `ventas_12_semanas.csv` para ejecutar la práctica.

**Frase para explicar la descarga:**

> Descargamos el dataset histórico de Corporación Favorita. A partir de su archivo de ventas, seleccionamos una tienda y doce semanas completas. Ese recorte conserva las ventas reales de las 33 familias y es el que utilizamos en el análisis.

## Diapositiva 1. Ventas y demanda con redes LSTM

**Qué decir:**

> Buenos días. En este trabajo analizamos las ventas de la tienda 44 de Corporación Favorita, ubicada en Quito según los metadatos del dataset. Elegimos doce semanas completas de 2017, del 22 de mayo al 13 de agosto.
>
> Tenemos 2.772 registros de 33 familias de productos. Para los gráficos nos concentramos en seis categorías. Buscamos conocer cómo cambian sus ventas y evaluar una red LSTM para estimar la demanda del día siguiente.

**Qué señalar:** las doce semanas, el periodo y la cantidad de registros. El número 2.772 corresponde a filas de datos, no a clientes ni a compras individuales.

**Transición:** “Primero revisamos las cantidades vendidas por categoría”.

## Diapositiva 2. Ventas por categoría

**Qué decir:**

> Sumamos las ventas de las doce semanas para bebidas, frutas y verduras, lácteos, limpieza, panadería y carnes. Bebidas y frutas y verduras registran las mayores cantidades en la escala del dataset.
>
> Por ejemplo, bebidas acumula 895.564. Debemos interpretar esta cifra con cuidado: el archivo registra cantidades vendidas y algunas categorías incluyen productos por peso. Por eso estas barras no comparan ingresos en dólares y tampoco indican qué categoría genera más ganancias.

**Qué señalar:** las dos barras rojas y la aclaración sobre las unidades. Basta mencionar un total como ejemplo; no hace falta leer las seis cifras.

**Transición:** “El acumulado muestra cuánto se vendió, y la siguiente diapositiva muestra cómo cambió durante las semanas”.

## Diapositiva 3. Evolución durante las 12 semanas

**Qué decir:**

> Agrupamos las ventas en semanas de lunes a domingo. Cada punto de estos gráficos contiene siete días completos.
>
> Bebidas y lácteos alcanzan su mayor venta semanal en la semana dos. Las otras cuatro categorías alcanzan su máximo en la semana seis. Esto muestra que las familias no se comportan exactamente igual.
>
> Cada gráfico tiene su propia escala vertical. Debemos observar las subidas y bajadas dentro de cada categoría, sin comparar directamente la altura de las líneas entre gráficos.

**Qué señalar:** el máximo de bebidas en la semana dos y el de limpieza en la semana seis. Los números del eje horizontal son semanas del análisis.

**Transición:** “También revisamos si había un patrón que se repitiera por día de la semana”.

## Diapositiva 4. El domingo lidera en cinco categorías

**Qué decir:**

> Calculamos el promedio de ventas de cada día de la semana para cada familia. Después lo expresamos como un índice donde cien representa el promedio diario de esa misma categoría.
>
> El domingo presenta el mayor promedio en cinco de las seis categorías. En bebidas, la venta media del domingo está un 46,6% por encima de su promedio diario. Carnes tiene un comportamiento diferente: su máximo ocurre el viernes y está un 67,6% por encima de su promedio.

**Qué señalar:** las cinco celdas rojas del domingo y la celda roja de carnes del viernes. El 147 de bebidas es un índice redondeado, no una cantidad de productos.

**Transición:** “Además del patrón semanal, comparamos el inicio y el final del periodo”.

## Diapositiva 5. Cinco categorías bajan al cierre

**Qué decir:**

> Comparamos el promedio semanal de las primeras cuatro semanas con el de las últimas cuatro. En este gráfico, cien por ciento representa el nivel del inicio.
>
> Cinco categorías terminan por debajo de ese nivel. Lácteos tiene la mayor caída, del 12,4%, y bebidas disminuye aproximadamente un 10%. Limpieza es la única que aumenta, con un 1,4%.
>
> Son resultados de esta tienda y de estas doce semanas. El análisis no demuestra qué causó los cambios ni permite generalizarlos a todo Ecuador.

**Qué señalar:** 87,6% en lácteos y 101,4% en limpieza. **87,6% es el nivel final respecto al inicio; 12,4% es la caída.**

**Transición:** “Con este historial entrenamos la red y comprobamos su error”.

## Diapositiva 6. La LSTM mejora solo en bebidas

**Qué decir:**

> Conservamos la LSTM del ejercicio original. Para cada predicción usamos seis días anteriores. Entrenamos con las primeras ocho semanas y evaluamos con las últimas cuatro, respetando el orden del tiempo.
>
> Medimos el error con RMSE y comparamos la red con una referencia sencilla: repetir la venta de hace siete días. Un RMSE menor significa menos error dentro de la misma categoría.
>
> Entre las tres categorías evaluadas, la LSTM mejora en bebidas: reduce el RMSE un 18,1% frente a la referencia. En lácteos y frutas y verduras, la referencia semanal obtiene menos error. Por eso debemos comprobar el desempeño por categoría antes de elegir el modelo.

**Qué señalar:** la línea real y la estimada en bebidas, y luego el valor rojo de cada fila de la tabla. El rojo marca el menor error de esa fila.

**Si preguntan por la arquitectura:** “Usamos una capa LSTM de 50 unidades y una capa Dense de una salida, con MAE, Adam y 50 épocas. Los escaladores se ajustan solo con entrenamiento”.

**Transición:** “Finalmente, utilizamos las funciones del notebook para estimar la demanda siguiente”.

## Diapositiva 7. Estimación del stock necesario

**Qué decir:**

> Tenemos dos funciones que utilizan la misma LSTM. La primera, `predecir`, trabaja con una categoría y permite elegir las variables y la ventana. La segunda, `predecir_categorias`, devuelve una tabla para varias familias y reutiliza los modelos entrenados.
>
> La tabla muestra las estimaciones para el 14 de agosto de 2017, el día posterior al último registro analizado. Por ejemplo, bebidas tiene una estimación de 10.602,2 en la escala del dataset.
>
> En esta práctica llamamos stock a la demanda estimada que se desea cubrir. No conocemos las existencias reales de bodega. Concluimos que las ventas cambian por día y categoría, y que el error del pronóstico debe evaluarse antes de usarlo para planificar el abastecimiento. Muchas gracias.

**Qué señalar:** las dos funciones y la fecha de las predicciones. Las cifras de la tabla son estimaciones, no ventas observadas del 14 de agosto.

## Respuestas breves para las preguntas del profesor

### ¿La data de doce semanas se descargó?

Descargamos el histórico real de Favorita y extrajimos las doce semanas mediante un filtro por tienda y fechas. El resultado está guardado en `ventas_12_semanas.csv`. El notebook comprueba su coincidencia con las filas de `train.csv`.

### ¿Por qué hay 2.772 registros si son 84 días?

Hay una fila por día y familia. Son 84 días multiplicados por 33 familias: 2.772 registros de una misma tienda.

### ¿Utilizamos todos los archivos originales?

Los conservamos en `Datasets/`. El análisis usa las ventas y promociones del recorte. `stores.csv` permite identificar la ciudad de la tienda. Petróleo, feriados y transacciones no se incorporaron a las entradas de esta LSTM. `test.csv` y `sample_submission.csv` pertenecen a la competencia de Kaggle.

### ¿Stock es el inventario real?

En el ejercicio, stock representa cobertura de demanda estimada a partir de ventas. El dataset no registra inventario de bodega. Para calcular una compra necesitaríamos también existencias disponibles, pedidos pendientes, tiempos de entrega y una política de seguridad.

### ¿Se puede decir que las promociones causaron los picos?

Estos gráficos muestran patrones descriptivos. Haría falta otro análisis para demostrar una relación causal con las promociones.

### ¿Por qué la LSTM no gana en todas las categorías?

Una red más compleja no garantiza menos error. Usamos solo ocho semanas para entrenar y las categorías tienen comportamientos diferentes. La comparación semanal permite reconocer cuándo la red aporta una mejora y cuándo una alternativa sencilla funciona mejor.

### ¿Los datos son actuales?

Son datos históricos reales de 2017. La fecha de descarga no cambia el periodo de las ventas analizadas.

### ¿Cuánto hay que subir a Colab?

Para ejecutar el análisis basta el notebook y `ventas_12_semanas.csv`. Si queremos reproducir también la extracción desde el histórico, subimos además `train.csv` y `stores.csv`.

## Antes de exponer

- Ensaya siguiendo las siete diapositivas y utiliza este guion como apoyo.
- Explica el significado de los índices y de los porcentajes, sin leer todas las cifras.
- Recuerda las dos limitaciones principales: las cantidades no son ingresos y stock no es inventario observado.
- Si la ejecución se interrumpe, las diapositivas y el notebook ya conservan los resultados del análisis.
