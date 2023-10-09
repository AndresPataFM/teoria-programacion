# Tipos de Datos

En javascript a grandes razgos hay 2 tipos de datos, los primitivos y los no primitivos. Para poder identificar el tipo de dato tenemos una herramienta en javascript que es el "typeof" (es un operador unario). Se utiliza de la siguiente manera:

```js
typeof dato-a-analizar

// si desean verlo en consola:
console.log(typeof dato-a-analizar)

```
## Indice:

* Datos primitivos
    * number
    * bigint
    * string
    * boolean
    * undefined
    * null (object)
    * symbol
* Datos no primitivos
    * function
    * object

## Datos primitivos

### number

A diferencia de otros lenguajes de programación, javascript unifica la mayoría de los tipos conocidos de números en un tipo general. Este es simplemente un número sin ningún otro cambio.

```js
let variable = 4
```
Sin embargo, en programación distinguimos los números en distintos tipos ya que los tratamos de maneras distintas (esto se debe a como son procesados y que tan preciso puede ser uno con ellos, más que nada esta limitado según cuanta memoria tiene dedicada la variable que maneja el numero, cosa que no podemos controlar en Javascript). Aca podemos distinguir:

* Enteros (integer): son todos los números, positivos, negativos y 0 sin valor luego de la coma (punto en nuestro casos que suamos notación inglesa). 
```js
124
```
* Punto Flotante | Flotante | Real (floating point | float | real): son todos los números posibles, incluyendo esos que tienen valores decimales. Como se requiere más memoria para hacer calculos con ellos y suelen generar MUCHOS errores de cálculos al ser imprecisos, se suelen hacer lo que se conoce como *workarounds* (soluciones alternativas) para evitar usarlos. Especialmente cualquier cosa que trabaje con $ evita usar numeros flotantes ya que errores de 0.01 son muy graves y pueden llevar a juicios importantes. Acuerdense que en ingles se utiliza un "." para separar la parte entera de la decimal:
```js
1.2
// Ejemplo de cálculos imprecisos:
1.2-1 // = 0.19999999999999996
```
* No es un número (Not a number) | NaN : paradoxicamente, el tipo de dato que simboliza que algo no es un número es un número. Este dato aparece cuando uno intenta hacer una coherción de datos de otro tipo de datos a un número y Js no ve como convertirlo, retornando NaN. Ayuda a mantenter de manera más consistente el tipado de variables al ser un tipo de dato numético
```Js
NaN
// Hay que respetar las mayúsculas, vean sino que Js piensan que usan una variable
nan
```
* Infinito (Infinity): Se utiliza para expresar tanto el infinito como números más grandes de lo que puede procesar Js normalmente. tambien encontramos casos especiales como la división por 0 que es Infinity (si, el valor es la palabra infinito en inglés con mayúscula al inicio), además, este puede ser positivo y negativo.
```js
Infinity
+Infinity
-Infinity

Number.MAX_VALUE*2 // Infinity

// Hay que respetar las mayúsculas, vean sino que Js piensan que usan una variable
infinity
```
#### Info extra

> En Javascript los number tienen doble-precisión en un formato de 64 bits (como referencia, C suele usar 32 bits para un entero)
>> Para ello utiliza: 
>> * 1 bit para el symbolo (negativo/positivo)
>> * 11 bits para el exponente (-1022 a 1023) para números grandes (mayores a 21 dígitos)
>> * 52 bits para la *mantissa* (esto es tanto el numero actual como los valores despues de la coma, o punto ya que usamos notación inglesa)
>
> Todo esto da como resultado que el número más grande siendo preciso que puede expresar de manera segura en Js son -2^53 + 1 y (2^53) - 1 y los más grandes posibles 2^(-1074) y (2^1024) - 1. Sepan que Si bien el valor de la fracción 2^(-1074) es muy pequeño, requiere de mucha memoria para ser preciso.

### bigint

