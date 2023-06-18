# Practica calificada 4
Resolución de la practica_calificada4
**Departamento Académico de Ingeniería ![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.001.png)**

**C8280 -Comunicación de Datos y Redes** 

**Amazon ELB - Auto Scaling** 

**Indicaciones** 

1\. Las respuestas deben ser explicadas, solo colocar resultados sin ninguna referencia no puntúa en las preguntas de la evaluación. 

2\. Realiza una copia de este documento y coloca todas tus respuestas y sube a tu repositorio personal de github en formato markdown. Presenta capturas de pantalla del procedimiento y las explicaciones necesarias. No puntúa si solo se hace la presentación de imágenes. 

3\. De preferencia adiciona un video adicional explicando los pasos realizados. Utiliza el sandbox de AWS usado en la práctica anterior. 

4\. Sube a la plataforma de Blackboard el enlace de github donde están todas tus respuestas. No olvides colocar tu nombre y apellido antes de subir el enlace de tus respuestas a la plataforma 

5\. Cualquier evidencia de copia elimina el examen se informará de la situación a la coordinación. 

**Amazon ELB** 

Aquí, usamos Amazon Elastic Load Balancing (ELB) y Amazon Cloud Watch a través de la CLI de AWS para equilibrar la carga de un servidor web. 

**Parte 1: ELB** 

**1. Inicia sesión en el sandbox del curso AWS . Ve al directorio donde guarda el archivo de script de instalación de apache del laboratorio de la práctica calificada 3. Para crear un balanceador de carga, haz lo siguiente.** 

aws elb create-load-balancer --load-balancer-name 

tu\_nombre de usuario --oyentes 

"Protocolo=HTTP,LoadBalancerPort=80,InstanceProtocol= 

HTTP,InstancePort=80" --availability-zones us-east-1d 

aws elb create-load-balancer --load-balancer-name kimberly-salazar --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" --availability-zones us-east-1d


Comunicación de Datos y Redes

**C8280 -Comunicación de Datos y Redes** 

**¿Cuál es el DNS\_Name del balanceador de carga? ![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.002.png)**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.003.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.003.png)

**2. El comando describe-load-balancers describe el estado y las propiedades de tu(s) balanceador(es) de carga. Presenta este comando.** 

aws elb describe-load-balancers 

--load-balancer-name tu\_nombre\_de\_usuario

`  `aws elb describe-load-balancers --load-balancer-name kimberly-salazar

**¿Cuál es la salida?** La salida es la descripción del estado y propiedades del balanceador de carga creado. Esta información puede incluir detalles como el nombre del balanceador de carga, los grupos de seguridad asociados, las instancias de EC2.etc.

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.004.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.005.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.006.png)

**3. Creamos dos instancias EC2, cada una ejecutando un servidor web Apache. Emite lo siguiente.** 

aws ec2 run-instances --image-id ami-d9a98cb0 --count 2 

--instance-type t1.micro --key-name tu\_nombre\_de\_usuario-key 

--security-groups tu\_nombre\_de\_usuario 

--user-data file://./apache-install --placement AvailabilityZone=us-east-1d 

aws ec2 run-instances --image-id ami-d9a98cb0 --count 2 --instance-type t1.micro --key-name kim\_salazar-key --security-groups kim\_salazar --user-data file://./apache-install --placement AvailabilityZone=us-east-1d

**¿Qué parte de este comando indica que deseas dos instancias EC2?** 

aws ec2 run-instances --image-id ami-d9a98cb0 **--count 2**

--instance-type t1.micro --key-name tu\_nombre\_de\_usuario-key 

--security-groups tu\_nombre\_de\_usuario 

--user-data file://./apache-install --placement AvailabilityZone=us-east-1d 

**¿Qué parte de este comando garantiza que tus instancias tendrán Apache instalado?** 

aws ec2 run-instances --image-id ami-d9a98cb0 --count 2 

--instance-type t1.micro --key-name tu\_nombre\_de\_usuario-key 

--security-groups tu\_nombre\_de\_usuario 

**--user-data file://./apache-install** --placement AvailabilityZone=us-east-1d 

