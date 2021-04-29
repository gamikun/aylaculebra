0. Ya hacía tiempo que quería hacer un tutorial, un tutorial para crear algún juego, y al final decidí hacerl un snake, bueno, ya sin tirar más rollo, vamos a comenzar.
1. Primero que nada, Voy a comenzar con la estructura HTML.
2. Le voy a dar estilo.
	2.1. Primero le pondré el fondo al body.
	2.2. Lo voy a alinear al centro para que esté centrado el contenedor del juego.
	2.3. A los canvas le daré un borde grueso de color blanco para que se note en el juego.
	2.4. Y de fondo un degradado que va de un tono de azul a un morado.
3. Ahora continúo con con la creación del elemento canvas que voy a
   utilizar como contenedor del juego y para dibujar en él. Le pondré
   un ancho y un alto de 500 píxeles.
4. Lo siguiente es comenzar con la sección importante del juego que es el script.
5. Voy a comenzar con una serie de constantes que nos serán importantes para la mecánica
   y funcionamiento general del juego.
   5.1. Las constante STATE_RUNNING y STATE_LOSING, que van a determinar cuando el juego está
   		corriendo, o está en fase de perdiendo, respectivamente.
   5.2. La constante TICK determinará los intervalos de tiempo en que se hará recálculo y
   		desplazamiento, dicho de otro modo la "velocidad".
   5.3. Seguimos con SQUARE_SIZE, la cual determina el ancho y el alto en píxeles de cada
   		cuadrito que dibujemos sobre nuestro canvas.
   5.4. Para determinar el ancho y alto virtual de nuestra espacio de juego, es decir cuantos
   		cuadritos cabrán en él, voy a declarar e inicializar BOARD_WIDTH y BOARD_HEIGHT.
   5.5. La constante GROW_SCALE determinará cuantos en cuantos cuadros de longitud
   		crecerá la serpiente cada vez que consuma una presa.
   5.6. Como la última de las constantes es un mapa de dirección de desplazamiento
   		de acuerdo a la tecla que es presionada. Para eso creo un objeto y lo voy
   		llenando con las letras de las teclas que utilizaré para el movimiento.
   		Cada elemento contendrá un arreglo que a su vez contendrán 2 elementos,
   		donde el primero es el desplazamiento en X y la segunda en Y. Las letras
   		se repetirán en minúsculas y mayúsculas.
 6. Nuestro siguiente paso es definir las variables del estado del juego, para eso voy
 	a inicializar la variable "state", la cual contendrá todos las variables que tienen
 	que ver con la lógica del juego.
 	6.1. "canvas" contendrá la referencia al elemento HTML del canvas que creamos previamente.
 	6.2. "context" será derivado de la variable canvas, más adelante veremos cómo.
 	6.3. La variable "snake" es un arreglo que contiene las posiciones de todos los puntos
 	     en el espacio de juego donde se encuentra la serpiente. Su valor default será 0,0
 	     pero nos encargaremos de darle un valor aleatorio más adelante.
 	6.4. Para saber qué dirección lleva la serpiente actualmente, guardaremos el valor
         en la variable "direction", la cual es solo un objeto con las propiedades x y y.
    6.5. Prey será similar a la anterior, una posición en x y yo, ya que solo tendremos
         una presa a la vez.
    6.6. La variable "growing" puede que sea la más difícil de explicar, esta determina
         cuantos cuadros hay pendientes de crecer, y se va disminuyendo en cada "tick".
    6.7. Como te había mencionado anteriormente con STATE_RUNNING y STATE_LOSING, es
         cualquiera de estos dos valores que la variable "runState" tomará. Su valor
         default es STATE_RUNNING.

-7. Ahora voy crear una función que genera posiciones aleatorias. Creo que es muy sencilla.
7. La función principal de ejecución, misma que hará efecto cada intervalo del juego,
   será la que llamaremos "tick", que por ahora la voy a definir vacía para poder
   continuar con la inicialización.
