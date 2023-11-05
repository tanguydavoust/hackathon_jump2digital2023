# Proyecto de Prueba de Data Science - Hackathon JUMP2DIGITAL 2023

## 1. Introducción:

Este proyecto es mi prueba tecnica para poder participar al Hackathon del JUMP2DIGITAL 2023 que tendrá lugar el 17 de Noviembre de 2023 en Barcelona. Este proyecto se centra en el análisis de dos conjuntos de datos: "Alquiler Mensual en Barcelona" y "Exposición a Ruidos en Barcelona". Me interesa saber que impacto puede tener el ruido en un barrio sobre el precio del alquiler. Aún, en este proyecto, el objetivo principal será la preparación de los datos para un futuro uso en proyectos de Data Science.

### a. Conjunto : Alquiler mensual medio (€/mes) y por superficie (€/m2) de la ciudad de Barcelona en 2017

Podemos encontrar este dataset aquí : (https://opendata-ajuntament.barcelona.cat/data/es/dataset/est-mercat-immobiliari-lloguer-mitja-mensual/resource/0a71a12d-55fa-4a76-b816-4ee55f84d327)

Este dataset contiene el precio medio por cada trimestre de 2017 de cada Barrio de la cuidad de Barcelona. Especificado en :
- Alquiler mensual medio (€/mes)
- Alquiler por superficie (€/m2)

### b. Conjunto : Población expuesta a los niveles de ruido del Mapa Estratégico de Ruido de la ciudad de Barcelona en 2017

Podemos encontrar este dataset aquí (https://opendata-ajuntament.barcelona.cat/data/es/dataset/poblacio-exposada-mapa-estrategic-soroll/resource/3846500e-72aa-4780-967f-f09aa184eaba)

Este dataset contiene el nivel de ruido por Barrio según fuente de ruido y diferente momento del día :

#### Fuentes de ruido exigidas por normativa:

Tráfico viario (TRANSIT) // Grandes infraestructuras viarias (GI_TR) // Ejes ferroviarios y tranvía (FFCC) // Industria (INDUST)

#### También se han estudiado de manera detallada otros tipos de emisores y receptores de ruido no exigibles por normativa:

Ocio y aglomeración de personas (OCI) // Calles peatonales (VIANANTS) // Parques (PARCS) // Patios interiores (PATIS)

#### Momento del día y media ponderada

El período día (D) es de 7.00 a 21.00 horas

La tarde (E), de 21.00 a 23.00 horas

La noche (N), de 23.00 a 7.00 horas.

De estos tres períodos se obtienen los índices acústicos D, E y N. Además, hay un cuarto índice acústico, 
El ponderado día-tarde-noche (DEN), que corresponde a una media ponderada de los índices Ld, Le y Ln, penalizando los períodos tarde y noche.

## 2. Depuración de datos:

En esta sección, detallaremos las técnicas de preprocesamiento de datos aplicadas a los conjuntos de datos "Alquiler Mensual en Barcelona" y "Exposición a Ruidos en Barcelona".

### a. Merge : Unión de los conjuntos de data:

Uno de los pasos clave en el preprocesamiento es la unión (merge) de estos conjuntos de datos para crear un conjunto de datos unificado que permita un análisis más completo. Para combinar la información de los precios de alquiler con la exposición a ruidos en Barcelona, hemos realizado un merge utilizando las columnas : "Codi_Districte", "Nom_Districte",	"Codi_Barri", "Nom_Barri" como referencia. El tipo de merge utilizado fue un "outer merge", lo que significa que conservamos todas las filas de ambos conjuntos de datos.

### b. Eliminación de valores faltantes:

Durante la revisión de los conjuntos de datos, se observó la presencia de valores faltantes únicamente en la columna "Preu" (precio del alquiler). Se optó por eliminar las filas que contenían valores faltantes en lugar de imputar los valores ausentes. La razón detrás de esta elección fue que la imputación podría haber introducido sesgos en nuestros datos. Además, dado que la columna "Preu" es de gran interés en nuestro análisis, mantener su integridad era fundamental.

Es relevante destacar que los valores nulos en la columna "Preu" representaban solamente un 6,5% de las observaciones, lo que respalda nuestra decisión de eliminar las filas con valores faltantes en esta columna.

### c. Comprobación de la no presencia de valores duplicados:

La existencia de valores duplicados podría distorsionar nuestros resultados y conclusiones. Por lo tanto, se realizó una verificación exhaustiva del conjuntos de datos para garantizar la integridad de los datos. Hemos podido observar que no había valores duplicados.

### d. Transformar las variables en numericas:

La transformación de variables categóricas en numéricas es un paso importante en el preprocesamiento de datos, especialmente cuando se trabaja con modelos de aprendizaje automático que requieren que todas las variables sean numéricas.

Por lo tanto, hemos primero observado las columnas categoricas de nuestro conjunto de datos. Hemos podido observar que teníamos varias columnas categoricas: Nom_Districte, Nom_Barri, Concepte, Rang_Soroll, Valor, Lloguer_mitja

A continuación nuestra metodología para tratar estas columnas :

- "Nom_Districte" y "Nom_Barri" tienen variables numericas equivalentes en el dataset : "Codi_Districte" y "Codi_Barri":
=> Las hemos eliminado

- "Valor" eran valores numericos pero tratados como categoricos por la culpa del "%" a final de cada observación.
=> Hemos eliminado el "%" y convertido cada observación en valor numerica.

- "Rang_soroll" y "Lloguer_mitja" tuvimos que transformarlas en valores numericas. Se aplicó la función pd.get_dummies() para llevar a cabo esta transformación.

### e. Estandarización de variables:

La estandarización implica reescalar las variables de manera que tengan una media de cero y una desviación estándar de uno. Este proceso es útil cuando se trabajan con modelos o algoritmos que son sensibles a la escala de las variables. Por lo cual hemos usado el metodo de StandarScaler para llevar esto a cabo :

### f. Baja Varianza:

La baja varianza en una variable implica que la mayoría de los valores en esa variable son iguales o muy similares. Estas variables con baja varianza pueden tener un impacto limitado en nuestros análisis y modelos, ya que no aportan mucha información discriminativa. Por lo tanto, es común eliminar estas variables o transformarlas de manera adecuada.

En nuestros conjuntos de datos, se aplicaron la siguiente estrategía:

- Identificación de Variables de Baja Varianza
- Eliminar las variables descubiertas => "Any":

### g. Analysis de covarianza:

- El análisis de covarianza se utilizó para investigar si existen variables muy corelacionadas entre ellas. Esto nos permitió poder eliminar variables con comportamiento identico que no iban a darnos nuevas información sobre el comportamiento de la variable de referencia "Preu".

Esto nos permitió reducir las dimensiones del conjunto de datos con la supresión de las columnas "Codi_Districte" y "Lloguer_mitja_Lloguer mitjà mensual (Euros/mes)".

## 4. Conclusiones:

Agradezco sinceramente por tomar el tiempo de revisar mi prueba técnica. En este notebook, hemos trabajado con dos conjuntos de datos proporcionados por OpenData Barcelona: uno que contiene información sobre los precios de alquiler por barrio y otro que detalla los niveles de exposición al sonido en la ciudad. Hemos unido estos conjuntos de datos para realizar un análisis cruzado con el objetivo de profundizar en la relación entre los precios de alquiler y la exposición al sonido en Barcelona.

Durante el desarrollo de este proyecto, llevamos a cabo un extenso preprocesamiento de las observaciones, incluyendo la eliminación de valores faltantes y la estandarización de las variables, para garantizar la calidad y la integridad de nuestros datos. El conjunto de datos resultante puede servir como una sólida base para futuros trabajos en el campo de la Ciencia de Datos.

Este proyecto no solo proporciona una comprensión más profunda de los factores que afectan los precios de alquiler en Barcelona, sino que también representa una valiosa oportunidad de prueba técnica en el contexto del Hackathon de Data Science de Jump2Digital. Espero que los hallazgos y las técnicas empleadas aquí sean de utilidad y puedan contribuir a futuras investigaciones y desarrollos en este emocionante campo.
