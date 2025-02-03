
# Base-de-datos
Son utilizadas como solución, o como soporte a la solución, de la
mayor parte de los problemas que se presentan en el mundo real
## tema 1
Definicion de ficheros: es una coleccion de informacion relacionada entre si y que es almazenada

1. Caracteristicas de los ficheros
 + la informacino debe se independiente
 + la informacion almacenada es permanente
 + gran capacidad de almacenamineto
2. Ficheros segun su uso
 + **Permanente o maestro**: no sufre modificaciones grandes y se actualeza constantemente
 + **De movimiento**: esta para actualizar el fichero maestro.
 + **De maniobra o Trabajo**: Se utiliza para realizar la actualizacion de el fichero de movimineto al maestro y cuando a terminado de pasar la informacion actualizada del ficher de movimiento al de trabajo, el de trabajo se consvierte en el _Maestro_
3. Fichero segun el tipo de acceso  
 + **secuenciales**: estan en desuso. ``Los registros estarán desordenados. Los registros no tienen una longitud fija.
Este tipo de acceso también se puede utilizar en el resto de ficheros.``
 + **Directos**: para acceder a la informacion se accede con *accesos directos*. Para buscar la informacion estas el *campo clave* que con una operaciones aridmeticas se puede encontrar la informacion en la posicion que se encuentra en el disco. Ej:En un coleguio, el campo clave es el dni porque todos los alumnos tiene el mismo numero de bites y todos el mundo se save su dni.
     - Consultas ¿como se realizan? Con dos tipos de calcilos uno mas practico que otro.
       * Caso 1 Codigo : campo clave, será un número del 1 al 99: 2 dígitos
Denominación: denominación del artículo. 18 caracteres.  
Podemos observar que la longitud_registro es 2+18 = 20.  
Podemos observar que el primer artículo comenzará en la dirección física 1 y
terminará en la 20,
el segundo artículo comenzará en la dirección física 21 y terminará en la 40…  
En este caso la fórmula sería la siguiente:  
Dirección física de comienzo del registro = ((Codigo – 1)*longitud_registro)+1  
Comprobamos:  
Dirección física de comienzo de registro 1 = ((1-1)*20)+1 = 1  
Dirección física de comienzo de registro 2 = ((2-1)*20)+1 = 21  
…  
Dirección física de comienzo de registro 99 = ((99-1)*20)+1 = 1961  
**Observa**: el fichero ocupará, como mucho, 1980 (el registro 99 irá de 1961 a 1980)  
**Ventajas**: no habrá huecos, salvo que se borre algún artículo o no se
introduzcan todos. No habrá sinónimos: dos códigos distintos no pueden dar la
misma dirección física de comienzo. La fórmula es sencilla de obtener.  
**Inconveniente**: El código nada tiene que ver con el artículo, habrá que
aprenderlos de memoria o tener un listado para consultarlo.
       * Caso 2: el campo clave es un dato del registro. Ejemplo de un fichero
ALUMNOS:
DNI : campo clave, será un número del 1 al 99999999: 8 dígitos  
Nombre: nombre y apellidos del alumno. 32 caracteres.  
Podemos observar que la longitud_registro es 8+32 = 40.  
Podemos observar que la fórmula del caso anterior es una opción inviable: si
prevemos tener 1000 alumnos (numero_maximo_de_registros = 1000) el
fichero podría ocupar un tamaño máximo de 1000*40=40.000, sin embargo con
la fórmula anterior el alumno con DNI 52.000.00 empezaría a grabarse en la
posición ((52000000-1)*40)+1 = 2.079.999.961. El Sistema Operativo tendría
que reservar sobre 2 GB para almacenar este único registro, ya que debe
reservar sitio para todos los anteriores si quiere garantizar el acceso directo.  
Solución: necesitamos una fórmula que convierta cualquier DNI en un número
del 1 al 1000 (siguiendo la suposición de que prevemos que no vamos a
superar ese número de alumnos). Esta solución la podemos conseguir si al
resto de dividir DNI entre 1000 y le sumamos uno. Nota: el símbolo que se
utiliza para indicar que queremos coger el resto de una división es el %
(DNI % 1000) + 1  
Ejemplo: (52.000.000 % 1000) + 1 = 1  
Observa: al dividir un número entre n el resto va de 0 a n-1  
Ejemplo: al dividir un número entre 1000 el resto va de 0 a 999  
El alumno cuyo (DNI % 1000) sea 0 debe comenzar en la dirección física 1  
El alumno cuyo (DNI % 1000) sea 1 debe comenzar en la dirección física 41  
…  
En este caso la fórmula sería la siguiente:  
Dirección física de comienzo del registro =
((DNI % numero_maximo_de_registros)*longitud_registro)+1  
Comprobamos:  
Dirección física de comienzo de registro del alumno con DNI 52.000.000  
((52000000 % 1000)*40)+1 = 1  
Dirección física de comienzo de registro del alumno con DNI 52.000.001  
((52000001 % 1000)*40)+1 = 41…  
Dirección física de comienzo de registro del alumno con DNI 52.000.999  
((52000999 % 1000)*40)+1 = 39961 y llegará hasta la posición 40.000  
**Ventaja**:  
o No hay que saberse el código de los alumnos, cada alumno se sabe
su dni y cuando nos lo diga podremos acceder directamente a su
información.  
**Inconvenientes**:  
o Genera Huecos: puede que de los 1000 alumnos ninguno tenga un
DNI terminado en 123 y por tanto ese hueco ocupa espacio en disco
pero nunca se utiliza.  
o Genera Sinónimos: varios DNI devuelven el mismo resto al dividir
entre 1000 y por tanto darán la misma dirección física de comienzo.
Ejemplo: 52000789, 53000789, 54000789 darán de resto 789. Este
problema se resuelve con lo que se denomina ZONA DE
EXCEDENTES: es una zona donde se guardan los registros que ya
tienen ocupada su dirección física de comienzo. Se guardan en
orden secuencial. Si viene el alumno 54000789 se aplicará la fórmula
y se irá directamente a buscar sus datos, pero encontrará a
52000789, como no coincide se irá a la zona de excedentes y
buscará secuencialmente hasta que lo encuentre.

  + **Por Indice**: Se ordena por el campo o los campos que queremos indexar, Busqueda dipotomica funciona de la siguiente manera alfabeticamente empieza a buscaar a partir de la mitad de la tabla y se hay si alfabeticamente esta por encima o por devajo de la informacion realiza otra lectura y se encamina a la direccion que es, si en esa lectura tampoco lo encuentra buelve a realizar alfabeticamente si esta por encima o por devajo del nombre que busca, hasi asta que encuentra el nombre y muestra la informacion.    
      - Ventajas:  
o Se puede acceder por campos que no son la clave y obtener los
registros en cualquier orden.  
      - Inconvenientes:  
o Las altas y bajas requieren una reorganización del fichero índice.  


4. Inconvenientes de los sistemas de ficheros
Un programa para efectuar cargos o abonos en una cuenta.
+ Un programa para crear una cuenta nueva.
+ Un programa para calcular el saldo de una cuenta.
+ Un programa para efectuar las retenciones fiscales.
+ Un programa para generar un listado de las operaciones mensuales
    

   
