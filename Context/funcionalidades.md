# Especificación funcional: localización del vehículo y navegación

## 1. Objetivo

La aplicación permitirá al usuario guardar automáticamente el lugar donde dejó estacionado su vehículo y encontrarlo posteriormente mediante un mapa y una ruta peatonal.

La solución debe funcionar tanto en espacios abiertos como en parqueaderos cubiertos, multinivel o con circulación restringida. Por esta razón, no debe asumir que el usuario puede desplazarse en línea recta entre su posición y el vehículo.

## 2. Alcance de la funcionalidad

La aplicación debe:

- detectar que el vehículo se ha detenido y que probablemente terminó el estacionamiento;
- registrar el punto de estacionamiento con fecha, hora y precisión estimada;
- mostrar al usuario una alerta confirmando dónde quedó el vehículo;
- permitir consultar el último lugar guardado;
- mostrar la posición del usuario y del vehículo en un mapa;
- calcular una ruta por pasillos y zonas transitables;
- evitar obstáculos como edificaciones, lagos, ríos y zonas no habilitadas;
- considerar puentes, ascensores, escaleras, rampas y cambios de nivel;
- recalcular la ruta cuando el usuario se desvíe;
- informar cuando la ubicación no tenga precisión suficiente.

## 3. Actores

### Usuario

Persona que estaciona el vehículo y posteriormente necesita encontrarlo.

### Sistema de localización

Componente que obtiene la posición del usuario o del vehículo mediante GPS, sensores del dispositivo y, cuando estén disponibles, tecnologías de posicionamiento interior.

### Administrador del parqueadero

Persona que configura el mapa, los niveles, los espacios, los pasillos, los accesos y las zonas que no son transitables.

## 4. Registro del lugar de estacionamiento

### 4.1 Detección de detención

La aplicación debe observar los cambios de movimiento del dispositivo o del vehículo y generar un evento de posible estacionamiento cuando se cumplan condiciones como:

- velocidad inferior al umbral configurado;
- permanencia en una zona cercana durante un tiempo mínimo;
- cambio de estado del vehículo, cuando exista integración con Bluetooth, dispositivo del vehículo o aplicación complementaria;
- desconexión del dispositivo del vehículo, cuando esa señal esté disponible.

Una detención breve en un semáforo, una fila o una congestión no debe registrarse automáticamente como estacionamiento. Para evitar falsos positivos, la aplicación debe usar una combinación de velocidad, tiempo detenido, geocerca y confirmación del usuario cuando la certeza no sea suficiente.

### 4.2 Confirmación automática y manual

Cuando la probabilidad de estacionamiento sea alta, el sistema guardará el punto automáticamente y mostrará una alerta.

Cuando la probabilidad sea media, mostrará una notificación como:

> ¿Estacionaste aquí? Guardar ubicación del vehículo

El usuario podrá guardar, corregir o descartar el punto manualmente. También podrá cambiar la ubicación arrastrando el marcador en el mapa o seleccionando un espacio, nivel o zona.

### 4.3 Datos almacenados

Cada registro de estacionamiento debe incluir:

- identificador del usuario;
- vehículo asociado, si el usuario tiene más de uno;
- coordenadas de latitud y longitud;
- nivel, zona, espacio o referencia del parqueadero, si se conocen;
- fecha y hora de registro;
- fuente de la ubicación: GPS, Wi-Fi, Bluetooth, UWB, selección manual u otra;
- precisión estimada en metros;
- orientación del dispositivo, si está disponible;
- nivel de confianza del registro;
- notas, fotografía o referencia opcional del usuario;
- estado del registro: activo, corregido, archivado o eliminado.

El historial debe conservar los registros anteriores para que el usuario pueda consultar dónde dejó el vehículo en días previos, sujeto a la política de privacidad definida por la aplicación.

## 5. Precisión de la ubicación

La aplicación debe mostrar la precisión estimada de cada ubicación y no presentar como exacta una posición que tenga incertidumbre.

### 5.1 Objetivo de precisión