8. Para inicializar las variables necesarias, voy a llenar el evento "onload" de
   la variable global "window".
   8.1. Lo primero que haremos en la inicialización, es referenciar el elementos
        canvas previamente definido en el HTML. Para eso voy a utilizar la función
        document.querySelector y le voy proporcionar el tipo de elemento, que en este
        caso es 'canvas'. Como solo tenemos uno, lo tomará sin problemas.
   8.2. Justo después de esto continuo con la referenciación del contexto de dibujo,
        extraido precisamente del elemento html canvas. Esto se guardará en el state.context
        y lo llamaremos desde la función getContext('2d') del canvas referenciado previamente.
   8.3. Bueno, ahora haremos el mecanismo de presión de teclas, utilizando el evento onkeydown
        de la variable "window". Dentro de este voy a utilizar la constante de DIRECTIONS_MAP
        que previamente definimos en la sección de constantes, y utilizando al propiedad "key"
        del evento del teclado que recibimos dentro de la función actual, podemos devolver
        el arreglo con el desplazamiento de acuerdo a la tecla. Si la tecla presionada es
        encontrada dentro del DIRECTIONS_MAP, sigue hacer una simple comparación, con la cual
        solo debemos estar seguro que la teclado presionada no es en dirección contraria a 
        donde se está desplazando actualmente la serpiente, si esto es así, entonces se procede
        a asignar la nueva dirección, la misma que pasamos desde la variable extraida de
        DIRECTIONS_MAP hacia "directions" del state.
   8.4. Por último llamaremos a nuestra función de cada ciclo de juego llamada "tick",
        la cual al estar todavía vacía, no hará nada hasta que pasemos al siguiente paso.
9. Hasta este punto podemos actualizarlo en el navegador para ver nuestro cuadro con degradado,
   aunque claramente no se verá funcionando todavía.
10. A continuación voy a comenzar con la definción de las dos funciones que harán el trabajo
    de dibujado, las cuales son "drawPixel" y "draw". Debido a que "draw" depende de "drawPixel",
    comenzaremos con "drawPixel".
    10.1. Prácticamente lo que hará drawPixel es dibujar cada uno de los cuadritos que componen
          ya sea la serpiente o a la presa. Esta función recibirá como primer parámetro el
          color, el segundo es "x" y el tercero es "y". Dichas posiciones son en la escala
          de los cuadritos, no es en la escala absoluta del canvas, estas últimas serán
          precisamente determinadas dentro de esta función.
          Bien... lo primero que voy a hacer dentro de la función es cambiar el fillStyle del
          contexto, asignándole la variable que viene del parámetro "color".
          Después solo llamaré a la función fillRect del mismo contexto, donde le proporciono
          primeramente la posición de "x", la cual es "x" multiplicada por SQUARE_SIZE.
          Lo mismo hacemos con "y".
          Para los últimos parámetros que son el ancho y el alto, simplemente le daremos
          SQUARE_SIZE de manera directa.
          Con esto ya tenemos completa nuestra función drawPixel.
    10.2. Ahora voy a definir y comenzar con la función principal de dibujado que
          llamaremos "draw".
          Esta función no recibirá ningún parámetro, ya que esta depende de algunas de
          las variables que ya se encuentra dentro del "state".
          Ya entrando en detalles, lo primero que necesitamos hacer es borrar el contexto
          para poder dibujar las cosas nuevas, para eso llamamos a la función clearRect,
          que pedirá la posición y el tamaño... le daremos la posición 0, 0,
          y de tamaño 500 y 500.
          Continuo con un recorrido por la variable snake, la cual son cada uno de los
          puntos que abarca la serpiente en pantalla, para eso voy a hacer un recorrido
          básico con un for... y ya dentro tomo los valores de "x" y "y" utilizando una
          asignación desestructurante, cuyos valores vienen de el elemento con el índice
          "idx" de la variable snake. Para cada uno de estos elementos llamo a la función
          que creamos antes que es "drawPixel", dándole el color verde y la posición.
          Habiendo dibujado la serpiente, solo queda dibujar a la presa. Para esto voy a
          utilizar la asignación desestructurante, pero en este caso tomándolo de la variable
          "prey" del state. Y ya para dibujara llamo a la misma función de antes, "drawPixel",
          solo que esta vez con un color amarillo para que se diferencíe de la serpiente.
