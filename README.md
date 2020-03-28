# Ejemplos de Spring Cloud Stream y Spring Cloud Data Flow

### Basado en:

(Con correcciones y algunos detalles mas)

* Spring Cloud Stream con RabbitMQ:

https://dataflow.spring.io/docs/stream-developer-guides/streams/standalone-stream-rabbitmq/ 

* Spring Cloud Data Flow:

https://dataflow.spring.io/docs/stream-developer-guides/streams/data-flow-stream/

### Spring Cloud Stream

* Levantar Rabbit:

docker run -d --hostname rabbitmq --name rabbitmq -p 15672:15672 -p 5672:5672 rabbitmq:3.7.14-management

En http://localhost:15672/ se puede ver la consola, acceder con guest/guest

* Levantar Jaeger:

docker run -d -p 6831:6831/udp -p 16686:16686 jaegertracing/all-in-one:latest

 No disponible en el ejemplo original. Se adapto para que se puedan ver las trazas.
Al utilizar librerías (Spring Clou Stream) que ya tienen Open Tracing adaptado, configurando
el nombre de la aplicación, librería y dirección del server, se podrán ver las trazas en:

http://localhost:16686/

* Levantar este ejemplo en algun ide o por linea de comando.

* Levantar el ejemplo:

https://github.com/diegochavezcarro/usage-cost-processor-rabbit

* Levantar el ejemplo:

https://github.com/diegochavezcarro/usage-cost-logger-rabbit

* Tambien se pueden levantar los 3 ejemplos en cualquier orden, ver en la consola de RabbitMQ 
cómo se acumulan los mensajes en las queues (se pueden leer tambien los mensajes que llegan
a las Queues). Tambien ir viendo las Connections y los Channels.

### Spring Cloud Data Flow

* Dejar levantado Rabbit. En el caso de los ejemplos de Tasks no es necesario.

* Levantar las diferentes herramientas:

java -jar spring-cloud-dataflow-server-2.4.2.BUILD-20200310.115040-7.jar

java -jar spring-cloud-skipper-server-2.3.2.BUILD-20200310.102318-6.jar

La Shell no es necesaria si se realiza todo desde la consola UI, en el ejemplo a veces sí
la uso:

java -jar spring-cloud-dataflow-shell-2.4.2.BUILD-20200310.115040-7.jar

* (Opcional) Si se quiere realizar el ejemplo con las aplicaciones pre armadas 
(https://dataflow.spring.io/docs/stream-developer-guides/getting-started/stream/), 
importarlas en la consola de la Shell:

app import --uri https://dataflow.spring.io/rabbitmq-maven-latest

* Si se quiere ver el ejemplo de Tasks con tareas compuestas, importar tambien las tasks:

app import --uri https://dataflow.spring.io/task-maven-latest

* Instalar en Maven los 3 ejemplos. Los tests andan bien, pero si se quiere acelerar 
con las diferentes pruebas ejecutar en cada uno de ellos:

./mvnw install -DskipTests

* Seguir el tutorial, donde se instalan las aplicaciones usar:

maven://io.spring.dataflow.sample:usage-detail-sender-rabbit:0.0.1-SNAPSHOT

maven://io.spring.dataflow.sample:usage-cost-processor-rabbit:0.0.1-SNAPSHOT

maven://io.spring.dataflow.sample:usage-cost-logger-rabbit:0.0.1-SNAPSHOT

Existe la posibilidad de usar Docker tambien, ver tutorial, si no coincide version de jdk
utilizar plugin de Spotify con un Dockerfile que incluya version correspondiente o generar
imagen a mano.

Los ejemplos incluyen no sólo Actuator sino Web ya que sino no se levantan bien los endpoints
de Health y la aplicación va a aparecer siempre como "Deploying" en Spring Cloud Data Flow.

A diferencia con el tutorial de la parte local, no es necesario setear el puerto (el cual igual
aca se usa en el app properties para ejecutar el ejemplo anterior),
de hecho el SCDF sobrescribe varias de las propiedades incluyendo esa, levantando un puerto
diferente para cada app. También ver en la consola del Rabbit cómo se utilizan otras 
queues a las que están definidas en el app properties, de hecho no es necesario con SCDF
incluir la config de las mismas.
Si se quiere sobrescribir alguna de las propiedades que el SCDF usa por defecto
hay que crear un white list:

https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#spring-cloud-dataflow-stream-app-whitelisting

El mismo puede estar en otro jar (ver sección siguiente a la anterior url) llamado de metadata. En el caso de utilizar docker 
de hecho esa metadata va a tener que estar en un jar con la config de maven y no en otra 
imagen de Docker.
