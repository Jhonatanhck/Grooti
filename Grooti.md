# Writeup - Máquina Grooti

## Despliegue y Reconocimiento

Primero tenemos que desplegar la máquina para empezar a operar.

![Despliegue de la máquina](images/Pasted%20image%2020260206153124.png)

Comenzamos con el escaneo de puertos. Obtenemos esto del escaneo de Nmap:

![Escaneo de Nmap](images/Pasted%20image%2020260206153248.png)

Pasaré a enumerar la página web. Tenemos esto en el index:

![Página web principal](images/Pasted%20image%2020260206153351.png)

En el directorio de imágenes encontramos esta contraseña, y nos dice que encontremos dónde ponerla: `password1`.

![Contraseña encontrada](images/Pasted%20image%2020260206153442.png)

Revisando el código fuente, tenemos esta nota que parece ser el nombre de algún usuario a nivel de sistema (`rocket`).

![Código fuente nota](images/Pasted%20image%2020260206153623.png)

## Acceso Inicial

Probaré el usuario `rocket` y la contraseña que encontramos para conectarme a la base de datos (MongoDB/MySQL, etc).

![Conexión exitosa a DB](images/Pasted%20image%2020260206153821.png)

Pudimos entrar. Vamos a ver qué podemos enumerar aquí dentro.

![Enumeración de bases de datos](images/Pasted%20image%2020260206153853.png)

Tenemos esta base de datos que parece ser interesante. Al explorarla, encontramos rutas. La última es la que no teníamos antes, así que la probaré en el navegador.

![Rutas encontradas en DB](images/Pasted%20image%2020260206154144.png)

Tenemos esto al acceder a la ruta que descubrimos:

![Acceso a ruta oculta](images/Pasted%20image%2020260206154749.png)

## Explotación

Al poner `whoami` y un número random del 1-100 se me descarga un archivo llamado `password100.txt`.

![Descarga de archivo txt](images/Pasted%20image%2020260206155743.png)

El contenido nos devuelve el mismo output:

![Contenido del txt](images/Pasted%20image%2020260206155831.png)

Lo que se me ocurrió para ir más rápido, en lugar de ir 1 por 1, fue abrir el **Burp Suite** e iterar.

![Intruder en Burp Suite](images/Pasted%20image%2020260206155904.png)

El número **16** me dio una respuesta diferente a las demás (diferente longitud).

![Respuesta diferente payload 16](images/Pasted%20image%2020260206155940.png)

Cuando veo la respuesta parece ser un archivo ZIP.

![Cabecera archivo ZIP](images/Pasted%20image%2020260206160005.png)

Así que ahora poniendo el 16 se me descarga un archivo ZIP.

![Descarga del ZIP](images/Pasted%20image%2020260206160033.png)

Aquí tenemos el archivo descargado:

![Archivo zip en local](images/Pasted%20image%2020260206160125.png)

Parece que necesita una contraseña. Probaré la contraseña que descubrimos al inicio (`password1`).

![Descompresión con contraseña](images/Pasted%20image%2020260206160159.png)

Funcionó. Veré qué hay dentro del `.txt`.

![Leyendo contenido extraído](images/Pasted%20image%2020260206160401.png)

Parece que tenemos una lista de contraseñas, seguro son para hacer fuerza bruta con SSH.

![Lista de contraseñas](images/Pasted%20image%2020260206160429.png)

Y efectivamente, tras probarlas tenemos las credenciales de **grooti** para SSH (`YoSoYgRoOt`).

![Login SSH exitoso](images/Pasted%20image%2020260206160539.png)

Estamos dentro.

![Shell de usuario](images/Pasted%20image%2020260206160645.png)

## Escalada de Privilegios

Enumerando por tareas cron me encontré con esta:

![Enumeración Cron](images/Pasted%20image%2020260206164653.png)

Tenemos permisos de lectura sobre el archivo `cleanup.sh`.

![Permisos cleanup.sh](images/Pasted%20image%2020260206164735.png)

Dentro del archivo vemos que está ejecutando otro script que se llama `malicious.sh`.

![Cat cleanup.sh](images/Pasted%20image%2020260206164808.png)

Aquí vemos que tenemos **permisos de escritura** dentro del archivo `malicious.sh`.

![Permisos de escritura malicious.sh](images/Pasted%20image%2020260206164849.png)

Voy a agregar esta última línea mandándome una reverse shell al final del archivo.

![Inyectando Reverse Shell](images/Pasted%20image%2020260206165038.png)

Nos ponemos en escucha con Netcat (`nc -lvnp <puerto>`).

![Netcat listening](images/Pasted%20image%2020260206165121.png)

Y así obtenemos una shell como **root**.

![Root Shell](images/Pasted%20image%2020260206165213.png)

**Flag de Root:**
![Flag Root](images/Pasted%20image%2020260206165251.png)





