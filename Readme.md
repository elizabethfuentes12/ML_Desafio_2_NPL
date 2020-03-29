# Desafío 2: Procesamiento de Lenguaje Natural

## __Alumnos__
* __Enrique Rodriguez__
* __Elizabeth Fuentes__  

## Enunciado

Procesamiento de Lenguaje Natural

El dataset “logs_textos_usuario.csv” corresponde a 4 meses de interacciones con un asistente digital (para más información https://aura.telefonica.com/).

* Columnas:

    * conversationId : id de la conversación. Único para cada sesión de usuario createAt : fecha y hora en que se generó la interacción.
    * text : texto escrito por el usuario.
    * intent : intent identificado por el motor cognitivo.
    * intentScore : confianza asociada al intent identificado.
    * resolved : usuario selecciono “SI” o “No” a “¿Fue útil esta respuesta?” tras ver una respuesta al texto ingresado.
    * selectedOption : En caso de seleccionar “No” en la opción anterior, se muestran 5 sugerencias. Numero de la opción de sugerencia seleccionada
    * topIntent1 : intent asociado a la primera opción topIntent2 : intent asociado a la segunda opción topIntent3 : intent asociado a la tercera opción topIntent4 : intent asociado a la cuarta opción topIntent5 : intent asociado a la quinta opción

El objetivo general de la actividad es evaluar la manipulación y presentación de resultados de análisis de NLP. Se espera principalmente que los participantes logren “identificar los tópicos más relevantes hablados por los usuarios”.

* Los objetivos específicos de la actividad son los siguientes:

    * Manipulación de librerías y herramientas para el procesamiento de lenguaje natural. Será un plus demostrar habilidades en herramientas para NLP en Python y/o R.
    * Visualización de los resultados. 
    * Presentación y storytelling de los participantes.
    

## DESARROLLO

## 1. Limpieza de los datos
* [Desafio_limpieza.ipynb](Desafio_limpieza.ipynb)
* La limpieza la realizamos de la siguiente forma:

    * Una vez identificado que tenemos tres tipos de informacion, las que se solucionan a la primera,las que no y las que no tienen solucion. 
    * Eliminamos de nuestro dataset aquellas que no tienen solucion, todas aquellas con el selectedOption = 5 ya que no nos generan ningun valor.
    * Las que no tienen solucion a la primera tienen 5 opciones de la cual se selecciona una indicada por el selectedOption.
    * Separamos nuestros dataset entre los dos tipos de soluciones en dos DataFrames resolved1 y not_resolved:
        * resolved1: los que se soluciona a la primera solo limpiamos las columnas dejando unicamente a text e intent.
        * not_resolved: los que no se soluciona a la primera macheamos el selectedOption con el intentent correspondiente con la funcion:
            def get_final_option(row):
                select=not_resolved.columns[row.selectedOption+4]
                return row[select]
                
    * Creamos un nuevo dataset juntando los DataFrame anteriores, el cual usaremos para nuestro modelo. 
           
 
 ## 2. Modelo
 * [Desafio_Modelo.ipynb](Desafio_Modelo.ipynb)
 * Observaciones:
     * Antes de aplicar cualquier modelo limpiamos y vectoriamos las oraciones que conforman el texto a analizar. 
     * Empleamos el modelo clasificador multinomial Naive Bayes, el cual nos dio el siguiente resultado:

     
                        precision    recall  f1-score   support

                   0       0.00      0.00      0.00         0
                   1       0.00      0.00      0.00         0
                   2       0.00      0.00      0.00         0
                   3       0.22      1.00      0.36         2
                   4       0.00      0.00      0.00         0
                   5       0.11      0.17      0.14        23
                   6       0.00      0.00      0.00         3
                   7       0.74      0.27      0.40       195
                   8       0.19      0.44      0.27         9
                   9       0.14      0.26      0.18        19
                  10       0.53      0.24      0.33       107
                  11       0.33      0.45      0.38        11
                  12       0.33      0.25      0.29         4
                  13       0.83      0.67      0.74        30
                  14       0.00      0.00      0.00         0
                  15       0.00      0.00      0.00         0
                  16       0.29      0.50      0.36         4
                  17       0.25      1.00      0.40         1
                  18       0.13      1.00      0.24         2
                  19       0.47      0.67      0.55        12
                  20       0.00      0.00      0.00         2
                  21       0.00      0.00      0.00         0
                  22       0.05      1.00      0.10         1
                  23       0.21      0.50      0.29        10
                  24       0.62      0.50      0.55        36
                  25       0.13      0.20      0.16        10
                  26       0.50      0.31      0.38        29
                  27       0.44      1.00      0.62         4
                  28       0.33      0.83      0.48         6
                  29       0.44      0.57      0.50         7
                  30       0.00      0.00      0.00         1
                  31       0.05      0.25      0.09         4
                  32       0.00      0.00      0.00         0
                  33       0.50      0.62      0.56         8
                  34       0.55      0.75      0.63        16
                  35       0.11      0.30      0.16        10
                  36       0.09      1.00      0.17         1
                  37       0.73      0.61      0.67        18
                  38       0.00      0.00      0.00         0
                  39       0.66      0.50      0.57        50
                  40       0.14      0.50      0.22         2
                  41       0.00      0.00      0.00         0
                  42       0.57      0.30      0.39        88
                  43       0.58      0.40      0.47        70

           micro avg       0.37      0.37      0.37       795
           macro avg       0.26      0.39      0.26       795
        weighted avg       0.56      0.37      0.41       795

     
### El modelo tiene una presicion del 56%, hay intent que tiene una precision y recall, lamentablemente la cantidad de datos no aportan para hacer un modelo mas acertado.
