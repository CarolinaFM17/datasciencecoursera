### Cómo compartir datos con un estadístico
Esta es una guía para cualquier persona que necesite compartir datos con un estadístico o científico de datos. El público objetivo al que me dirijo es:

- Colaboradores que necesitan estadísticos o científicos de datos para analizar los datos por ellos.
- Estudiantes o investigadores postdoctorales de diversas disciplinas que buscan asesoramiento.
- Estudiantes de estadística de nivel básico cuyo trabajo consiste en recopilar/limpiar/organizar conjuntos de datos.
El objetivo de esta guía es brindar instrucciones sobre la mejor manera de compartir datos para evitar los errores más comunes y las causas de retraso en la transición de la recopilación al análisis de datos. El grupo Leek trabaja con numerosos colaboradores, y la principal fuente de variación en la rapidez de obtención de resultados es el estado de los datos al llegar al grupo. Según mis conversaciones con otros estadísticos, esto es cierto prácticamente en todos los casos.

Considero fundamental que los estadísticos puedan manejar los datos en cualquier estado en que los reciban. Es importante visualizar los datos brutos, comprender los pasos del proceso y poder incorporar fuentes ocultas de variabilidad en el análisis. Por otro lado, para muchos tipos de datos, los pasos de procesamiento están bien documentados y estandarizados. Por lo tanto, la conversión de los datos brutos a un formato directamente analizable puede realizarse antes de consultar con un estadístico. Esto puede agilizar considerablemente el tiempo de respuesta, ya que el estadístico no tiene que realizar previamente todos los pasos de preprocesamiento.

### Lo que debes entregarle al estadístico
Para facilitar un análisis más eficiente y oportuno, esta es la información que debe proporcionar a un estadístico:

1. Los datos brutos.

2. Un conjunto de datos ordenado
3. Un libro de códigos que describe cada variable y sus valores en el conjunto de datos ordenado.
4. Una receta explícita y exacta que usaste para pasar de 1 a 2,3.

Analicemos cada parte del paquete de datos que transferirá.

# Los datos brutos
Es fundamental que incluyas los datos en su formato original al que tengas acceso. Esto garantiza que se pueda mantener la trazabilidad de los datos a lo largo de todo el flujo de trabajo. A continuación, se muestran algunos ejemplos de datos en formato original:

- El extraño archivo binario que genera su máquina de medición

- El archivo de Excel sin formato con 10 hojas de cálculo que la empresa con la que contrataste te envió.

- Los datos JSON complejos que obtuviste al extraer datos de la API de Twitter

- Los números introducidos manualmente que recopilaste mirando a través de un microscopio

Sabrás que los datos brutos están en el formato correcto si:

1. No se ejecutó ningún software sobre los datos.

2. No se modificó ninguno de los valores de los datos.

3. No eliminaste ningún dato del conjunto de datos.

4. No resumiste los datos de ninguna manera.

Si modificaste los datos originales, estos ya no se encuentran en su formato original. Presentar datos modificados como si fueran datos originales es una práctica común que ralentiza el análisis, ya que el analista a menudo tendrá que realizar un estudio forense para determinar por qué los datos originales parecen extraños. (¿Y qué pasaría si llegaran nuevos datos?)

# El conjunto de datos ordenado
Hadley Wickham expone los principios generales de los datos ordenados en este artículo y en este vídeo . Si bien tanto el artículo como el vídeo describen los datos ordenados utilizando R , los principios son de aplicación más general:

1. Cada variable que midas debe estar en una columna.

2. Cada observación diferente de esa variable debe estar en una fila diferente.

3. Debe haber una tabla para cada "tipo" de variable.

4. Si tiene varias tablas, estas deben incluir una columna que permita unirlas o fusionarlas.

Si bien estas son las reglas básicas, existen otras medidas que facilitarán considerablemente el manejo de sus datos. En primer lugar, incluya una fila en la parte superior de cada tabla u hoja de cálculo con los nombres completos de las filas. Por ejemplo, si midió la edad al momento del diagnóstico de los pacientes, encabezará esa columna con el nombre completo AgeAtDiagnosisen lugar de usar ADxuna abreviatura que podría resultar difícil de entender para otra persona.

