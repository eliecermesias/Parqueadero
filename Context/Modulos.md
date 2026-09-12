# División de módulos y estimación

## 1. Criterios de estimación

Las estimaciones son preliminares y están expresadas en **días-persona** de ocho horas. Incluyen desarrollo, pruebas básicas y documentación técnica del módulo, pero no incluyen la compra o instalación de hardware de posicionamiento.

Se asume:

- una persona desarrolladora con experiencia básica o intermedia en Python y Django;
- un parqueadero inicial, con un mapa disponible o que pueda levantarse durante el proyecto;
- aplicación web responsive como primera interfaz;
- MariaDB como base de datos;
- integración con GPS del dispositivo antes de incorporar tecnologías interiores;
- requisitos estables y participación periódica de una persona responsable del negocio.

Las estimaciones no son compromisos de calendario. Un día-persona no equivale necesariamente a un día calendario cuando existen dependencias, revisiones o trabajo en paralelo.

## 2. Resumen de módulos

| ID | Módulo | Prioridad | Estimación |
| --- | --- | --- | ---: |
| M01 | Base técnica y configuración | Imprescindible | 5-7 días |
| M02 | Usuarios, autenticación y permisos | Imprescindible | 5-7 días |
| M03 | Parqueaderos, niveles y espacios | Imprescindible | 8-12 días |
| M04 | Vehículos y perfiles | Imprescindible | 4-6 días |
| M05 | Registro de estacionamiento | Imprescindible | 8-12 días |
| M06 | Captura y calidad de ubicación | Imprescindible inicial | 10-15 días |
| M07 | Alertas y notificaciones | Imprescindible inicial | 5-8 días |
| M08 | Mapa y red de circulación | Alta | 12-18 días |
| M09 | Cálculo de rutas peatonales | Alta | 15-25 días |
| M10 | Navegación y seguimiento | Alta | 10-16 días |
| M11 | Historial y reportes operativos | Media | 7-12 días |
| M12 | Administración del mapa y operación | Alta | 8-12 días |
| M13 | Seguridad, auditoría y privacidad | Imprescindible | 7-10 días |
| M14 | Pruebas, despliegue y observabilidad | Imprescindible | 10-15 días |
| M15 | Análisis de datos | Futuro | 15-25 días |
| M16 | Automatizaciones y tareas asíncronas | Futuro | 10-18 días |

## 3. Descripción de cada módulo

### M01. Base técnica y configuración

**Objetivo:** preparar el proyecto Django, los ambientes y la conexión con MariaDB.

**Incluye:**

- estructura del proyecto y aplicaciones Django;
- configuración de desarrollo, pruebas y producción;
- variables de entorno y archivo `.env.example`;
- conexión a MariaDB, migraciones y datos iniciales;
- configuración de archivos estáticos y media;
- formato, revisión básica y documentación para ejecutar el proyecto.

**Dependencias:** ninguna.

**Estimación:** 5-7 días-persona.

### M02. Usuarios, autenticación y permisos

**Objetivo:** controlar el acceso a la aplicación y proteger los datos de ubicación.

**Incluye:**

- registro, inicio y cierre de sesión;
- recuperación y cambio de contraseña;
- perfil del usuario;
- roles de usuario y administrador;
- permisos para consultar, corregir y eliminar ubicaciones;
- configuración de consentimiento para ubicación.

**Dependencias:** M01.

**Estimación:** 5-7 días-persona.

### M03. Parqueaderos, niveles y espacios

**Objetivo:** representar la estructura física del parqueadero.

**Incluye:**

- parqueaderos y sedes;
- niveles, zonas y espacios;
- capacidad y estado de los espacios;
- accesos, salidas y puntos de referencia;
- configuración de horarios y restricciones;
- relaciones con coordenadas geográficas o geometrías del mapa.

**Dependencias:** M01, M02.

**Estimación:** 8-12 días-persona.

### M04. Vehículos y perfiles

**Objetivo:** asociar uno o varios vehículos con cada usuario.

**Incluye:**

- placa o identificador del vehículo;
- marca, modelo, color y fotografía opcionales;
- vehículo principal y vehículos adicionales;
- estado activo o archivado;
- validación de acceso a la información del vehículo.

**Dependencias:** M02.

**Estimación:** 4-6 días-persona.

### M05. Registro de estacionamiento

**Objetivo:** guardar y consultar el evento en que un vehículo queda estacionado.

**Incluye:**

- creación manual y automática de un registro;
- asociación con usuario, vehículo, parqueadero, nivel y espacio;
- fecha, hora, estado y observaciones;
- corrección, confirmación y cierre del registro;
- historial de estacionamientos;
- transacciones para evitar registros incompletos o duplicados.

**Dependencias:** M02, M03, M04, M06.

**Estimación:** 8-12 días-persona.

### M06. Captura y calidad de ubicación

**Objetivo:** obtener la mejor ubicación disponible y comunicar su nivel de precisión.

**Incluye:**

