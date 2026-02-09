# SCRIP DE PRUEBAS DE CARGA
## PRUEBAS DE RENDIMIENTO UTILIZANDO LA HERRAMIENTA JMETER
### Autor: Michal Pesantez
## Prerequisitos
- Maquina Local con sistema operativo Windows 11 64 bits

<img width="160" height="28" alt="Image" src="https://github.com/user-attachments/assets/ebc61342-7d30-496e-a321-897c635606f2" />

- Java 17 configurado en las variables de entorno de la maquina

<img width="100" height="100" alt="Image" src="https://github.com/user-attachments/assets/42803071-ff01-4126-be0a-5deeaf4dc73f" />

- JMeter descargado desde la pagina oficial

<img width="100" height="100" alt="Image" src="https://github.com/user-attachments/assets/637fbc26-9925-4d5d-8ce6-dfc32704549c" />

# Configuracion de herramienta
- Agregar un `Thread Group`
- Dirigirse a la opcion `Tools` -> `Import from cURL` y colocar el sigueinte curl, con esto la solicitud HTTP y las cabeceras se generan automaticamente.
> curl -X POST 'https://fakestoreapi.com/auth/login' -H 'Content-Type: application/json' -d '{"username": "john_doe", "password": "pass123"}'
- Reemplazar las cadenas de texto `"jhon_doe"` y `"pass123"` por las etiquetas `"${user}"` y `"${passwd}"` para indicarle a JMeter que esas seran las variables que se tomaran del archivo `users.csv`
- Agregar un `CSV Data Set Config` con la siguiente configuracion:
  - Filename: ruta del archivo `users.csv`
  - File encoding: `UTF-8`
  - Variable Names (comma-delimited): `user,passwd` indicandole a JMeter que estas seran las varibles definidas en el archivo
  - Ignore first line: &#9989;  
- Agregar un `Constant Throughput Timer` para poder asegurarnos que se puedan enviar por lo menos 20TPS, se debe comfigurar en 1200.0
- Agregar `Duration Assertion` y connfigurarlo en 1500, esto con la finalidad de que todas las peticiones que demoren un tiempo mayor a 1.5 segundos se marquen como ERROR;
- Agregar los siguientes reportes para poder revisar el comportamiento del rendimiento de las solicitudes:
  - `View Results Tree` ver el resultado de las peticiones, asi como toda la informacion recopilada en cada una de ellas
  - `Agregate Report` ver en terminos generales como se esta comportando el rendimiento de las peticiones enviadas
  - `View Results in Table` ver los resultados de cada peticion con la informacion relevante, dandonos la posibilidad de exportar esta informacion a un archivo csv
  - `Summary Report` ver informacion consolidada de las peticiones realizadas
- Una vez ejecutadas las pruebas se pueden generar reportes desde la propia herramienta JMeter, para lo cual nos dirigimos a la pestana `Tools` -> `Generate HTML Reports`, para poder generar estos reportes se deben configurar los resultados mencionados anteriormente.

# CONCLUSIONES

## Resumen

Una vez ejecutadas las pruebas se pudo determinar que se llego a alcanzar los 20TPS, que ninguna solicitud supera el umbral de los 1.5 segundos de respuesta, adicional la tasa de error es del 0.10%.
Con base en estos datos obtenidos, se puede concluir que el sistema es altamente estable y cumple satisfactoriamente con todos los Acuerdos de Nivel de Servicio (SLA) definidos:
### Cumplimiento de Objetivos (KPIs)
- **Capacidad de Procesamiento (Throughput):** Se logró alcanzar y mantener el objetivo de 20 TPS, demostrando que la infraestructura soporta la carga transaccional requerida.
- **Tiempos de Respuesta:** El sistema mostró un desempeño óptimo, ya que ninguna solicitud superó el umbral de 1.5 segundos. Esto garantiza una experiencia de usuario fluida y rápida siempre y cuando no se superen las condiciones de carga probadas.
- **Calidad del Servicio (Tasa de Error):** Se registró una tasa de error de apenas el 0.10%, lo cual es menor al límite máximo permitido del 3%.

# REPORTES HTML GENERADOS DESDE JMETER

<img width="1910" height="915" alt="Image" src="https://github.com/user-attachments/assets/1e7039e3-af0a-4e63-bed0-b906aa32fbb1" />

<img width="1892" height="860" alt="Image" src="https://github.com/user-attachments/assets/652a1789-eea8-4e25-8e92-f77a3b4b177b" />

<img width="1910" height="915" alt="Image" src="https://github.com/user-attachments/assets/c643b1ae-dc02-4e42-b890-4df61947b3a8" />