Aquí hay un ejemplo de cómo funcionaría esto en genómica. Supongamos que para 20 personas ha recopilado mediciones de expresión genética con secuenciación de ARN . También ha recopilado información demográfica y clínica sobre los pacientes, incluyendo su edad, tratamiento y diagnóstico. Tendría una tabla/hoja de cálculo que contiene la información clínica/demográfica. Tendría cuatro columnas (identificador del paciente, edad, tratamiento, diagnóstico) y 21 filas (una fila con nombres de variables y luego una fila para cada paciente). También tendría una hoja de cálculo para los datos genómicos resumidos. Por lo general, este tipo de datos se resume al nivel del número de recuentos por exón. Supongamos que tiene 100 000 exones, entonces tendría una tabla/hoja de cálculo que tendría 21 filas (una fila para nombres de genes y una fila para cada paciente) y 100 001 columnas (una fila para identificadores de pacientes y una fila para cada tipo de dato).

Si comparte sus datos con el colaborador en Excel, los datos deben estar en un archivo de Excel por tabla. No deben contener varias hojas de cálculo, ni macros aplicadas, ni columnas o celdas resaltadas. Como alternativa, puede compartir los datos en un archivo CSV o de texto delimitado por tabulaciones . (Tenga en cuenta que la lectura de archivos CSV en Excel a veces puede provocar errores en el manejo de las variables de fecha y hora).

# El libro de códigos
Para casi cualquier conjunto de datos, las mediciones que calcule deberán describirse con más detalle del que puede o debe incluir en la hoja de cálculo. El libro de códigos contiene esta información. Como mínimo, debe contener:

1. Información sobre las variables (¡incluidas las unidades!) en el conjunto de datos que no está contenida en los datos ordenados.

2. Información sobre las opciones de resumen que usted seleccionó

3. Información sobre el diseño del estudio experimental que utilizó

En nuestro ejemplo de genómica, el analista querría saber cuál es la unidad de medida para cada variable clínica/demográfica (edad en años, tratamiento por nombre/dosis, nivel de diagnóstico y su heterogeneidad). También querría saber cómo se seleccionaron los exones utilizados para resumir los datos genómicos (UCSC/Ensembl, etc.). Asimismo, querría conocer cualquier otra información sobre cómo se realizó la recopilación de datos/diseño del estudio. Por ejemplo, ¿se trata de los primeros 20 pacientes que acudieron a la clínica? ¿Son 20 pacientes altamente seleccionados según alguna característica como la edad? ¿Se les asignó aleatoriamente a los tratamientos?

Un formato común para este documento es un archivo de Word. Debe incluir una sección titulada "Diseño del estudio" con una descripción detallada de cómo se recopilaron los datos. También debe haber una sección llamada "Libro de códigos" que describa cada variable y sus unidades.

# Cómo codificar variables
Cuando introduces variables en una hoja de cálculo, te encontrarás con varias categorías principales dependiendo de su tipo de datos :

1. Continuo

2. Ordinal

3. Categórico

4. Desaparecido

5. Censurado

Las variables continuas son cualquier cosa medida en una escala cuantitativa que puede ser cualquier número fraccionario. Un ejemplo sería algo como el peso medido en kg. Los datos ordinales son datos que tienen un número fijo y pequeño (< 100) de niveles pero están ordenados. Esto podría ser, por ejemplo, respuestas de encuestas donde las opciones son: malo, regular, bueno. Los datos categóricos son datos donde hay múltiples categorías, pero no están ordenadas. Un ejemplo sería el sexo: masculino o femenino. Esta codificación es atractiva porque es autodocumentada. Los datos faltantes son datos que no se observan y se desconoce el mecanismo. Debe codificar los valores faltantes como NA. Los datos censurados son datos donde se conoce el mecanismo de falta de algún nivel. Ejemplos comunes son una medición por debajo de un límite de detección o un paciente que se pierde en el seguimiento. También deben codificarse como NAcuando no se tienen los datos. Pero también debe agregar una nueva columna a sus datos ordenados llamada, "VariableNameCensored" que debe tener valores TRUEsi está censurado y FALSEsi no. En el libro de códigos debe explicar por qué faltan esos valores. Es fundamental informar al analista si existe algún motivo por el que falten algunos datos. Tampoco debe imputar , inventar ni descartar las observaciones faltantes.