Como mencionamos, los números enteros paran de ser precisos fuera de -(2^53)+1 < X < (2^53)-1, por ejemplo:

```js
100856901*278598677 // 28098599184919976
```

lógicamente en este caso, si los últimos números son un 1 y un 7, esperaría que el último número sea 7 pero es 6, eso se debe a como maneja la memoria de los números Js. Es por eso que se crearon los big int o enteros grandes, para manejar estos casos:

```js
100856901n*278598677n
28098599184919977n
```

Ahora si podemos ver el 7 al final. lo que hizo Js es sacar la memoria que dedicaba a calcular potenciales puntos flotantes y dedicarla a manejar enteros.

Un big int se declara agregandole un n al final del número (NO PUEDE TENER DECIMALES)

```js
546n
```

#### Limitaciones: 

* No maneja fracciones asi que cuidado al dividir
* No puede interactuar con datos de tipo number en operaciones matemáticas
* Se recomienda evitar su uso con critografía por temas de seguraidad
* Se recomienda su uso solo para números fuera del rango  -(2^53)+1 < X < (2^53)-1

### string

Para representar cadenas de texto, en Javascript (y muchos otros lenguajes) se utilizan strings. Js utiliza los carácteres [UTF-16](https://www.fileformat.info/info/charset/UTF-16/list.htm) (aunque muchos se apegan a UTF-8 por mayor compatibilidad) lo que significa que se distingue entre mayúsculas ya que carácteres como `"A"` tienen un código de `feff0041` y `a` tiene un código de `feff0061`. Al ser distintos los códigos podrán ver como para Js una mayúscula es algo totalmente distinto de una minúscula.

Para declararlos uno utiliza comillas y hay 3 tipos:
* Comillas dobles (doble quote) `""`
* Comillas simples (simple quote) `''`
* Backtick ` `` `

Lo importante es que el string finaliza al momento en el que se encuentra una comilla del mismo tipo con el que se inicio. Por lo que podés formar:

```js
const string1 = "hola1"
const string2 = 'hola2'
const string3 = `hola3`
```
La ventaja de tener varios tipos de comillas es que permiten utilizar un tipo de comilla dentro de otra sin "romper" el string:

```js
const seRompe = 'can't'
```
Vean como js considera esa t como variable y no entiende proque esta ahí y abre otro string, hay 2 soluciones, escapes o utilizar otras comillas:
```js
const escape = 'can\'t'
const otras = "can't"
```
En terminos prácticos js no distingue entre las comillas, pero la tendencia contundente es utilizar comillas simples al no requerir apretar shift aunque en typescript los contribuidores recomiendan comilla doble. Ahora, las backtick tienen funcionalidad extra que son string templates, permitiendo inyectar código de js que va a ser corcido a un string (forzozamente convertido de manera automática, ver el otro md). Ejemplo:
```js
const nombre = 'Andrés'
const numeroFavorito = 4
const saludo = `Hola mi nombre es ${nombre} y mi número favorito es ${numeroFavorito} que multiplicado por 5 es ${numeroFavorito*5}`
console.log(saludo)
// Hola mi nombre es Andrés y mi número favorito es 4 que multiplicado por 5 es 20
```
Vean como TODO es un string y los numeros no se consideran tipos de datos distintos.

#### Info extra

> En Javascript los strings son secuencias de unidades de código de [UTF-16](https://www.fileformat.info/info/charset/UTF-16/list.htm), lo que resulta en 65536 carácteres posibles (aun más con pares).
> 
> Cada secuencia tiene un límite nativo de 2^53 - 1 carácteres (el límite de enteros precisos) peropara ahorrar memoria la mayoría e los dispositivos implementan un máximo de que permite almacenarlo con 32 bits:
> * V8: 2^29 - 24
> * Firefox: 2^28 - 1
> * Safari: 2^31 - 1
>
> Son *inmutables*, se puede cambiar la referencia del valor a un string distinto pero este no se modifica
