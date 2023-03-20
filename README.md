# Instrucciones de ejecución

cd CMF-API
npm install
npm run format
npm run dev


# Evaluación Técnica


La Comisión Para el Mercado Financiero chileno tiene APIs públicas que permiten acceder a
reportes bancarios e indicadores financieros. Para resolver los requerimientos listados a
continuación, es necesario utilizar la API que entrega los valores de la Unidad de Fomento (UF).

1. A través de esta API se debe obtener el valor de la UF a partir de un año ingresado a
través de alguna interfaz.

2. Con la información anterior se deberá determinar cuáles fueron los meses en que la UF
tuvo una variación al alza, en cuales la variación fue a la baja y, finalmente, en los que no
tuvo variación.

3. Listar los meses en que subió la UF. Estos meses deben ser ordenados de menor a mayor
considerando el valor de aumento, es decir, del mes que menos subió al que más subió.

4. Listar los meses en que bajó la UF. Estos meses deben ser ordenados de mayor a menor
considerando el valor de aumento, es decir, del mes que más bajó al que menos bajó.

5. Listar los meses en que no hubo variación. Estos meses deben ser ordenados
alfabéticamente a partir del nombre del mes.

6. Identificar el día con el mayor valor y menor valor de UF de todo el año.

7. Construir una interfaz web idónea, donde se visualice toda la información pedida en los
puntos anteriores.


El desarrollo de estos puntos debe ser entregado en un plazo máximo de 24 horas desde que se
recibió el correo. La entrega debe hacerse a través de un archivo comprimido al mismo correo
que envío la prueba
