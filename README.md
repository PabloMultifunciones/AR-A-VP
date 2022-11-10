# AR-A-VP
Aprendizaje Reforzado - Adicional - Vector de Palabras
### Introduccion ###
En un nivel, es simplemente un vector de pesos. En una codificación simple 1-de-N (o 'one-hot'), cada elemento en el vector está asociado con una palabra en el vocabulario. La codificación de una palabra dada es simplemente el vector en el que el elemento correspondiente se establece en uno, y todos los demás elementos son cero.  

Supongamos que nuestro vocabulario tiene solo cinco palabras: Rey, Reina, Hombre, Mujer y Niño. Podríamos codificar la palabra 'Reina' como:  

<img width="474" alt="word2vec-one-hot" src="https://user-images.githubusercontent.com/95035101/201149887-05c1bb8e-740e-48c8-b956-11a8c31e2a60.png">

Usando tal codificación, no hay una comparación significativa que podamos hacer entre los vectores de palabras que no sean las pruebas de igualdad.  

En word2vec, se utiliza una representación distribuida de una palabra. Tome un vector con varios cientos de dimensiones (digamos 1000). Cada palabra está representada por una distribución de pesos entre esos elementos. Entonces, en lugar de un mapeo uno a uno entre un elemento en el vector y una palabra, la representación de una palabra se extiende a través de todos los elementos en el vector, y cada elemento en el vector contribuye a la definición de muchas palabras.  

Si etiqueto las dimensiones en un vector de palabra hipotético (por supuesto, no hay tales etiquetas preasignadas en el algoritmo), podría verse un poco así:  

<img width="705" alt="word2vec-distributed-representation" src="https://user-images.githubusercontent.com/95035101/201150036-9b07cc17-ea22-4cdc-a593-5612dee26af9.png">

Tal vector viene a representar de alguna manera abstracta el "significado" de una palabra. Y como veremos a continuación, simplemente examinando un gran corpus es posible aprender vectores de palabras que son capaces de capturar las relaciones entre palabras de una manera sorprendentemente expresiva. También podemos usar los vectores como entradas a una red neuronal.  

### Razonamiento con vectores de palabras ###

" Encontramos que las representaciones de palabras aprendidas de hecho capturan regularidades sintácticas y semánticas significativas de una manera muy simple. Específicamente, las regularidades se observan como desplazamientos vectoriales constantes entre pares de palabras que comparten una relación particular. Por ejemplo, si denotamos el vector de la palabra i como xi y nos enfocamos en la relación singular/plural, observamos que xapple – xapples ≈ xcar – xcars, xfamily – xfamilies ≈ xcar – xcars, y así sucesivamente. Quizás lo más sorprendente es que encontramos que este también es el caso para una variedad de relaciones semánticas, según lo medido por la tarea SemEval 2012 de medir la similitud de la relación."

Los vectores son muy buenos para responder preguntas de analogía de la forma a es a b como c es a ?. Por ejemplo, ¿el hombre es a la mujer lo que el tío es al ? (tía) usando un método simple de desplazamiento vectorial basado en la distancia del coseno.  

Por ejemplo, aquí hay compensaciones de vectores para tres pares de palabras que ilustran la relación de género:  

<img width="298" alt="word2vec-gender-relation" src="https://user-images.githubusercontent.com/95035101/201152739-fdf36677-8436-452a-918d-d8d9c814bda6.png">

Y aquí vemos la relación singular plural:  

<img width="305" alt="word2vec-plural-relation" src="https://user-images.githubusercontent.com/95035101/201152895-f4cb9f29-4420-41ac-857d-1b3f2bc02273.png">

Este tipo de composición vectorial también nos permite responder “Rey – Hombre + Mujer = ?” pregunta y llega al resultado "Reina"! Todo lo cual es realmente notable cuando piensas que todo este conocimiento simplemente proviene de mirar muchas palabras en contexto (como veremos pronto) sin proporcionar otra información sobre su semántica.

"Sorprendentemente, se encontró que la similitud de las representaciones de las palabras va más allá de las simples regularidades sintácticas. Usando una técnica de compensación de palabras donde se realizan operaciones algebraicas simples en los vectores de palabras, se demostró, por ejemplo, que vector ("Rey") - vector ("Hombre") + vector ("Mujer") da como resultado un vector que es más cercano a la representación vectorial de la palabra Reina."

Vectores para rey, hombre, reina y mujer:

<img width="412" alt="word2vec-king-queen-vectors" src="https://user-images.githubusercontent.com/95035101/201153256-25ea0f2e-6659-42c4-a9aa-d7f15cac81c3.png">