11. A partir de aquí ya no moveremos nada sobre el dibujo, lo demás será solo lógica del juego.
12. Vamos a regresarnos hacia dentro de la función "tick", y nos vamos a asegurar de que esta
    se puede volver a ejecutar a sí misma, o bueno, más bien volverse a calendarizar a sí misma
    debido a la naturaleza asíncrona de javascript.
        12.1 Para esto necesitamos llamar a la función setTimeout, donde el primero parámetro
             es la función misma, o sea tick y el segundo una variable que definiremos dentro
             de la misma función actual, llamada "interval"... Para que esto siga teniendo
             sentido, pues nos vamos al inicio de la función para declarar esta variable.
             Su valor default va a ser igual a la constante TICK en mayúsculas.
        12.2 Aunque la función principal de la lógica se esté repitiendo, todavía nos haría
             falta estar llamando a la función principal de dibujado. Para eso vamos a utilizar
             la función requestAnimationFrame, la cual va a programar la ejecución del dibujado,
             le vamos a dar como único parámetro la función de dibujado, de manera que cada
             vez que se concluya la ejecución de la función "tick", solicitaremos que se dibuje.
13. Hasta este punto si vas al navegador y actualizas la página, ya se debe estar dibujando una
    presa y uan víbora de 1 cuadro de largo. Sin embargo nos hace falta toda la lógica del juego.
    Esta lógica la podríamos clasificar en 5 partes, que iremos viendo de manera ordenada de acuerdo
    a su importancia.
    1. Desplazamiento de la serpiente.
    2. Consumir a la presa.
    3. Crecimiento de la serpiente.
    4. Colisión.
    5. Desparición
14. Desplazamiento de la serpiente.
    Para comenzar con la programación del desplazamiento de la serpiente, primero
    hay que tomar en cuenta que runState debe ser STATE_RUNNING.
    Vamos a volver un poco a la inicio de la función tick para inicializar
    esta variable que sería "highestIndex", que es el índice más alto, es igual
    al snake punto length menos 1.
    Ya volviendo dentro del if, vamos a hacer un recorrido desde el índice más alto
    hasta el más bajo.
    La asignación sq la tomamos de snake con el índice idx.
    Y aquí necesitamos otro if, de manera que si el índice es 0, el movimiento
    sea de acuerdo a la dirección actual asignada en el state, esto es porque
    al índice 0 corresponde a la cabeza de la serpiente.
    Pero si no lo es, el cuadro actual o sq o square, copiará el valor de su
    elemento anterior en el arreglo, de esta manera se irán recorriendo los cuadros
    uno detrás de otro a como fue creciendo la cola de la serpiente.
    Esto se podría decir que ya está listo para poder controlar la serpiente, aunque
    esta todavía no haría nada más que moverse.
    Vamos a probarlo para que veas cómo nos está quedando hasta ahora.
15. Consumir a la presa
    - Para consumir la presa, hay dos actores, primero es la cabeza de la serpiente,
    y la otra es la presa misma.
    - Vamos de vuelta a la sección de variables locales de la función "tick",
    aquí voy a declarar la variable head, lo que único que va a hacer es referenciar
    al primer elemento de la serpiente o como se llama la variable, "snake".
    - Ahora voy a introducir el concepto de puntaje, aunque no vamos a sumar puntaje,
    es la bandera que vamos a guardar cuando la cabeza haya llegado a la posición
    donde se encuentra la presa.
    - A esta variable la voy a llamar "didScore", que en español significa "puntuó",
    su valor va a ser igual a un booleano de si "x" de la cabeza es exactamente
    igual al x de la presa, pero que al mismo tiempo, "y" de la cabeza sea igual
    a "y" de la presa. Si estas dos condiciones se cumplen, se puede considerar
    que se puntuó, y la variable "didScore" contendrá el valor "true", de lo contrario
    "false".
    - Ya que sabemo si puntuó, tenemos que irnos más abajo, casi donde están las últimas
    llamadas a las funciones dentro de la función "tick", para agregar un if,
    donde se hará la reasignación de la posición en "x" y "y" de la presa, o sea
    donde aparecerá una presa nueva. Dentro de esta condición, vamos a asignar a la
    variable "prey" del "state", dos valores aleatorios de acuerdo al tamaño máximo
    del espacio de juego o BOARD_WIDTH y BOARD_HEIGHT.
    - Bien, con esto ya es suficiente para poder "puntuar", o dicho de otro modo
    poder atrapar a las presas y reaparecer otra... vamos a ver cómo va actualizando
    en el navegador. Si te fijas, ya puedes mover la serpiente, comerte a la presa,
    la presa reaparece, pero no crece la serpiente, que es lo que nos está faltando.