**¿Cuál es el ID de instancia de la primera instancia?**  i-08623a7a651fa1bb7

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.007.png)

**¿Cuál es el ID de instancia de la segunda instancia?** i-0f55925035f9c56c6

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.008.png)

4\. **Para usar ELB, tenemos que registrar las instancias EC2. Haz lo siguiente, donde instance1\_id e instance2\_id son los obtenidos del comando en el paso 3.** 

aws elb register-instances-with-load-balancer 

--load-balancer-name tu\_nombre\_de\_usuario 

--instances instance1\_id instancia2\_id 

aws elb register-instances-with-load-balancer --load-balancer-name kimberly-salazar --instances i-0c0a95df6a3426dd0 i-02ebbc93d652d84fb

**¿Cuál es la salida?  La salida muestra que ya se registraron las instancias con un balanceador de carga en AWS.**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.009.png)

Ahora vea el estado de la instancia de los servidores cuya carga se equilibra. 

aws elb describe-instance-health 

--load-balancer-name tu\_nombre\_de\_usuario 

aws elb describe-instance-health --load-balancer-name kimberly-salazar

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.010.png)

**5. Abre el navegador del sandox. Recupera la dirección IP de tu balanceador de carga del paso 1, ingresa la URL http://nombre\_dns\_de\_tu\_balanceador\_carga/ en tu navegador web. ¿Qué apareció en el navegador?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.011.png)

**Pero si nos saliera de la siguiente forma:**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.012.png)

**ssh -i devenv-key.pem ubuntu@id intancia**

**sudo nano /etc/apt/sources.list** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.013.png)

**deb http://old-releases.ubuntu.com/ubuntu precise main restricted universe multiverse**

**deb http://old-releases.ubuntu.com/ubuntu precise-updates main restricted universe multiverse**

**deb http://old-releases.ubuntu.com/ubuntu precise-security main restricted universe multiverse**

**6. Abre dos ventanas de terminal adicionales y ssh en ambos servidores web. En cada uno, cd al directorio DocumentRoot (probablemente /usr/local/apache/htdocs) y modifique la página de inicio predeterminada, index.html, de la siguiente manera.** 

<html><body><h1>¡Funciona!</h1> 

<p>La solicitud se envió a la instancia 1.</p> 

<p>La solicitud fue atendida por el servidor web 1.</p> 

</body></html> 

Para el segundo servidor, haz lo mismo excepto que use la instancia 2 y el servidor 2 para las líneas 2 y 3. En el navegador web, accede a tu balanceador de carga 4 veces (actualícelo/recárgalo 4 veces). Esto genera 4 solicitudes a tu balanceador de carga. 

**ubuntu@ip-172-31-18-231:~$ sudo -s**

**root@ip-172-31-18-231:~# cd /var/www**

**root@ip-172-31-18-231:/var/www# ls**

**index.html**

**root@ip-172-31-18-231:/var/www# vi index.html**

**root@ip-172-31-18-231:/var/www#** 

**DNS: kimberly-salazar-1929973315.us-east-1.elb.amazonaws.com**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.014.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.015.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.016.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.017.png)**¿Cuántas solicitudes atendió el servidor web 1?** 

Atendió 2 solicitudes 

**¿Cuántas solicitudes atendió el servidor web 2?** 

Atendió 2 solicitudes 

**Parte 2: CloudWatch** 

**7. CloudWatch se utiliza para monitorear instancias. En este caso, queremos monitorear los dos servidores web. Inicia CloudWatch de la siguiente manera.** 

aws ec2 monitor-instances 

--instance-ids instance1\_id instance2\_id 

**¿Cuál es la salida?**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.018.png)

` `Ahora examina las métricas disponibles con lo siguiente: aws cloudwatch list-metrics --namespaces "AWS/EC2" 

**aws cloudwatch list-metrics --namespace "AWS/EC2"**

**¿Viste la métrica CPUUtilization en el resultado?** 


![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.019.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.020.png)