El resultado de la composición vectorial Rey – Hombre + Mujer = ?

<img width="442" alt="word2vec-king-queen-composition" src="https://user-images.githubusercontent.com/95035101/201153409-a98fb1ab-9f0a-4eb5-9974-4392dbfab106.png">

Aquí hay algunos resultados más logrados usando la misma técnica:  

<img width="1118" alt="word2vec-ee-table-8" src="https://user-images.githubusercontent.com/95035101/201153596-e44ed864-77c4-461c-b609-9a7cb8136228.png">

Así es como se ve la relación país-ciudad capital en una proyección PCA bidimensional:

<img width="1149" alt="word2vec-dr-fig-2" src="https://user-images.githubusercontent.com/95035101/201153722-9dfbf408-bcb0-4fe6-af07-4deeda104b93.png">

Aquí hay algunos ejemplos más de las preguntas de estilo 'a es a b como c es a?' respondidas por vectores de palabras:  

<img width="1132" alt="word2vec-dr-table-2" src="https://user-images.githubusercontent.com/95035101/201153879-5e8653e9-cae3-40a5-8f53-8c275adefa98.png">

También podemos usar la adición de elementos vectoriales para hacer preguntas como "Aerolíneas alemanas +" y, al observar los tokens más cercanos al vector compuesto, obtendremos respuestas impresionantes:

<img width="1129" alt="word2vec-dr-table-5" src="https://user-images.githubusercontent.com/95035101/201153937-eaef54cc-b815-49b2-abaf-633ef1e8d3dc.png">

Los vectores de palabras con tales relaciones semánticas podrían usarse para mejorar muchas aplicaciones de PNL existentes, como la traducción automática, la recuperación de información y los sistemas de respuesta a preguntas, y pueden permitir que se inventen otras aplicaciones futuras.  

La relación semántica-sintáctica de palabras evalúa la comprensión de una amplia variedad de relaciones, como se muestra a continuación. Utilizando vectores de palabras de 640 dimensiones, un modelo entrenado en skip-gram logró una precisión semántica del 55 % y una precisión sintáctica del 59 %.  

<img width="1116" alt="word2vec-efficient-estimation-table-3" src="https://user-images.githubusercontent.com/95035101/201154087-4a8b7b7a-3028-4931-a754-03148fef7e0a.png">

### Aprender vectores de palabras ###

Mikolov et al. no fueron los primeros en usar representaciones vectoriales continuas de palabras, pero sí mostraron cómo reducir la complejidad computacional del aprendizaje de tales representaciones, lo que hace que sea práctico aprender vectores de palabras de alta dimensión en una gran cantidad de datos. Por ejemplo, “Hemos utilizado un corpus de Google News para entrenar los vectores de palabras. Este corpus contiene alrededor de 6B tokens. Hemos restringido el tamaño del vocabulario a 1 millón de palabras más frecuentes…”  

La complejidad en los modelos de lenguaje de redes neuronales (feedforward o recurrente) proviene de las capas ocultas no lineales.  

"Si bien esto es lo que hace que las redes neuronales sean tan atractivas, decidimos explorar modelos más simples que tal vez no puedan representar los datos con tanta precisión como las redes neuronales, pero que posiblemente puedan entrenarse con muchos más datos de manera eficiente."  

Se proponen dos nuevas arquitecturas: un modelo continuo de bolsa de palabras y un modelo continuo de salto de gramo. Veamos primero el modelo de bolsa de palabras continua (CBOW).   

Considere una pieza de prosa como "El modelo Skip-gram continuo introducido recientemente es un método eficiente para aprender representaciones vectoriales distribuidas de alta calidad que capturan una gran cantidad de relaciones de palabras sintácticas y semánticas precisas". Imagine una ventana deslizante sobre el texto, que incluye la palabra central actualmente en foco, junto con las cuatro palabras que la preceden, y las cuatro palabras que la siguen:   

<img width="699" alt="word2vec-context-words" src="https://user-images.githubusercontent.com/95035101/201157500-b849b199-73b4-4c89-8287-25adb20308bd.png">

Las palabras de contexto forman la capa de entrada. Cada palabra está codificada en forma one-hot, por lo que si el tamaño del vocabulario es V, estos serán vectores de dimensión V con solo uno de los elementos establecido en uno y el resto en ceros. Hay una sola capa oculta y una capa de salida.   

<img width="565" alt="word2vec-cbow" src="https://user-images.githubusercontent.com/95035101/201157693-d7143e02-3ad9-4150-8619-db847d2cc1ac.png">

