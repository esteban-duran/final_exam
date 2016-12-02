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
![alt text](https://github.com/esteban-duran/final_exam/blob/master/flask_parcial/images/pantallazo 1.PNG "Configuracion inicial")

****
#Instalación de dependencias

Para la ejecución de los ejercicios se emplearán entornos virtuales, por lo cuál es necesario instalar las dependencias necesarias

```
# cd /tmp
# wget https://bootstrap.pypa.io/get-pip.py
# python get-pip.py
# pip install virtualenv
```

#Desarrollo

Crear un usuario para el desarrollo del ejercicio
```
# adduser filesystem_user
# passwd filesystem_user
```
Ingrese la contraseña deseada para el usuario que acaba de crear

#Crear un ambiente virtual

Cree un ambiente virtual
```
# su filesystem_user
$ cd ~/
$ mkdir envs
$ cd envs
$ virtualenv flask_env
```
#Activar un ambiente virtual

Active el ambiente virtual creado
```
$ cd ~/envs
$ . flask_env/bin/activate
```

#Instalación de Flask

Con el ambiente activo, instale la libreria Flask
```
$ pip install Flask
```
#Ejercicio

Cree un directorio para alojar los ejercicios
```
$ cd ~/
$ mkdir -p flask_parcial/
$ cd flask_examples/
```
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

###Pruebas del servicio web (REST)
#### files
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/files_GET.PNG "GET")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/files_POST.PNG "POST")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/files_PUT.PNG "PUT")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/files_DELETE.PNG "DELETE")

#### files/recently_created
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/rc_GET.PNG "GET")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/rc_POST.PNG "POST")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/rc_PUT.PNG "PUT")
![alt text](https://github.com/esteban-duran/12103025-parcial_uno/blob/master/flask_parcial/images/rc_DELETE.PNG "DELETE")


#Dirección al github de la soluciónn
https://github.com/esteban-duran/12103025-parcial_uno