El objetivo funcional será una precisión de hasta unos pocos metros cuando las condiciones y la tecnología disponible lo permitan. La precisión real dependerá del entorno:

- en espacios abiertos: GPS o GNSS puede ser suficiente en condiciones favorables;
- en parqueaderos cubiertos: la señal GPS puede ser débil, reflejada o inexistente;
- en estructuras grandes: se pueden requerir Bluetooth Low Energy, Wi-Fi, UWB, sensores del dispositivo, códigos QR, puntos de referencia o selección manual;
- en varios niveles: las coordenadas horizontales no bastan; también debe identificarse el nivel o la cota.

La interfaz debe mostrar un círculo de incertidumbre o una etiqueta de precisión, por ejemplo, “precisión aproximada: 8 m”. Si la precisión supera el límite configurado, el sistema debe pedir confirmación o una referencia adicional.

### 5.2 Mejora de la ubicación en interiores

El sistema debe admitir progresivamente estas fuentes:

- GPS/GNSS para espacios abiertos;
- geocercas para detectar la entrada y salida del parqueadero;
- Bluetooth o balizas instaladas en zonas, niveles o pasillos;
- Wi-Fi del establecimiento, si la infraestructura lo permite;
- UWB para una precisión más alta cuando exista hardware compatible;
- códigos QR, NFC o señalización física para confirmar el nivel y la zona;
- selección manual del espacio o captura de una fotografía.

La fuente y la calidad de la medición deben quedar registradas para poder mejorar el sistema posteriormente.

## 6. Mapa del parqueadero y del entorno

El sistema debe contar con un mapa georreferenciado compuesto por elementos transitables y no transitables.

### 6.1 Elementos transitables

- pasillos peatonales;
- rampas;
- escaleras;
- ascensores;
- puentes y pasos elevados;
- accesos y salidas;
- zonas de conexión entre niveles;
- entradas de edificios o establecimientos.

### 6.2 Obstáculos y restricciones

- edificaciones;
- lagos, ríos y cuerpos de agua;
- muros, cerramientos y zonas privadas;
- vías sin cruce peatonal;
- áreas de circulación vehicular restringida;
- espacios temporales bloqueados;
- zonas sin acceso para personas con movilidad reducida;
- ascensores o pasillos cerrados temporalmente.

Cada obstáculo debe tener geometría, nivel, estado y, cuando aplique, horarios de disponibilidad. Un obstáculo no debe ser solamente una imagen en el mapa: debe influir en el cálculo de la ruta.

## 7. Cálculo de rutas peatonales

### 7.1 Ruta basada en una red de pasillos

La ruta se calculará sobre una red de nodos y segmentos:

- los nodos representan cruces, accesos, espacios, ascensores, escaleras, rampas y cambios de nivel;
- los segmentos representan pasillos o conexiones transitables;
- cada segmento tendrá distancia, nivel, dirección permitida, accesibilidad y estado;
- los obstáculos no transitables no se incluirán en la red o tendrán un costo que impida atravesarlos.

La distancia en línea recta se podrá mostrar como referencia, pero nunca deberá usarse como ruta final cuando existan obstáculos o varios niveles.

### 7.2 Preferencias de navegación

El usuario podrá seleccionar, según la información disponible:

- ruta más corta;
- ruta más rápida;
- ruta accesible para silla de ruedas o movilidad reducida;
- evitar escaleras;
- preferir ascensores o rampas;
- evitar zonas descubiertas;
- evitar zonas con acceso restringido.

Si una preferencia no puede cumplirse, el sistema debe informarlo y ofrecer la mejor alternativa disponible.

### 7.3 Instrucciones y recalculo

La aplicación debe mostrar instrucciones por etapas, por ejemplo:

1. Avanza hasta el pasillo principal.
2. Gira a la izquierda en el cruce de la zona B.
3. Toma el ascensor hasta el nivel 2.
4. Continúa hasta el espacio de estacionamiento.

Durante el recorrido, el sistema debe comparar la posición actual con la ruta. Si el usuario se desvía, pierde la señal o encuentra un acceso cerrado, debe recalcular la ruta o solicitar una confirmación.