- captura de latitud, longitud y hora;
- cálculo y almacenamiento de precisión estimada;
- selección manual del punto en el mapa;
- detección de parqueadero, zona y nivel cuando existan datos suficientes;
- diferenciación entre GPS, Wi-Fi, Bluetooth, UWB, QR o captura manual;
- validación de ubicación insuficiente;
- soporte inicial para GPS y preparación para proveedores de posicionamiento interior.

**Dependencias:** M01, M03, M04.

**Estimación:** 10-15 días-persona para GPS y selección manual. La integración con balizas, Wi-Fi o UWB debe estimarse aparte después de conocer el hardware y el proveedor.

### M07. Alertas y notificaciones

**Objetivo:** informar al usuario cuando se detecte o confirme un estacionamiento.

**Incluye:**

- detección de velocidad baja y permanencia mínima;
- reglas para evitar alertas por semáforos o detenciones breves;
- alerta inmediata al guardar la ubicación;
- confirmación cuando la confianza sea media;
- notificaciones de precisión insuficiente;
- recordatorios configurables;
- control de duplicados durante una misma detención.

**Dependencias:** M05, M06.

**Estimación:** 5-8 días-persona para alertas dentro de la aplicación. Las notificaciones móviles en segundo plano o push pueden requerir 5-10 días adicionales, según la plataforma.

### M08. Mapa y red de circulación

**Objetivo:** representar los espacios transitables y los obstáculos del entorno.

**Incluye:**

- mapa georreferenciado;
- pasillos, cruces y accesos;
- obstáculos: edificaciones, lagos, ríos, muros y zonas restringidas;
- niveles y conexiones verticales;
- rampas, escaleras, ascensores y puentes;
- estado disponible o cerrado de cada elemento;
- importación o carga de geometrías, si se define un formato.

**Dependencias:** M03, M12.

**Estimación:** 12-18 días-persona para un mapa inicial. El levantamiento de un parqueadero complejo puede agregar 5-15 días-persona.

### M09. Cálculo de rutas peatonales

**Objetivo:** encontrar una ruta realista entre la posición del usuario y el vehículo.

**Incluye:**

- construcción de la red de nodos y segmentos;
- búsqueda de la ruta más corta o rápida;
- exclusión de obstáculos y accesos cerrados;
- cambios de nivel mediante escaleras, rampas, ascensores o puentes;
- preferencia de accesibilidad;
- rutas que eviten escaleras o prefieran rampas y ascensores;
- respuesta cuando no exista una ruta válida.

**Dependencias:** M03, M06, M08.

**Estimación:** 15-25 días-persona para una primera versión basada en un grafo. La navegación avanzada, mapas de alta precisión o rutas dinámicas pueden requerir una estimación adicional.

### M10. Navegación y seguimiento

**Objetivo:** guiar al usuario hasta el vehículo y recalcular la ruta durante el recorrido.

**Incluye:**

- posición actual y destino en el mapa;
- instrucciones por tramos;
- indicador de nivel y conexión vertical;
- detección de desvío;
- recalculo de ruta;
- confirmación de vehículo encontrado;
- manejo de pérdida de señal o baja precisión.

**Dependencias:** M06, M08, M09.

**Estimación:** 10-16 días-persona.

### M11. Historial y reportes operativos

**Objetivo:** permitir consultar la operación histórica del parqueadero.

**Incluye:**

- historial de estacionamientos por usuario y vehículo;
- consultas por fecha, sede, nivel y zona;
- exportación CSV inicial;
- conteos de entradas, salidas y estacionamientos activos;
- reporte básico de ocupación e ingresos, si existen datos de pago.

**Dependencias:** M05, M03.

**Estimación:** 7-12 días-persona.

### M12. Administración del mapa y operación

**Objetivo:** permitir que una persona autorizada mantenga vigente la información del parqueadero.

**Incluye:**

- administración de sedes, niveles y zonas;
- alta y edición de pasillos y conexiones;
- creación de obstáculos y zonas restringidas;
- cierre temporal de accesos;
- actualización del estado de espacios;
- importación de datos del mapa cuando se defina el formato.

**Dependencias:** M02, M03, M08.

**Estimación:** 8-12 días-persona.

### M13. Seguridad, auditoría y privacidad

**Objetivo:** proteger la información personal y los registros de ubicación.

**Incluye:**

- control de acceso por objeto y por rol;
- registro de cambios importantes;
- política de retención y eliminación del historial;
- protección de sesiones y formularios;
- manejo de secretos fuera del repositorio;
- registro de consentimientos y permisos;
- revisión de exposición de coordenadas en API y administración.

**Dependencias:** M02, M05, M06.

**Estimación:** 7-10 días-persona, distribuidos parcialmente durante el desarrollo de los demás módulos.

### M14. Pruebas, despliegue y observabilidad

**Objetivo:** hacer que el sistema sea verificable, desplegable y operable.

**Incluye:**

