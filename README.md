# SR_restaurantes

Hoy en día, existe una gran variedad de restaurantes y muchas veces, ya sea por innovar o descubrir nuevos restaurantes en tu cuidad, no sabemos a qué restaurante podríamos ir. Por este motivo, a continuación, se ha construido un sistema de recomendación de restaurantes con la ayuda de todos los clientes que asisten.
Este sistema de recomendación consiste en recomendar restaurantes basándose en las valoraciones de las personas que suele opinar lo mismo que tú y en el tipo de comida que más te gusta, usando la información sobre lo que habitualmente sueles comer.
El sistema va a tener como entrada cuatro parámetros:

1.  Un dataframe con las valoraciones de cada cliente sobre unas características de los restaurantes a los que ha ido. Estas características van a ser:

    1. Comida: Nota del 1-10.
    2. Servicio: Nota del 1-10.
    3. Local: Nota del 1-10.
    4. Precio: Tiene las categorías “Alto”, “Bajo” y “Medio”.
       Además, vendrá la información de qué tipo de comida ha consumido el cliente.
       Vamos a distinguir los siguientes tipos de comida:

    - Tapas españolas
    - Americana
    - Italiana
    - China
    - Mexicana

      De esta manera obtenemos un dataframe donde las variables/columnas van a ser el nombre del restaurante, las características anteriores, la localización del restaurante y el tipo de comida que ha consumido el cliente, y los casos/filas van a ser el nombre de cada uno de los clientes.

2.  El nombre de la persona a la que le vamos a recomendar los restaurantes.
3.  Un dataframe donde contenga la información de los restaurantes a los que ha ido la persona a recomendar, en el último año, junto con el tipo de comida que ha consumido.
4.  La provincia de la persona a la que le vamos a recomendar los restaurantes.

El sistema va a comenzar filtrando los restaurantes de la provincia de la persona a recomendar. Una vez realizado, creará un nuevo dataframe donde obtengamos las valoraciones genéricas de cada cliente sobre cada restaurante. Así pues, ahora las variables/columnas van a ser los restaurantes y los casos/filas seguirán siendo el nombre de cada cliente.

Para obtener esta valoración genérica del restaurante, se realizará lo siguiente: Se toma la media de las valoraciones de comida, servicio y local y en función de dicha media:

- Si la media es >=7:
  - Si el precio toma el valor “Alto”, entonces reducimos la media una unidad.
  - Si el precio toma el valor “Medio”, entonces la media no se modifica.
  - Si el precio toma el valor “Bajo”, entonces tomamos el mínimo entre 10 y media+1.
- Si la media es <7:
  - Si el precio toma el valor “Alto”, entonces tomamos el máximo entre 0 y la media-1.
  - Si el precio toma el valor “Medio”, entonces la media no se modifica.
  - Si el precio toma el valor “Bajo”, entonces aumentamos la media una unidad.

Una vez obtenido el dataframe anterior, vemos cuáles son los restaurantes a recomendar y para cada uno de ellos, vamos a obtener una medida de cuánto podría gustarle ese restaurante, es decir, vamos a predecir cuánto le gustaría el restaurante a la persona a recomendar si hubiese ido. Esta calificación va a venir dada por la suma de dos marcadores:

    1. Suma ponderada de las valoraciones de los clientes que han ido a ese restaurante.
    2. Un marcador que me diga cuánto le gusta a la persona a recomendar el tipo de comida que tiene ese restaurante.

Para el primer marcador, calcularemos la correlación de Pearson entre los clientes y ponderamos cada cliente mediante esta medida. De esta forma, las personas que opinen similares a la persona a recomendar, tendrán más peso en la suma ponderada de las valoraciones. Sin embargo, las personas que opinen muy diferente van a tener una correlación negativa, por lo que disminuirá la suma ponderada. Para que no ocurra esto, vamos a pasar la correlación a un número entre 0 y 1 y así, estas personas afectarán poco en la suma ponderada de las valoraciones, pero no la disminuirá.

Para el segundo marcador, calculamos la frecuencia relativa de cada tipo de comida que ha consumido la persona a recomendar en el último año. Nos quedamos con la frecuencia relativa del tipo de comida que presenta el restaurante.

Para saber de qué tipo de comida es el restaurante, observamos el tipo de comida que consumieron los clientes cuando fueron.

Como resultado final del sistema de recomendación, vamos a obtener una lista de los 10 restaurantes mejor puntuado a los que no ha ido la persona a recomendar, ordenada en función de la calificación calculada anteriormente.

_Observaciones_:

- En todo momento vamos a suponer que:
  - Cada restaurante sólo tiene un tipo de comida.
  - Si hay una cadena de restaurantes, al valorarlo un cliente, lo hace para toda la cadena en sí.
- Como la base de datos ha sido creada por mí, no hay un número considerable de personas. Pero si lo hubiese, lo ideal sería realizar un kNN o un análisis cluster y centrarnos en las personas dentro del grupo donde está la persona a recomendar.
