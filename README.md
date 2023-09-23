Se nos da una data sobre la plataforma de Steam. La data se nos da comprimida y básicamente consiste en en tres archivos .json. En la carpeta 'Guía_archivos_Diccionarios', dentro de la carpeta 'Antes_de' hay tres .txt que son justamente una explicación detallada de cada archivo .json.

Entonces lo primero que vamos a hacer es realizar el proceso de ETL de cada uno de estos archivo, por tanto, he creado tres .ipynb que representan el proceso de ETL + EDA de cada archivo .json. Al finalizar todo el proceso de ETL + EDA lo que obtendremos son archivos .parquet y .json que se situarán en Data_Consumible; efectivamente, estos datos están ya preprocesados como para ser consumidos por las funciones de la API y por el sistema de recomendación. Se pueden ver más detalles acerca de esta data 'consumible' en dónde dice 'Despues_De', que de la misma forma, son archivos .txt que explican la estructura de cada archivo respectivamente según el nombre.

ETL:
En el proceso de ETL se hace lo usual, se tratan los nulos, se procesan columnas, se eliminan columnas innecesarias, se reindexa, ...

EDA:
En este proceso, no hay mucha documentación en los archivos .ipynb ya que el trabajo ha sido más mental al momento de seleccionar características. La idea o el argumento de la selección de las características es la siguiente: 'Si se va a implementar un sistema de recomendación, lo que se prioriza es que, efectivamente, las recomendaciones sean del agrado del usuario, lo que a su vez es fácil intuir que lo que más representa el 'agrado' del usuario se encuentra en las especificaciones'. Por ejemplo, a mis hermanos y a mí nos gusta el juego de '0ad' que es un juego de estrategia, dónde escoges una civilización y la idea es derrotar a tu rival y para ello hay una secuencia de crecimiento económico que puede apoyar a esta conquista, digamos que es algo como 'Age of Empires', sin embargo, lo que más nos ha gustado del juego es la dinámica que se ha preestablecido a partir de la programación, además de su realismo notable en cuánto a gráficos, también el número de soldados que se pueden tener y por supuesto la posiblidad de jugar de forma online, y, en nuestro caso, que el juego es gratuito. 
Entonces la idea es que los juegos recomendados sean parecidos en cuanto a las especificaciones del juego ingresado, es por ello que solamente he considerado la columna de 'especificaciones' y no otras columnas. Por otro lado, era también posible utilizar la columna de review sobre el análisis de sentimientos para que se tenga estas 'dos características o vectores principales'. A partir de estos dos se podría establecer en el plano XY una región cuadrada con los vértices (0,0), (0,1), (1,1), (1,0), la idea es que en esta region se encontrará una nube de puntos, donde la primera componente será el grado de similitud del item con el item que se ingreso, y la segunda componente será algo así como el grado de fidelidad de esa similitud, este grado se obtendría a partir del análisis de sentimientos y se tendría que reescalar por supuesto para que este dentro del rango de 0 a 1. La cuestión es que el punto (1,1) iba a ser cuando tanto el titulo es totalmente igual al titulo ingresado y la fidelidad de este grado sean 100 confiables. Entonces para cada titulo que se ingrese en la funcion de recomendacion, se iba a graficar esta nube de puntos y se iban a escoger los puntos mas cercanos a (1,1) de tal forma que garanticemos que obtendremos las mejores recomendaciones.
Sin embargo he optado por no implementar esto ya que está desbalanceado las recomendaciones de los juegos, y tanta es la diferencia que no es posible ni siquiera realizar un rellenado a partir de un algoritmo de aprendizaje supervisado.
Finalmente el sistema de recomendación recomendará los titlos más parecidos al tútulo ingresado a partir de las especificaciones de los titulos, por lo cual se ha usado la similitud de Jaccard para este fin.
Como una conclusión importante, siento que el sistema de recomendación no es tan efectivo, es decir, no es tan preciso. Y la razón se debe a que realmente para realizar un sistema de recomendación según mi propuesta, es necesario requerir de muchas más especificaciones, quizás 1000 o menos de tal forma que el sistema de recomendación sería más parecido a un filtrado muy preciso que un modelo de aprendizaje de máquina.

















