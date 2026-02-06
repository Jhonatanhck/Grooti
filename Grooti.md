
Maquina

![[Pasted image 20260206153124.png]]
Primero tenemos que desplegar la maquina para empezar a operar

Reconocimiento

![[Pasted image 20260206153248.png]]
Obtenemos esto del escaneo de nmap, pasare a enumerar la pagina web

![[Pasted image 20260206153351.png]]
Tenemos esto en la pagina web

![[Pasted image 20260206153442.png]]
En el directorio de imagenes tenemos esta contrasena, y nos dice que encontremos donde ponerla
(password1)

![[Pasted image 20260206153623.png]]
En el codigo fuente tenemos esta nota que parece ser el nombre de algun usuario a nivel de sistema

Probare el usuario rocket y la contrasena que encontramos para conectarme a la base de datos

![[Pasted image 20260206153821.png]]
Y pudimos entrar vamos a ver que podemos enumerar aqui dentro

![[Pasted image 20260206153853.png]]
Y tenemos esta base de datos que parece ser interesante

![[Pasted image 20260206154144.png]]
Y tenemos esto que son rutas, la ultima es la que no teniamos de antes, asi que la probare en el navegador

![[Pasted image 20260206154749.png]]
Tenemos esto al acceder a la ruta que descubrimos


![[Pasted image 20260206155743.png]]
Al poner whoami y un numero random del 1-100 se me decargar un archivo llamado password100.txt

![[Pasted image 20260206155831.png]]
Nos devuelve el mismo output


![[Pasted image 20260206155904.png]]
Lo que se me ocurrio para ir mas rapido 1 por 1 fue abrir el burpsuite e iterar 1 por 1 

![[Pasted image 20260206155940.png]]
y el numero 16 me dio una respuesta diferente a las demas

![[Pasted image 20260206160005.png]]
Cuando veo la respuesta parece ser un archivo zip

![[Pasted image 20260206160033.png]]
Asi que ahora poniendo el 16 se me descargar un archivo zip

![[Pasted image 20260206160125.png]]
Aqui tenemos el archivo

![[Pasted image 20260206160159.png]]
Parece que necesita una contrasena

probare la contrasena que descubrimos al inicio

![[Pasted image 20260206160401.png]]
Funciono, vere que hay dentro del txt

![[Pasted image 20260206160429.png]]
parece que tenemos una lista de contrasena seguro son para hacer fuerza bruta con ssh

![[Pasted image 20260206160539.png]]
Y efectivamente, tenemos las credenciales de grooti para ssh (YoSoYgRoOt)

![[Pasted image 20260206160645.png]]
Estamos dentro


![[Pasted image 20260206164653.png]]
Enumerando por tareas cron me encontre con esta 

![[Pasted image 20260206164735.png]]
Tenemos permisos de lectura sobre el archivo cleanup.sh

![[Pasted image 20260206164808.png]]
Dentro del archivo vemos que esta ejecutando otro script que se llama malicious.sh

![[Pasted image 20260206164849.png]]
aqui vemos que tenemos permisos de escritura dentro del archivo malicious.sh

![[Pasted image 20260206165038.png]]
Voy a agregar esta ultima linea mandandome una reverse shell 

![[Pasted image 20260206165121.png]]
Nos ponemos en escucha

![[Pasted image 20260206165213.png]]
Y asi obtenemos una shell como root

Grooti
![[Pasted image 20260206165251.png]]






