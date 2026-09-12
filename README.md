<div align="center">

# Parqueadero

**Sistema web para la gestión inteligente de parqueaderos, localización de vehículos y navegación peatonal.**

![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.2_LTS-092E20?style=for-the-badge&logo=django&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-10.11+-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Estado](https://img.shields.io/badge/Estado-En%20planeaci%C3%B3n%20e%20inicio-blue?style=for-the-badge)

</div>

---

## Presentación

**Parqueadero** es un proyecto orientado a construir una plataforma web para administrar la operación de un parqueadero y, progresivamente, ayudar a los usuarios a recordar dónde dejaron su vehículo y encontrarlo mediante mapas, rutas peatonales y alertas inteligentes.

La idea central es partir de un núcleo operativo confiable: vehículos, usuarios, espacios, entradas, salidas, pagos, historial y auditoría. Sobre esa base, el sistema podrá crecer hacia funcionalidades más avanzadas como análisis de ocupación, reportes, navegación dentro de parqueaderos cubiertos, automatizaciones e integración mediante API.

> El repositorio se encuentra en una etapa inicial. La carpeta `Context/` contiene la especificación funcional, técnica y modular que guía la construcción del sistema.

---

## Objetivo Del Proyecto

Crear una aplicación web capaz de:

- gestionar parqueaderos, zonas, niveles y espacios físicos;
- registrar vehículos, usuarios y operaciones de estacionamiento;
- controlar entradas, salidas, tarifas y pagos;
- conservar historial operativo para análisis posterior;
- ubicar el vehículo estacionado con fecha, hora, fuente y precisión;
- calcular rutas peatonales evitando obstáculos y accesos restringidos;
- preparar el sistema para reportes, indicadores, API y automatizaciones.

---

## Funcionalidades Planeadas

| Área | Descripción |
| --- | --- |
| Gestión operativa | Registro de vehículos, clientes, entradas, salidas, pagos y cierres. |
| Espacios físicos | Administración de sedes, niveles, zonas, cupos, accesos y restricciones. |
| Localización | Captura manual o automática de la ubicación del vehículo estacionado. |
| Navegación | Rutas peatonales por pasillos, rampas, escaleras, ascensores y puentes. |
| Alertas | Confirmación de estacionamiento, precisión insuficiente y recordatorios. |
| Historial | Consulta de operaciones pasadas y conservación de datos relevantes. |
| Seguridad | Autenticación, roles, permisos, privacidad y auditoría de cambios. |
| Reportes | Indicadores de ocupación, ingresos, permanencia y comportamiento. |
| API futura | Exposición de datos y operaciones a otros sistemas o aplicaciones. |

---

## Visión Técnica

El sistema está pensado como una aplicación web modular construida con Django y MariaDB.

| Componente | Tecnología prevista |
| --- | --- |
| Lenguaje | Python 3.12 o superior |
| Framework web | Django 5.2 LTS |
| Base de datos | MariaDB 10.11 LTS o superior |
| Driver de base de datos | `mysqlclient` |
| Configuración | Variables de entorno con `.env` |
| API futura | Django REST Framework |
| Pruebas | `pytest`, `pytest-django`, `coverage` |
| Producción | Gunicorn detrás de Nginx |

---

## Arquitectura Modular Propuesta

```text
parqueadero/
├── manage.py
├── config/                 # Configuración, URLs, WSGI y ASGI
├── usuarios/               # Autenticación, roles y permisos
├── parqueaderos/           # Sedes, niveles, zonas y espacios
├── operaciones/            # Entradas, salidas, vehículos y tarifas
├── pagos/                  # Cobros, medios de pago y cierres
├── reportes/               # Consultas e indicadores operativos
├── common/                 # Utilidades compartidas
├── templates/
├── static/
├── media/
├── tests/
├── .env.example
├── requirements.txt
└── README.md
```

Esta estructura puede ajustarse conforme avancen los casos de uso. La prioridad será mantener módulos pequeños, claros y con responsabilidades bien definidas.

---

## Localización Del Vehículo

Una de las funcionalidades diferenciales del proyecto será permitir que el usuario guarde el lugar donde dejó estacionado su vehículo y lo encuentre después.

El sistema deberá considerar:

- ubicación por GPS/GNSS en espacios abiertos;
- selección manual del punto, zona, nivel o espacio;
- precisión estimada en metros;
- historial de ubicaciones;
- confirmación cuando la confianza sea media o baja;
- mapas interiores y obstáculos;
- rutas reales por zonas transitables;
- cambios de nivel mediante escaleras, rampas, ascensores o puentes;
- recalculo de ruta cuando el usuario se desvíe.

La aplicación no asumirá que el usuario puede caminar en línea recta hasta el vehículo. La ruta deberá respetar pasillos, accesos, obstáculos y restricciones reales del entorno.

---

## Roadmap

| Fase | Enfoque |
| --- | --- |
| 1 | Base técnica del proyecto Django, configuración y conexión con MariaDB. |
| 2 | Usuarios, roles, permisos y seguridad inicial. |
| 3 | Modelado de parqueaderos, niveles, zonas y espacios. |
| 4 | Registro de vehículos y operaciones de estacionamiento. |
| 5 | Entradas, salidas, tarifas, pagos e historial. |
| 6 | Ubicación manual/GPS del vehículo y alertas básicas. |
| 7 | Mapa, red de circulación y rutas peatonales. |
| 8 | Reportes operativos, indicadores y exportaciones. |
| 9 | API, automatizaciones y análisis avanzado de datos. |

---

## Instalación Local

Cuando el proyecto Django esté inicializado, el flujo esperado será:

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py test
python manage.py runserver
```

En Windows:

```bash
venv\Scripts\activate
```

Variables mínimas esperadas en `.env`:

```env
DJANGO_DEBUG=True
DJANGO_SECRET_KEY=cambiar-en-cada-ambiente
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1
DB_NAME=parqueadero
DB_USER=parqueadero_app
DB_PASSWORD=definir-localmente
DB_HOST=127.0.0.1
DB_PORT=3306
TIME_ZONE=America/Bogota
```

---

## Estado Actual Del Repositorio

Actualmente el repositorio contiene:

```text
.
├── Context/
│   ├── Colaboradores.md
│   ├── Entorno.md
│   ├── Modulos.md
│   └── funcionalidades.md
├── main.py
└── README.md
```

La documentación en `Context/` define la dirección del proyecto. El siguiente paso natural es inicializar la aplicación Django, fijar dependencias y crear la primera estructura de módulos.

---

## Principios De Diseño

- Construir primero un núcleo transaccional confiable.
- Mantener datos históricos para análisis posterior.
- Separar responsabilidades por módulos.
- Proteger información personal y datos de ubicación.
- Configurar tarifas y horarios sin modificar código.
- Evitar guardar secretos dentro del repositorio.
- Escribir pruebas para reglas críticas: tarifas, pagos, entradas, salidas, permisos y transacciones.

---

## Documentación Del Proyecto

| Archivo | Contenido |
| --- | --- |
| [`Context/Colaboradores.md`](Context/Colaboradores.md) | Lista de integrantes del proyecto y correos institucionales. |
| [`Context/Entorno.md`](Context/Entorno.md) | Entorno técnico, arquitectura esperada, base de datos, seguridad y flujo de desarrollo. |
| [`Context/Modulos.md`](Context/Modulos.md) | División por módulos, prioridades, dependencias y estimaciones. |
| [`Context/funcionalidades.md`](Context/funcionalidades.md) | Especificación funcional de localización del vehículo y navegación. |

---

## Colaboradores

La lista oficial de integrantes del proyecto se mantiene en [`Context/Colaboradores.md`](Context/Colaboradores.md).

| Nombre | Rol | Correo institucional |
| --- | --- | --- |
| Alejandro Salazar Tovar | Colaborador | asalazart1@correo.usbcali.edu.co |
| Camilo Piedrahita Velasquez | Colaborador | cpiedrahitav@correo.usbcali.edu.co |
| Cristian Camilo Colorado Posada | Colaborador | cccoloradop@correo.usbcali.edu.co |
| Fernanda Elizabeth Munoz Chaves | Colaborador | femunozc@correo.usbcali.edu.co |
| Jacobo Bejarano Gomez | Colaborador | jbejaranog@correo.usbcali.edu.co |
| Jhon Ricky Chito Melo | Colaborador | jrchitom@correo.usbcali.edu.co |
| Jose Esteban Otero Rada | Colaborador | jeoteror@correo.usbcali.edu.co |
| Juan Esteban Gutierrez Ceballos | Colaborador | jegutierrezc@correo.usbcali.edu.co |
| Juan Esteban Salazar Barco | Colaborador | jesalazarb1@correo.usbcali.edu.co |
| Juan Felipe Rodriguez Castro | Colaborador | jfrodriguezc2@correo.usbcali.edu.co |
| Juan Jose Patino Perez | Colaborador | jjpatinop@correo.usbcali.edu.co |
| Juan Manuel Ramos Mosquera | Colaborador | jmramosm1@correo.usbcali.edu.co |
| Juan Sebastian Moran Ordonez | Colaborador | jsmorano@correo.usbcali.edu.co |
| Miguel Angel Rodriguez Bravo | Colaborador | marodriguezb2@correo.usbcali.edu.co |
| Nicolas Navarro Jaramillo | Colaborador | nnavarroj@correo.usbcali.edu.co |
| Samuel Mejia Chavarriaga | Colaborador | smejiac1@correo.usbcali.edu.co |
| Santiago Cardona Gomez | Colaborador | scardonag2@correo.usbcali.edu.co |
| Valentina Zuluaga Gonzalez | Colaborador | vzuluagag1@correo.usbcali.edu.co |
| Violeta Vinasco Lopez | Colaborador | vvinascol@correo.usbcali.edu.co |
| Yeison Velasco Daza | Colaborador | yvelascod@correo.usbcali.edu.co |
| Daniel Estrella Munevar | Colaborador | destrellam@correo.usbcali.edu.co |

---

## Autor

Proyecto desarrollado como base para una solución de gestión y localización en parqueaderos.

<div align="center">

**Parqueadero busca convertir una operación cotidiana en una experiencia más ordenada, segura y fácil de usar.**

</div>