El objetivo de entrenamiento es maximizar la probabilidad condicional de observar la palabra de salida real (la palabra de enfoque) dadas las palabras de contexto de entrada, con respecto a los pesos. En nuestro ejemplo, dada la entrada ("un", "eficiente", "método", "para", "alto", "calidad", "distribuido", "vector") queremos maximizar la probabilidad de obtener "aprendizaje ” como Salida.  
 
Dado que nuestros vectores de entrada son uno-caliente, multiplicar un vector de entrada por la matriz de peso W1 equivale simplemente a seleccionar una fila de W1.  

<img width="600" alt="word2vec-linear-activitation" src="https://user-images.githubusercontent.com/95035101/201157870-f7471faf-a662-4975-a1f0-6eb933102476.png">

Dados los vectores de palabras de entrada C, la función de activación para la capa oculta h equivale a simplemente sumar las filas "calientes" correspondientes en W1 y dividirlas por C para obtener su promedio.  

"Esto implica que la función de enlace (activación) de las unidades de la capa oculta es simplemente lineal (es decir, pasa directamente su suma ponderada de entradas a la siguiente capa)."  

Desde la capa oculta hasta la capa de salida, la segunda matriz de ponderación W2 se puede usar para calcular una puntuación para cada palabra del vocabulario, y softmax se puede usar para obtener la distribución posterior de las palabras.  

El modelo skip-gram es lo opuesto al modelo CBOW. Se construye con la palabra de enfoque como vector de entrada único, y las palabras de contexto de destino ahora están en la capa de salida:  

<img width="474" alt="word2vec-skip-gram" src="https://user-images.githubusercontent.com/95035101/201158200-d26e91f5-1a14-4e67-8dfa-a1635177bdaf.png">

La función de activación de la capa oculta consiste simplemente en copiar la fila correspondiente de la matriz de pesos W1 (lineal) como vimos antes. En la capa de salida, ahora generamos distribuciones multinomiales C en lugar de solo una. El objetivo de entrenamiento es minimizar el error de predicción sumado en todas las palabras de contexto en la capa de salida. En nuestro ejemplo, la entrada sería "aprendizaje", y esperamos ver ("un", "eficiente", "método", "para", "alta", "calidad", "distribuido", "vector") en la capa de salida.  

###Optimizaciones ###

Tener que actualizar cada vector de palabra de salida para cada palabra en una instancia de entrenamiento es muy costoso...  

Para resolver este problema, una intuición es limitar la cantidad de vectores de salida que deben actualizarse por instancia de entrenamiento. Un enfoque elegante para lograr esto es el softmax jerárquico; otro enfoque es a través del muestreo.  

Softmax jerárquico utiliza un árbol binario para representar todas las palabras del vocabulario. Las palabras mismas son hojas en el árbol. Para cada hoja, existe un camino único desde la raíz hasta la hoja, y este camino se usa para estimar la probabilidad de la palabra representada por la hoja. "Definimos esta probabilidad como la probabilidad de que un paseo aleatorio comience desde la raíz y termine en la hoja en cuestión".  

<img width="1206" alt="word2vec-hierarchical-softmax" src="https://user-images.githubusercontent.com/95035101/201158516-085c8652-1dd7-40ee-b5cc-4887a33d56de.png">

La principal ventaja es que en lugar de evaluar V nodos de salida en la red neuronal para obtener la distribución de probabilidad, se necesita evaluar solo sobre log2(V) palabras… En nuestro trabajo usamos un árbol binario de Huffman, ya que asigna códigos cortos a las palabras frecuentes lo que resulta en un entrenamiento rápido.  

El muestreo negativo es simplemente la idea de que solo actualizamos una muestra de palabras de salida por iteración. La palabra de salida objetivo debe mantenerse en la muestra y se actualiza, y agregamos a esto algunas palabras (no objetivo) como muestras negativas. "Se necesita una distribución probabilística para el proceso de muestreo, y puede elegirse arbitrariamente... Uno puede determinar una buena distribución empíricamente".  

Mikolov et al. también use un enfoque de submuestreo simple para contrarrestar el desequilibrio entre palabras raras y frecuentes en el conjunto de entrenamiento (por ejemplo, "en", "el" y "a" proporcionan menos valor de información que las palabras raras). Cada palabra del conjunto de entrenamiento se descarta con probabilidad P(wi) donde

<img width="346" alt="word2vec-subsampling" src="https://user-images.githubusercontent.com/95035101/201158837-a7d79d53-5146-4bac-90ad-9bdb4a67ca78.png">

f(wi) es la frecuencia de la palabra wi y t es un umbral elegido, típicamente alrededor de 10-5.  


