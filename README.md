****
Nombre | Código
--- | --- | ---
Esteban Durán Giraldo | 12103025 
Jose Luis Sacanamboy | 13103010
****
#Configuración de la máquina virtual
##Sistema Operativo Centos 7
Para la configuración del sistema se requiere haber descargado previamente la imagen minimal de centos 7, esta se puede encontrar en el siguiente link http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1511.iso

###Creación de la máquina virtual
Darle el nombre a la máquina y escoger la arquitectura del sistema operativo, en nuestro caso Red Hat 64 bits
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%201.PNG "Configuracion inicial")

A continuación se establece el tamaño de la memoria RAM sugerida
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%202.PNG "Memoria RAM")

Luego se establece el orden de arranque
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%203.PNG "Orden de arranque")

Se escogen la cantidad de procesadores, la cantidad sugerida son 2
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%204.PNG "Procesadores")

Se selecciona el ISO previamente descargado para arrancar en la unidad virtual de CD
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%205.PNG "Seleccion ISO")

Se congifuran las interfaces de red para trabajar
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%206.PNG "Interfaz de red NAT")
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%207.PNG "Interfaz de red de puente")

Una vez terminada la configuración se corre la máquina y se instala el sistema operativo
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%208.PNG "Instalacion de centos 7")
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%208.1.PNG "Instalacion de centos 7")

Se escoge el idioma a mostrar, la configuración del teclado por defecto corresponde al lenguaje seleccionado
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%209.PNG "Lenguaje")

Se escoge el disco virtual sobre el que se va a instalar el SO
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%209.1.PNG "Disco a instalar")

Se configura la cuenta root
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2010.PNG "Cuenta root")

Se configura la cuenta del usuario sin privilegios root
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2011.PNG "Cuenta alterna")

Es necesario seleccionar para las interfaces de red la siguiente opcion
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2012.PNG "RED")

Ahora todo en orden, es necesario reiniciar la máquina
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2013.PNG "Instalación finalizada")

****
#Instalación de dependencias
Una vez terminada la configuración inicial se procede a la ejecución de los ejercicios, para los cuales se emplearán entornos virtuales, por lo qué es necesario instalar las dependencias requeridas. Es necesario ejecutar los comandos a continuación desde la cuenta **root**

Se verifica las interfaces de red y la ip asignada a la maquina con el siguiente comando

```
# ip a
```
Arroja el siguiente resultado
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2014.PNG "ip a")


Para verificar  el correcto funcionamiento de las interfaces de red es necesario ejecutar los siguientes comandos

```
# cd /etc/sysconfig/network-scripts/
# ls
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2016.PNG "verificacion de red")

Los archivos subrayados en rojo son las interfaces que se crearon en el momento de la configuración inicial, es necesario verificar que se ejecuten al iniciar la máquina virtual, los siguientes comandos nos ayudarán a esto

```
# cat ifcfg-enp0s3
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2017.PNG "verificacion de red nat")


```
# cat ifcfg-enp0s8
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2018.PNG "verificacion de red bridge")

Una vez se rectifique el correcto funcionamento se instala el **firewallD** para un mejor manejo de los puertos, con el comando

```
# yum install firewalld -y
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2020.PNG "verificacion de red nat")

Una vez instalado se añade el puerto **9090** a los permitidos, que es sobre el cuál se ejecutará el servicio
```
# firewall-cmd --zone=public --add-port=9090/tcp --permanent
# firewall-cmd --reload
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2022.PNG "apertura puertos")

Ahora es necesario verificar si el puerto fue añadido correctamente, para hacer esto se ejecuta el comando
```
# firewall-cmd --list-all
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2025.PNG "Verificacion puertos")

Luego se instala **netstat** 
```
# yum install net-tools -y
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2026.PNG "Nmap")

También es necesario el servicio **git** para trabajar en repositorios
```
# yum install git -y
```

Ahora se añade el usuario **python_user** a la lista de los privilegiados para ejecutar comandos root
```
# visudo
```
Se añade la siguiente linea
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2028.PNG "Nmap")
Luego se ejecuta el comando
```
# usermod -G wheel python_user
```
Luego se instala el servicio **Wget**
```
# yum install wget
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2030.PNG "Wget")