En general, evite codificar las variables categóricas u ordinales como números. Al ingresar el valor de sexo en los datos ordenados, debe ser "masculino" o "femenino". Los valores ordinales en el conjunto de datos deben ser "malo", "regular" y "bueno", no 1, 2 y 3. Esto evitará posibles confusiones sobre la dirección de los efectos y ayudará a identificar errores de codificación.

Codifique siempre toda la información sobre sus observaciones mediante texto. Por ejemplo, si almacena datos en Excel y utiliza texto de color o formato de fondo de celda para indicar información sobre una observación («Se observaron entradas de variables rojas en el experimento 1»), esta información no se exportará (¡y se perderá!) al exportar los datos como texto sin formato. Todos los datos deben codificarse como texto que pueda exportarse.

# La lista/guion de instrucciones
Quizás ya lo hayas oído, pero la reproducibilidad es fundamental en la ciencia computacional . Esto significa que, al enviar tu artículo, los revisores y el resto de la comunidad científica deben poder replicar con exactitud los análisis, desde los datos brutos hasta los resultados finales. Si buscas la eficiencia, probablemente realizarás algunos pasos de resumen y análisis de datos antes de que estos se consideren ordenados.

Lo ideal para realizar un resumen es crear un script informático (en R, Python, o cualquier otro lenguaje) que tome los datos sin procesar como entrada y genere los datos ordenados que deseas compartir como salida. Puedes ejecutar el script varias veces para comprobar si el código produce el mismo resultado.

En muchos casos, la persona que recopiló los datos tiene incentivos para facilitar el trabajo del estadístico y así agilizar la colaboración. Es posible que no sepa programar en un lenguaje de scripting. En ese caso, lo que debe proporcionar al estadístico es pseudocódigo . Debería tener un aspecto similar a este:

1. Paso 1: tome el archivo sin procesar, ejecute la versión 3.1.2 del software summarize con los parámetros a=1, b=2, c=3.

2. Paso 2: ejecute el software por separado para cada muestra.

3. Paso 3: tome la tercera columna del archivo outputfile.out para cada muestra y esa será la fila correspondiente en el conjunto de datos de salida.

También debes incluir información sobre el sistema operativo (Mac/Windows/Linux) en el que utilizaste el software y si lo probaste más de una vez para confirmar que obtuviste los mismos resultados. Lo ideal es que se lo muestres a un compañero de clase o de laboratorio para confirmar que puede obtener el mismo archivo de salida que tú.

### Qué debes esperar del analista
Al entregar un conjunto de datos debidamente organizado, se reduce drásticamente la carga de trabajo del estadístico. Por lo tanto, es de esperar que le respondan mucho antes. Sin embargo, la mayoría de los estadísticos meticulosos revisarán su procedimiento, harán preguntas sobre los pasos que realizó e intentarán confirmar que pueden obtener los mismos datos organizados que usted, al menos mediante verificaciones puntuales.

Entonces, lo que se puede esperar del estadístico es lo siguiente:

1. Un script de análisis que realiza cada uno de los análisis (no solo las instrucciones).

2. El código informático exacto que utilizaron para realizar el análisis.

3. Todos los archivos/figuras de salida que generaron.

Esta es la información que utilizará en el suplemento para demostrar la reproducibilidad y precisión de sus resultados. Cada paso del análisis debe explicarse claramente y debe hacer preguntas cuando no entienda lo que hizo el analista. Es responsabilidad tanto del estadístico como del científico comprender el análisis estadístico. Es posible que no pueda realizar los análisis exactos sin el código del estadístico, pero debe poder explicar a un compañero de laboratorio o a su investigador principal por qué el estadístico realizó cada paso.

### Colaboradores
- Jeff Leek - Escribió la versión inicial.

- L. Collado-Torres - Se corrigieron errores tipográficos y se agregaron enlaces.

- Nick Reich - Añadió consejos sobre cómo almacenar datos como texto.

- Nick Horton - Sugerencias menores de redacción.

Paso 2: ejecute el software por separado para cada muestra.
Paso 3: tome la tercera columna del archivo outputfile.out para cada muestra y esa será la fila correspondiente en el conjunto de datos de salida.
También debes incluir información sobre el sistema operativo (Mac/Windows/Linux) en el que utilizaste el software y si lo probaste más de una vez para confirmar que obtuviste los mismos resultados. Lo ideal es que se lo muestres a un compañero de clase o de laboratorio para confirmar que puede obtener el mismo archivo de salida que tú.
