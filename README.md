# Examen 1 - Sistemas Distribuidos
## Integrantes:
- Jesús Paz
- Felipe Cortez
- Juan David Bolaños

### Nota:
  Deben tener agregada la vagrant box llamada "centos/7"

## Configuración inicial

Deben ingresar los siguientes comandos:
1) Para clonar el repositorio: 
~~~
  git clone https://github.com/JesusPaz/sd2019b-exam1.git
~~~
2) Entran a la carpeta raiz:
~~~
  cd sd2019b-exam1
~~~
3) Deben instalar:
~~~
  vagrant plugin install vagrant-vbguest
~~~
4) Ya tienen todo listo para iniciar las maquinas:
~~~
  vagrant up
~~~

Al teminar de hacer el vagrant up, deben de ingresar a la maquina master. Esta permitir instalar todos los paquetes en el resto que maquinas (minions). Para esto deben ejecutar:
~~~
  vagrant ssh master
~~~
Para ver que todo se genero correstamente y que el master puede alcanzar a los minions deben ejecutar:
~~~
  salt '*' test.ping
~~~
Al entrar en la master deben ejecutar los siguientes comandos para instalar todas las dependencias relacionadas con el Load Balancer e instalar apache en los servidores web:
~~~
  sudo su
  salt-run state.orch apache_orchestration
~~~
Para obtener la dirección ip del Load Balancer ejecutan:
~~~
  salt loadbalancer network.ipaddrs eth1
~~~
Luego ingresan esa dirección a su navegador, recargan para ver la diferencia entre los servidores.


Si en la terminal todo les sale correcto, quiere decir que ya todo quedó bien instalado. :)