Luego se instala el servicio **python**
```
# yum install wget
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2032.PNG "python")

```
# cd /tmp
# wget https://bootstrap.pypa.io/get-pip.py
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2031.PNG "python")
```
# python get-pip.py
# pip install virtualenv
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2033.PNG "python")

****

#Desarrollo del ejercicio
Cambie al usuario previamente creado **python_user**
```
# su python_user
```
#Crear un ambiente virtual

Cree un ambiente virtual
```
$ cd ~/
$ mkdir envs
$ cd envs
$ virtualenv flask_env
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2034.PNG "enviroments")

#Activar un ambiente virtual

Cree un directorio para alojar los ejercicios
```
$ cd ~/
$ mkdir -p flask_parcial/
$ cd flask_final/
$ mkdir files_created
$ vi files.py
$ vi files_command.py
```
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2035.PNG "creacion de directorios")

Cree un archivo de nombre **files.py**
```python
from flask import Flask, abort, request
import json, os

from files_commands import get_all_files, remove_file, get_all_recent_files

app = Flask(__name__)

@app.route('/files', methods=['POST'])
def create_file():
  content = request.get_json(silent=True)
  filename = content['filename']
  content = content['content']
  os.chdir('files_created')
  add_process = open(filename+'.txt','a')
  add_process.write(content+'\n')
  add_process.close()
  os.chdir('..')
  return "File created",201

@app.route('/files', methods=['GET'])
def read_file():
  list = {}
  list["files"] = get_all_files()
  return json.dumps(list), 200

@app.route('/files', methods=['PUT'])
def update_file():
  return "not found", 404

@app.route('/files', methods=['DELETE'])
def delete_file():
  error = False
  for filename in get_all_files():
    if not remove_file(filename):
      error = True
  if error:
    return 'some files were not deleted', 400
  else:
    return 'all files were deleted', 200

@app.route('/files/recently_created', methods=['POST'])
def create_recent_file():
  return "not found", 404

@app.route('/files/recently_created', methods=['GET'])
def read_recent_file():
  list = {}
  list["files"] = get_all_recent_files()
  return json.dumps(list), 200

@app.route('/files/recently_created', methods=['PUT'])
def update_recent_file():
  return "not found", 404

@app.route('/files/recently_created', methods=['DELETE'])
def delete_recent_file():
  return "not found", 404

if __name__ == "__main__":
  app.run(host='0.0.0.0', port=9090, debug='True')
```
Cree un archivo de nombre **files_commands.py**
```python
from subprocess import Popen, PIPE
import os
def get_all_files():
  file_list = Popen(('ls','files_created'), stdout=PIPE, stderr=PIPE).communicate()[0].split('\n')
  return filter(None,file_list)

def remove_file(filename):
    os.chdir('files_created')
    remove_process = Popen(["rm","-rf",filename], stdout=PIPE, stderr=PIPE)
    remove_process.wait()
    os.chdir('..')
    return False if filename in get_all_files() else True

def get_all_recent_files():
  file_list = Popen(('ls','files_created','-t'), stdout=PIPE, stderr=PIPE).communicate()[0].split('\n')
  return filter(None,file_list)
```

##Active el ambiente virtual creado
```
$ cd ~/envs
$ . flask_env/bin/activate
```

#Instalación de Flask

Con el ambiente activo, instale la libreria Flask y ejecute el servicio
```
$ pip install Flask
$ python files.py
```
#Ejercicio

###Pruebas del servicio web (REST)
### files
#### GET
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/files_GET "GET")
#### POST
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/files_POST "POST")
#### PUT
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/files_PUT "PUT")
#### DELETE
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/files_DELETE "DELETE")

### files/recently_created
#### GET
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/recent_GET "GET")
#### POST
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/recent_POST "POST")
#### PUT
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/recent_PUT "PUT")
#### DELETE
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/recent_DELETE "DELETE")

#Pruebas de Flask con Netstat
![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2037.PNG "netstat 1")

![alt text](https://github.com/esteban-duran/final_exam/blob/master/images/pantallazo%2038.PNG "netstat 2")
#Dirección al github de la solución
https://github.com/esteban-duran/12103025-parcial_uno