**8. Ahora configuramos una métrica para recopilar la utilización de la CPU. Obtener la hora actual con date -u. Esta será tu hora de inicio. Tu hora de finalización debe ser 30 minutos más tarde. Haz lo siguiente.** 

aws cloudwatch get-metric-statistics 

--metric-name CPUUtilization 

--start-time your\_start\_time --end-time 

your\_end\_time --period 3600 --namespace AWS/EC2 

--statistics Maximum 

– dimensions Name=InstanceId,Value=instance2\_id 

**aws cloudwatch get-metric-statistics --metric-name CPUUtilization --start-time 2023-06-15T01:23:27Z --end-time 2023-06-15T01:53:27Z --period 3600 --namespace AWS/EC2 --statistics Maximum --dimensions Name=InstanceId,Value=i-0f55925035f9c56c6** 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.021.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.022.png)

9\. Apache tiene un herramienta benchmark llamada ab. Si desea ver más información sobre ab, consulte http://httpd.apache.org/docs/2.0/programs/ab.html. Para ejecutar ab, emita el siguiente comando en tu sistema de trabajo. 

ab -n 50 -c 5 http://nombre\_dns\_de\_tu\_balanceador\_carga/ 

**ab -n 50 -c 5 http://kimberly-salazar-1929973315.us-east-1.elb.amazonaws.com/** 

**¿Qué significan -n 50 y -c 5? ¿Cuál es la salida?.** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.023.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.024.png)

**•	-n indica el número total de solicitudes que se enviarán al servidor web. En el ejemplo proporcionado -n 50, se realizaron un total de 50 solicitudes al servidor web.**

**•	-c representa el número de solicitudes concurrentes que se enviarán al servidor web al mismo tiempo. En el ejemplo -c 5, se enviarán 5 solicitudes concurrentes en cada momento.**

**10. Ahora queremos examinar la métrica de latencia del ELB. Utiliza el siguiente comando con las mismas horas de inicio y finalización que especificó en el paso 8.** 

aws cloudwatch get-metric-statistics --metric-name Latency 

--start-time your\_start\_time --end-time your\_end\_time --period 3600 --namespace AWS/ELB 

--statistics Maximum --dimensions 

Name=LoadBalancerName,Value=tu\_nombre\_de\_usuario 

aws cloudwatch get-metric-statistics --metric-name 

RequestCount --start-time your\_start\_time --end-time your\_end\_time --period 3600 --namespace AWS/ELB 

--statistics Sum --dimensions 

Name=LoadBalancerName,Value=tu\_nombre\_de\_usuario 

**aws cloudwatch get-metric-statistics --metric-name Latency --start-time 2023-06-15T01:23:27Z --end-time 2023-06-15T01:53:27Z --period 3600 --namespace AWS/ELB --statistics Maximum --dimensions Name=LoadBalancerName,Value=kimberly-salazar**

**aws cloudwatch get-metric-statistics --metric-name RequestCount --start-time 2023-06-15T01:23:27Z --end-time 2023-06-15T01:53:27Z --period 3600 --namespace AWS/ELB --statistics Sum --dimensions Name=LoadBalancerName,Value=kimberly-salazar**

**¿Qué resultados recibió de ambos comandos?** 

**Primer comando:**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.025.png)

**Segundo comando:**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.026.png)

**Parte 3: Limpieza** 

**11. Necesitamos cancelar el registro de las instancias de ELB. Haz lo siguiente**.

` `aws elb desregister-instances-from-load-balancer 

--load-balancer-name tu\_nombre\_de\_usuario 

--instances instance1\_id instance2\_id 

**aws elb deregister-instances-from-load-balancer --load-balancer-name kimberly-salazar --instances i-08623a7a651fa1bb7  i-0f55925035f9c56c6**

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.027.png)

**12. A continuación, eliminamos la instancia de ELB de la siguiente manera.**
**
` `aws elb delete-load-balancer --load-balancer-name 

tu\_nombre\_de\_usuario 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.028.png)

Finalmente, finaliza las instancias del servidor web de tus instancias y tu instancia EC2. 

**¿Qué comandos usaste?**