- pruebas unitarias de tarifas, detección y rutas;
- pruebas de integración con MariaDB;
- pruebas de permisos y privacidad;
- revisión de errores de ubicación y rutas sin salida;
- configuración de despliegue;
- copias de seguridad;
- registro de errores y métricas básicas;
- documentación de recuperación.

**Dependencias:** todos los módulos del MVP.

**Estimación:** 10-15 días-persona, además del tiempo de pruebas incluido en cada módulo.

### M15. Análisis de datos

**Objetivo:** convertir los datos históricos en indicadores para mejorar la operación.

**Incluye:**

- extracción segura de datos sin afectar la operación;
- limpieza y transformación;
- ocupación por hora, día, nivel y zona;
- tiempo promedio de permanencia;
- rotación de espacios;
- ingresos y comportamiento de usuarios, si existe el módulo de pagos;
- exportación para Pandas/Jupyter;
- tableros o API para consumo externo.

**Tecnología inicial propuesta:** Pandas y JupyterLab para exploración. Posteriormente se podrá evaluar Metabase, Superset, Polars u otra herramienta según el volumen y el tipo de usuarios.

**Dependencias:** M05, M11, M14.

**Estimación:** 15-25 días-persona para indicadores y exportaciones iniciales. Un sistema de tableros productivo puede agregar 10-20 días-persona.

### M16. Automatizaciones y tareas asíncronas

**Objetivo:** ejecutar procesos en segundo plano o en horarios programados sin bloquear la aplicación web.

**Incluye:**

- reportes programados;
- alertas de ocupación o eventos anómalos;
- limpieza de datos temporales;
- importaciones y exportaciones;
- reintentos y control de fallos;
- historial de ejecuciones;
- monitoreo de tareas.

**Tecnología inicial propuesta:** comenzar con Celery y Redis cuando exista la primera tarea asíncrona real. Evaluar Prefect si los procesos se convierten en flujos de datos complejos. Apache Airflow se reserva para una escala mayor.

**Dependencias:** M05, M07, M11, M14.

**Estimación:** 10-18 días-persona para colas, tareas y programación inicial. Los flujos de datos complejos deben estimarse como proyectos independientes.

## 4. Alcance recomendado por versión

### Versión 1: núcleo operativo

Debe incluir M01, M02, M03, M04, M05, M06 en su versión GPS/manual, M07, M11 básico, M13 y M14.

**Estimación aproximada:** 69-101 días-persona.

Esta versión permite registrar dónde quedó el vehículo, mostrar la última ubicación, generar alertas y conservar datos históricos. Todavía puede depender de selección manual en interiores.

### Versión 2: mapas y rutas

Debe añadir M08, M09, M10 y M12.

**Estimación adicional aproximada:** 45-71 días-persona.

Esta versión permite navegar por pasillos y niveles, evitando obstáculos y utilizando ascensores, escaleras, rampas o puentes.

### Versión 3: precisión interior

Debe ampliar M06 y probar Bluetooth, Wi-Fi, UWB, QR o NFC según el parqueadero.

**Estimación adicional:** 15-40 días-persona de software, sin incluir compra, instalación, calibración ni mantenimiento del hardware.

### Versión 4: análisis y automatización

Debe incorporar M15 y M16 después de contar con datos operativos confiables.

**Estimación adicional aproximada:** 25-43 días-persona para una primera versión.

## 5. Dependencias principales

```text
M01 -> M02 -> M03 -> M04
									|      \
									v       v
								M06 -> M05 -> M07
									|      \
									v       v
								M08 -> M09 -> M10
									|
									v
								M12

M05 + M11 -> M15
M05 + M07 -> M16
M02 + M05 + M06 -> M13
Todos los módulos -> M14
```

## 6. Riesgos que pueden modificar la estimación

- La precisión de metros en interiores requiere infraestructura física o una fuente de posicionamiento adecuada.
- Crear el mapa de pasillos y obstáculos puede ser más costoso que programar la interfaz.
- Los ascensores requieren conocer su disponibilidad y las reglas para usarlos.
- Los mapas multinivel necesitan una referencia vertical además de latitud y longitud.
- Las notificaciones en segundo plano dependen de la plataforma móvil y sus permisos.
- La navegación en tiempo real puede requerir una aplicación móvil, no solo una aplicación web.
- Cambios frecuentes en el diseño del parqueadero obligan a mantener el mapa y recalcular la red.
- El volumen de datos definirá si MariaDB es suficiente para análisis directo o si conviene una réplica, almacén analítico o proceso de exportación.

## 7. Recomendación de planificación

Se recomienda implementar primero la Versión 1, validar el registro del estacionamiento en un entorno real y medir la precisión disponible. Después se debe levantar el mapa de un parqueadero concreto y construir la Versión 2 sobre ese caso real.

No se recomienda iniciar simultáneamente el desarrollo de precisión interior, tableros avanzados y automatizaciones. Esas capacidades dependen de datos, infraestructura y flujos operativos que todavía deben validarse.