## 8. Alertas

### 8.1 Alerta al detenerse

Cuando el sistema detecte un posible estacionamiento, debe generar una alerta inmediata, siempre que se cumplan las reglas de detección configuradas.

La alerta debe indicar:

- que la ubicación fue guardada o necesita confirmación;
- hora del registro;
- nivel, zona o espacio, si se conoce;
- precisión estimada;
- acción para abrir el mapa;
- acción para corregir o descartar el registro.

Ejemplo:

> Ubicación del vehículo guardada. Nivel 2, zona B. Precisión aproximada: 6 m.

La alerta debe evitar duplicados durante una misma detención y no debe interrumpir repetidamente al usuario.

### 8.2 Recordatorio posterior

El usuario podrá solicitar un recordatorio después de un tiempo configurable. El sistema también podrá mostrar el lugar guardado al detectar que el usuario se aleja del parqueadero o vuelve a acercarse a él.

### 8.3 Alertas de calidad

La aplicación debe advertir cuando:

- la ubicación tenga una precisión insuficiente;
- no se pueda determinar el nivel;
- el mapa interior no esté disponible;
- el parqueadero no tenga una red de pasillos configurada;
- la ruta dependa de un acceso temporalmente cerrado.

## 9. Flujo principal de uso

### Estacionar el vehículo

1. El usuario llega a una zona habilitada.
2. El sistema detecta una reducción de velocidad y una detención prolongada.
3. Obtiene la mejor ubicación disponible y calcula su precisión.
4. Determina el parqueadero, nivel, zona o espacio cuando sea posible.
5. Guarda el registro y genera la alerta.
6. El usuario confirma o corrige el punto si la confianza es baja.

### Encontrar el vehículo

1. El usuario abre “Encontrar mi vehículo”.
2. La aplicación muestra la última ubicación guardada y su precisión.
3. El usuario selecciona el punto de inicio o permite obtener su ubicación actual.
4. El sistema calcula una ruta por pasillos y conexiones transitables.
5. La aplicación muestra instrucciones y actualiza la ruta mientras el usuario camina.
6. El usuario puede marcar el vehículo como encontrado y cerrar el registro activo.

## 10. Requisitos de privacidad y permisos

- Solicitar permisos de ubicación explicando su finalidad.
- Permitir al usuario activar o desactivar la detección en segundo plano.
- Informar cuándo la aplicación está usando ubicación en segundo plano.
- Proteger los registros de ubicación para que solo sean visibles para el usuario autorizado.
- Permitir eliminar el historial de ubicaciones.
- No compartir ubicaciones con terceros sin consentimiento.
- Reducir la recolección de datos cuando no sea necesaria para la funcionalidad.

## 11. Criterios de aceptación iniciales

- El sistema puede guardar manualmente una ubicación y mostrarla en un mapa.
- El sistema registra fecha, hora, fuente y precisión estimada.
- Una detención breve no genera automáticamente un nuevo estacionamiento.
- Una detención que cumple las condiciones genera una sola alerta.
- El usuario puede corregir o descartar una ubicación detectada.
- Una ruta no atraviesa obstáculos configurados.
- Una ruta puede incluir cambios de nivel mediante escaleras, rampas, ascensores o puentes.
- El sistema recalcula la ruta cuando el usuario se desvía.
- El sistema informa cuando la precisión o el mapa no son suficientes.
- Los permisos y registros de ubicación respetan la configuración de privacidad del usuario.

## 12. Evolución recomendada

La primera versión puede comenzar con ubicación manual, GPS, una alerta al guardar y un mapa sencillo. Después se pueden incorporar detección automática, mapas interiores, balizas, rutas multinivel y navegación en tiempo real.

La precisión de metros y la navegación interior deben probarse en parqueaderos reales antes de considerarse garantizadas. La aplicación debe conservar siempre una alternativa manual, como seleccionar el espacio, escanear un código QR o agregar una nota y fotografía.