**[cloudshell-user@ip-10-6-95-182 ~]$ aws ec2 terminate-instances --instance-ids i-08623a7a651fa1bb7 i-0f55925035f9c56c6**

` `**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.029.png)

**Auto Scaling** 

Usamos AWS CLI para configurar sus instancias EC2 para el escalado automático. **Parte 1: escalar hacia arriba** 

**1. Inicia sesión en el sandbox virtual. Cambia al directorio donde guarda el archivo de script de instalación de apache. Inicie una instancia de la siguiente manera.** aws autoscaling create-launch-configuration 

--launch-configuration-name *tu\_nombre\_de\_usuario*-lc 

--image-id ami-d9a98cb0 --instance-type t1.micro 

--key-name *tu\_nombre\_usuario*-key --security-groups 

*tu\_nombre\_usuario* --user-data file://./apache-install 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.030.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.031.png)












A continuación, crea un equilibrador de carga. 

aws elb create-load-balancer --load-balancer-name 

tu\_nombre\_de\_usuario-elb --listeners 

"Protocol=HTTP, LoadBalancerPort=80, 

InstanceProtocol=HTTP,InstancePort=80" 

– availability-zones us-east-1c 

¿Cuál es la salida? ![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.032.png)



**2. Ahora creamos un grupo de escalado automático. Haz lo siguiente.** 

aws autoscaling create-auto-scaling-group 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

--launch-configuration-name tu\_nombre\_de\_usuario-lc 

--min-size 1 --max-size 3 --load-balancer-names 

tu\_nombre\_de\_usuario-elb --availability-zones us-east-1c 

**¿Cuál es la salida? ![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.033.png)**

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.034.png)






![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.035.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.036.png)





3\. **Para describir el grupo de escalado automático que acabas de crear, emite el siguiente comando.** 

aws autoscaling describe-auto-scaling-groups 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

¿Cuál es la salida? Deberías ver que se crea una nueva instancia EC2. Si no lo ves, espera 2 minutos y vuelve a ejecutar el comando. 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.037.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.038.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.039.png)

¿Cuál es el ID de la instancia?

i-0407fd2c4e8f0b7a9

4\**. Ahora creamos una política de escalado hacía arriba seguida de una alarma de CloudWatch para determinar, en caso de que lapolítica sea cierta, que AWS necesita ampliar nuestros recursos. Escribe los dos comandos que siguen. AnotA el valor de PolicyARN del primer comando.** 

aws autoscaling put-scaling-policy 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

--policy-name tu\_nombre\_de\_usuario-scaleup 

--scaling-adjustment 1 

--adjustment-type ChangeInCapacity --cooldown 120 

aws cloudwatch put-metric-alarm --alarm-name 

tu\_nombre\_de\_usuario-highcpualarm 

--metric-name CPUUtilization --namespace AWS/EC2 

--statistic Average --period 120 --threshold 70 

--comparison-operator GreaterThanThreshold 

--dimensions "Name=AutoScalingGroupName,Value= 

tu\_nombre\_de\_usuario-asg" --evaluation-periods 1 

--alarm-actions value\_of\_PolicyARN 

"PolicyARN": "arn:aws:autoscaling:us-east-1:091101640480:scalingPolicy:885a9e7f-b954-48dc-9a71-af49368326dd:autoScalingGroupName/kimberly-salazar-asg:policyName/kimberly-salazar-scaleup"

**¿Cuál es la salida de ambos comandos?** 

Primer comando:

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.040.png)

Segundo comando:

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.041.png)

**¿Cuál es el valor de PolicyARN? (El valor es una cadena larga sin comillas).** 

arn:aws:autoscaling:us-east-1:091101640480:scalingPolicy:885a9e7f-b954-48dc-9a71-af49368326dd:autoScalingGroupName/kimberly-salazar-asg:policyName/kimberly-salazar-scaleup

Ejecuta el siguiente comando para ver una descripción de tu alarma. 

aws cloudwatch describe-alarms --alarm-names 

tu\_nombre\_de\_usuario-highcpualarm 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.042.png)