16. Crecimiento de la serpiente.
    - En esta etapa entran en juego la variable "growing" del state, y la constante
    "GROW_SCALE".
    - Aunque antes de que nada, vamos a necesitar obtener la posición de la cola,
    pero no podemos tomar la referencia, sino que tomaremos los valores. En cuanto
    a la variable, hay que definir una objeto vacío en la sección de variables locales
    de la función "tick", la vamos a llamar "tail". Y más adelante, pero antes del "didScore",
    y utilizando la función "assign" de la "clase" "Object", se van a copiar todas
    las propiedaes del último elemento la serpiente, dentro de nuestra
    variable "tail".
    - Ahora, vamos a volver a la condición que creamos con el "didScore", esto es
    porque la puntuada va determinar cuando la serpiente debe crecer, así que,
    antes de la asignación de la presa, vamos a sumarle al "growing" del state,
    la constante GROW_SCALE. Esta es la primera parte del crecimiento de serpiente.
    - Ahora, para que el crecimiento de serpiente sea efectivo, nada más necesitamos
    abrir otra condición, después de la que vimos anteriormente, esta se va a ejecutar
    siempre que growing sea mayor a 0. Dentro vamos a poner un push, donde se estará
    insertando la variable que creamos, que es "tail", y le vamos restar uno a la
    variable growing.
    - Ya con eso nuestro juego es jugable, ya podemos volver a probarlo.
    - Aunque de cualquier manera, como puedes ver, cuando consumimos, crece,
      y sigue creciendo, pero no podemos colisionar ni con la orilla ni
      consigo misma, para eso es el siguiente punto.
17. Colisión
    - Ya teníamos algunos minutos sin crear una nueva función, pero ya vamos a eso.
      Vamos crear la función, ques se llame "detectCollision". Esta, lo que va hacer
      a groso modo, es detectar cuando la cabeza de serpiente colisiona en las orillas
      del espacio del juego, o cuando esta colisiona con cualquiera de sus partes.
    - Lo primero y más sencillo que vamos a validar, es cuando colisiona con las orillas,
      primero voy a referenciar la cabeza, que es el primer elemento de snake.
    - Luego ya la validación, donde la posición de "x" es menor a 0 o mayor a BOARD_WIDTH,
      o bien, que la de "y" sea menor a 0 o mayor a BOARD_HEIGHT. Si esto se cumple,
      simplemente devolveremos un true, el cual va determinar que si hubo colisión
      con las orillas.
    - La siguiente parte, que es un poco más complejo, solo necesitamos recorrer
      cada uno de los cuadros de la serpiente, lo cual vamos a hacer con un for.
    - Ya dentro del for, referencío el cuadro actual de la serpiente, dentro
      de la variable "sq" de "square" o cuadro, luego, abro un if y simplemente
      tengo que validar que la posición de la cabeza y la posición de cualquier
      otra parte del cuerpo, estén en la misma posición, esto estaría determinando
      que están colisionados, y devolvería el booleano "true".
    - Si el recorrido por los cuadros de la serpiente, no encontraron una colisión,
      simplemente lo que se haría, es devolver un booleano "false".
    - Ya tenemos nuestra función de colisión lista, aunque, todavía nos hace falta
      llamarla en el lugar adecuado, dentro de la función "tick".
    - Justo antes de la condición de "didScore", vamos a abrir otro if,
      donde vamos a llamar a la función "detectCollision", y si la colisión es detectada,
      vamos a cambiar el "runState", para que ahora tenga el valor STATE_LOSING, y también
      nos debemos asegurar que ya no siga creciendo, poniendo growing a 0.
18. Desparición
    - Para este último paso solo hace falta
    un poco de código, si recuerdas la condición donde validamo el runState,
    a esta le vamos a adicionar un else if,
    para cuando el runState sea STATE_LOSING.
    - Ya dentro del if, lo primero por hacer
    es cambiar el intervalo de actualización
    a 10 milisegundos.
    - Después vamos a crear otra condición
    que se ejecutará, cuando exista por lo
    menos un cuadro de la serpiente, y lo que se hará dentro, es simplemente un eliminado de la cabeza de la serpiente utilizando la función splice, donde el
    primer parámetro es el índice, y el segundo la cantidad de cuadros que se borrarán.
    - La segunda condición, se dará cuando la serpiente ya no tenga ningún cuadro, esto significa que ya se deberá comenzar otra
    partida, así que para eso vamos a cambiar nuevamente el runState a STATE_RUNNING
    y como la serpiente ya está vacía, le vamos a agregar una posición aleatoria nueva, y reposicionamos la presa también.


    


