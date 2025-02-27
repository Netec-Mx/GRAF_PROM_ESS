# Práctica 2. Instalación de Prometheus y Grafana con Docker
En este laboratorio se busca configurar a Prometheus y Grafana dentro de la tecnología de contenedores. 


## Objetivos
- Comprender la configuración de Docker.
- Usar comandos para crear un contenedor de Prometheus.
- Usar comandos para crear un contenedor de Grafana.
- Explorar las herramientas. 

## Duración aproximada
- 30 minutos.
  
---
<div style="width: 400px;">
        <table width="50%">
            <tr>
                <td style="text-align: center;">
                    <a href="../Capitulo1/"><img src="../images/anterior.png" width="40px"></a>
                    <br>anterior
                </td>
                <td style="text-align: center;">
                   <a href="../README.md">Lista Laboratorios</a>
                </td>
<td style="text-align: center;">
                    <a href="../Capitulo3/">
                    <img src="../images/siguiente.png"
                     width="40px"></a>
                    <br>siguiente
                </td>
            </tr>
        </table>
</div>

---

## Diagrama

![Implementacion](../images/2/diagrama.png)

<br>


## Instrucciones

### Configuración de Prometheus en docker

1. Abrir una terminal dentro de un servidor que tenga docker instalado. 

2. Validar que docker este instalado con el comando: 

```bash
docker version
```

3. Crear una red de docker con el siguiente comando:

```bash
docker network create --subnet 192.168.4.0/24 --gateway 192.168.4.1 monitor
```
![network](../images/2/1.png)


4. Crear el contenedor de Prometheus con el siguiente comando:

>**Nota:** Esta primer versión de Prometheus no llevará configuración adicional. 

```bash
docker run -p 9090:9090 --name prometheus --network monitor -d  prom/prometheus:latest
```

>**IMPORTANTE:** Cuando se ejecute el comando por primera vez tardará un poco por la descarga de la imagen de Prometheus. En caso de error, cambiar la imagen a **edgardovefe/pgclase:prometheus**

5. Validar que el contenedor este iniciado con el comando:

```bash
docker ps
```

![prometheus up](../images/2/2.png)

6. Abrir un explorador web y seguir la siguiente dirección: http://localhost:9090

![prometheus](../images/2/3.png)

>**Nota:** Al abrir la dirección localhost:9090 podrás observar el dashboard de Prometheus.

7. Abrir la dirección http://localhost:9090/metrics para observar las métricas de Prometheus. 

![metrics](../images/2/4.png)


8. Explorar la configuración actual de Prometheus **Status->Configuration**, podrás observar un archivo **yaml**: 

![configuration](../images/2/5.png)

>**Nota:** Analizar la configuración que esta por default.



## Configuración de Grafana en docker
1. Para configurar a Grafana en docker, asegurate de tener la red **monitor en docker**, puedes validar si existe con el siguiente comando:

```bash
docker network ls
```
![network](../images/2/6.png)

2. Usar la versión **open source** de Grafana para las pruebas. 

3. Ejecutar el siguiente comando:

```bash
docker run -p 3000:3000 --name grafana -e GF_SECURITY_ADMIN_PASSWORD=pass --network monitor -d grafana/grafana-oss:latest
```

>**Nota:** El comando anterior crea un contenedor con Grafana y el password de inicio es **pass**.

4. Verificar que el contenedor este iniciado con el comando:

```bash
docker ps
```

![container grafana](../images/2/7.png)


5. Abrir un explorador web y seguir la siguiente dirección: http://localhost:3000 

![dashboard grafana](../images/2/8.png)

6. Escribir el usuario y contraseña para entrar al panel de Grafana:
- **user**: admin
- **password**: pass

![grafana login](../images/2/9.png)


## Resultado esperado

Se espera que el alumno tenga las 2 plataformas iniciadas, tanto Prometheus como Grafana. 

## Prometheus
![prometheus](../images/2/10.png)

## Grafana
![grafana](../images/2/11.png)