**5. Inicia sesión en la instancia EC2 desde el paso 1 mediante ssh. Cambia al root. Cargaremos y ejecutaremos una herramienta stress para aumentar la utilización de procesamiento del servidor. Emite los siguientes comandos de Linux.** 

apt-get install stress 

stress--cpu 1 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.043.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.044.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.045.png)

Inicia un nuevo terminal. Repite el siguiente comando cada 2 minutos hasta que veas la segunda instancia EC2 y luego una tercera instancia EC2. En este punto, emite las siguientes instrucciones en la ventana de tu terminal inicial. 

aws autoscaling describe-auto-scaling-groups 

--auto-scaling-group-name tu\_nombre\_de\_usuario -asg 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.046.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.047.png)











![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.048.png)






**Parte 2: reducir la escala** 

6\. Ahora exploraremos cómo AWS puede controlar el escalado hacia abajo mediante la eliminación de algunas de las máquinas virtuales creadas. Ejecuta los siguientes dos comandos, nuevamente tomando nota del PolicyARN creado a partir del primer comando para usar en el segundo. 

aws autoscaling put-scaling-policy 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

--policy-name tu\_nombre\_de\_usuario-scaledown 

--scaling-adjustment -1 --adjustment-type 

ChangeInCapacity --cooldown 120 

aws cloudwatch put-metric-alarm --alarm-name 

tu\_nombre\_de\_usuario-lowcpualarm 

--metric-name CPUUtilization --namespace AWS/EC2 

--statistic Average --period 120 --threshold 30 

--comparison-operator LessThanThreshold --dimensions 

"Name=AutoScalingGroupName,Value=tu\_nombre\_de\_usuario-asg" 

--evaluation-periods 1 --alarm-actions 

value\_of\_PolicyARN 

**¿Cuáles son las salidas?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.049.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.050.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.051.png)

**¿Cuál es el valor de PolicyARN?** 

**"PolicyARN": "arn:aws:autoscaling:us-east-1:091101640480:scalingPolicy:00bbd2c9-67ab-43fa-9834-193418c4402f:autoScalingGroupName/kimberly-salazar-asg:policyName/kimberly-salazar-scaledown",**

**7. Cambia al terminal de la instancia EC2. Escribe ctrl+c para detener el comando stress. Vuelva a la ventana del terminal y repite el siguiente comando cada 2 minutos hasta que vea solo un EC2 en su grupo de escalado automático.** 

aws autoscaling describe-auto-scaling-groups 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.052.png)![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.053.png)



![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.054.png)




**Parte 3: Limpieza** 

8\. Elimina el grupo de escalado automático mediante el siguiente comando. Si el grupo tiene instancias o actividades de escalado en curso, debes especificar la opción para forzar la eliminación para que se realice correctamente. Si el grupo tiene políticas, al eliminar el grupo se eliminan las políticas, las acciones de alarma subyacentes y cualquier alarma que ya no tenga una acción asociada. Después de eliminar tu grupo de escala, elimina tus alarmas como se muestra a continuación. 

aws autoscaling delete-auto-scaling-group 

--auto-scaling-group-name tu\_nombre\_de\_usuario-asg 

--force-delete 

aws cloudwatch delete-alarms --alarm-name 

tu\_nombre\_de\_usuario-lowcpualarm 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.055.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.056.png)

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.057.png)

aws cloudwatch delete-alarms --alarm-name 

tu\_nombre\_de\_usuario-highcpualarm 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.058.png)

Elimina tu configuración de lanzamiento de la siguiente manera. 

aws autoescaling delete-launch-configuration 

--launch-configuration-name tu\_nombre\_de\_usuario-lc

**[cloudshell-user@ip-10-6-39-175 ~]$ aws autoscaling delete-launch-configuration --launch-configuration-name kimberly-salazar-lc** 

**¿Cuál es la salida?** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.059.png)

Finalmente, elimina tu ELB. ¿Qué comando ejecutaste?. **aws elb delete-load-balancer --load-balancer-name kimberly-salazar-elb** 

![](Aspose.Words.183a722b-c0af-4236-be81-5f35a62d73a9.060.png)

