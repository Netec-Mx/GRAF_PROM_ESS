# 5. Crear alertas de un microservicio en Grafana
Se necesita crear una alerta que le avise al equipo cuando un microservicio se ha caido. 

## Objetivos
- Observar las opciones de alertas que tiene grafana
- Conectar grafana a un webhook 
- Configurar un punto de conexión
- Crear un template personalizado de alertas
- Activar una alerta

---
<div style="width: 400px;">
        <table width="50%">
            <tr>
                <td style="text-align: center;">
                    <a href="../Capitulo4/"><img src="../images/anterior.png" width="40px"></a>
                    <br>anterior
                </td>
                <td style="text-align: center;">
                   <a href="../README.md">Lista Laboratorios</a>
                </td>
<td style="text-align: center;">
                    <a href="../Capitulo6/"><img src="../images/siguiente.png" width="40px"></a>
                    <br>siguiente
                </td>
            </tr>
        </table>
</div>

---

> **IMPORTANTE:** Para este laboratorio es importante tener configurados los siguientes contenedores 
**microservicio cliente**, **prometheus** y **grafana**
en el caso de no tenerlo es importante revisar los laboratorios **3** y **4** de esta guía. 



## Diagrama

![diagrama](../images/5/diagrama.png)


## Instrucciones
Este laboratorio esta separado las siguientes  secciones.

- **[Configuración teams](#configuración-teams-return)**
- **[Conexión teams con Grafana](#conexión-teams-con-grafana-return)**
- **[Creación de template](#creación-de-template-return)**
- **[Crear alerta de grafana](#crear-alerta-de-grafana-return)**

## Configuración teams [return](#instrucciones)
1. Para esta sección necesitaremos una cuenta de **microsoft teams** que nos permita crear canales. 

2. En la sección de **equipos** crear un nuevo canal con el nombre que quieras. 

    ![canal](../images/5/1.png)

3. El canal tendrá las siguientes opciones:

    ![info canal](../images/5/2.png)

4. Ahora en el canal **Administración del canal-> Configuración -> Conectores** 

    ![alt text](../images/5/3.png)

5. Dentro de conectores buscar **Incoming Webhook** este conector va a permitir que grafana pueda enviar alertas al canal de teams

    ![webhook](../images/5/4.png)

6. Cambiar del webhook el **nombre** y la **imagen**

    ![data webhook](../images/5/5.png)

7. Al crear el **webhook** nos va a generar un url **Copiamos y guardamos el url webhook**

    ![webhook](../images/5/6.png)

8. Guarde el url **Webhook url** lo usaremos en la sección de grafana

## Conexión teams con Grafana [return](#instrucciones)

1. En el menu de grafana buscaremos la opción **Alerting->Contact points**

    ![points](../images/5/7.png)

2. Crear un nuevo **Contact point** con la siguiente configuración:
- **Name:** Teams Contact
- **Integration**: Microsoft Teams
- **URL**: ***Webhook url copiado de teams***

    ![contact point](../images/5/8.png)


3. **Save Contact point**


## Creación de template [return](#instrucciones)

1. Abrir el **contact point** creado
2. Abrir las opciones **Optional Microsoft Teams settings** y configurar:
- **Title**: Grafana alerta
- **Message**: Añadir el siguiente código:

```go
{{ if eq .Status "firing" }}
☣️ALERTA CRITICA☣️
- Estado de alerta: {{ .Status }}
{{ range .Alerts }}
- application: {{ index .Labels  "microservice"}}
{{ end }}
{{ else }}
☑️ESTADO SOLUCIONADO☑️
- Estado de alerta: {{ .Status }}
- Receiver: {{ .Receiver }}
{{ range .Alerts }}
- application: {{ index .Labels "microservice" }}
{{ end }}
{{ end }}

```
3. Guardar y probar la alerta, al probarla en nuestro canal de teams deberíamos de observar lo siguiente:

    ![alerta](../images/5/9.png)



## Crear alerta de grafana [return](#instrucciones)

1. Ahora en el menú de grafana abrir **Alert rules**

    ![grafana](../images/5/10.png)

2. La alerta tendrá la siguiente configuración:
- **Name**: Alerta microservicio cliente
- **Query**: Mostrar si el microservicio esta en estado up
```bash
up{job="spring-application"}
```

![query](../images/5/11.png)


- **Threshold**: Si el microservicio esta apagado:

    ![alt text](../images/5/12.png)

- **Folder**: Alertas microservicio

- **Evaluation group**: Cada minuto

    ![grupos](../images/5/13.png)

- **Add Labels**: 
    - microservice: client
    
        ![alt text](../images/5/14.png)

- **Contact Point:** Teams Contact

    ![contact](../images/5/15.png)

3. **Save rule and exit**

4. Esperar un poco hasta que la alerta se active. 


## Resultado Esperado [Instrucciones](#instrucciones)

1. Para probar la alerta es necesario prender y apagar el microservicio cliente

```bash
#----apagar contenedor----
docker stop client 

#----prender contenedor----
docker start client
```

> **NOTA:** Esperar unos 2 minutos entre apagado y prendido para darle tiempo a la alerta. 

![resultado](../images/5/16.png)