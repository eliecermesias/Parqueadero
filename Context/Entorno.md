# Especificación del entorno

## 1. Propósito

La aplicación será un sistema web para la gestión de un parqueadero, construido con Python y Django, con MariaDB como base de datos principal.

El entorno debe permitir que el sistema crezca posteriormente hacia:

- análisis de ocupación, ingresos, permanencia y comportamiento de usuarios;
- generación de reportes y tableros de indicadores;
- automatización de tareas operativas y procesos programados;
- integración con otros sistemas mediante una API.

Estas capacidades futuras deberán incorporarse como módulos independientes. La primera versión debe concentrarse en el núcleo operativo del parqueadero y conservar datos históricos confiables.

## 2. Tecnologías base

| Componente | Tecnología propuesta | Criterio |
| --- | --- | --- |
| Lenguaje | Python 3.12 o superior | Soporte de tipos, rendimiento y ecosistema vigente |
| Framework web | Django 5.2 LTS o la versión LTS vigente al iniciar el proyecto | ORM, autenticación, administración y seguridad integrados |
| Base de datos | MariaDB 10.11 LTS o superior compatible | Persistencia transaccional y disponibilidad a largo plazo |
| Driver de base de datos | `mysqlclient` | Driver nativo recomendado para Django y MariaDB |
| API futura | Django REST Framework | Exponer operaciones a aplicaciones externas o automatizaciones |
| Servidor de desarrollo | `runserver` | Uso local únicamente |
| Servidor de producción | Gunicorn detrás de Nginx | Separar la aplicación del servidor web público |
| Control de versiones | Git | Historial y colaboración |
| Entornos | `venv` y archivo `.env` local | Separar dependencias y configuración por entorno |

Las versiones exactas deben fijarse en `requirements.txt` o `pyproject.toml` después de crear el proyecto y probar la compatibilidad entre Django, Python, MariaDB y el driver.

## 3. Componentes de la aplicación

La aplicación debe organizarse en aplicaciones Django pequeñas y con responsabilidades claras. Una estructura inicial sugerida es:

```text
parqueadero/
├── manage.py
├── config/                 # Configuración, URLs y WSGI/ASGI
├── usuarios/               # Usuarios, roles y permisos del sistema
├── parqueaderos/           # Sedes, zonas y espacios físicos
├── operaciones/           # Entradas, salidas, vehículos y tarifas
├── pagos/                  # Cobros, medios de pago y cierres
├── reportes/               # Consultas y reportes operativos
├── common/                 # Utilidades compartidas, si son necesarias
├── templates/
├── static/
├── media/
├── tests/
├── .env.example
├── requirements.txt
└── manage.py
```

La estructura puede ajustarse cuando se conozcan con más detalle los casos de uso. No se deben crear aplicaciones genéricas solo para agrupar código sin una responsabilidad concreta.

## 4. Requisitos funcionales iniciales

El núcleo debe contemplar, como mínimo:

- registro de vehículos y clientes, cuando aplique;
- registro de entrada y salida;
- identificación del espacio o zona ocupada;
- cálculo de tiempo y tarifa;
- registro de pagos y estado de la operación;
- consulta de operaciones históricas;
- usuarios autenticados con roles y permisos;
- auditoría de cambios importantes;
- configuración de tarifas y horarios sin modificar el código.

Las operaciones de entrada, salida y pago deben ejecutarse dentro de transacciones de base de datos. El modelo debe conservar fechas, usuario responsable y estado de cada operación para que los datos puedan analizarse posteriormente.

## 5. Base de datos MariaDB

La base de datos será la fuente principal de verdad para las operaciones del sistema.

### Configuración requerida

- Motor de tablas: InnoDB.
- Juego de caracteres: `utf8mb4`.
- Zona horaria consistente entre Django, MariaDB y el servidor.
- Migraciones de Django como único mecanismo normal para modificar el esquema.
- Restricciones, índices y claves foráneas definidos en los modelos.
- Copias de seguridad periódicas y prueba de restauración en ambientes no productivos.
- Usuario de base de datos separado para la aplicación; no usar el usuario administrador en producción.

### Datos preparados para análisis

Desde el inicio se deben conservar los eventos operativos, no solo el estado actual. Por ejemplo, una plaza puede estar libre actualmente, pero sus ocupaciones anteriores deben seguir disponibles para calcular la ocupación por hora, la rotación y los ingresos.

Se recomienda:

- usar `DateTimeField` con zona horaria activa;
- evitar borrar físicamente información financiera u operativa sin una política definida;
- incluir campos de creación y actualización en las entidades relevantes;
- registrar estados en lugar de sobrescribir hechos históricos;
- agregar índices para fechas, estados, vehículos y relaciones consultadas con frecuencia.

## 6. Configuración por ambiente

La configuración sensible no debe quedar escrita en el código ni en el repositorio. El archivo `.env` será local y `.env.example` documentará las variables necesarias.

Variables mínimas:

```text
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

En producción se debe usar `DJANGO_DEBUG=False`, una clave secreta segura, hosts explícitos, HTTPS y credenciales administradas fuera del repositorio.

## 7. Dependencias iniciales

Dependencias de aplicación recomendadas:

```text
Django
mysqlclient
python-dotenv
django-environ
pytest
pytest-django
coverage
```

No es necesario instalar desde el comienzo herramientas de análisis o automatización. Se agregarán cuando exista un caso de uso concreto y se haya definido dónde se ejecutarán sus procesos.

## 8. Análisis de datos: decisión pendiente

La aplicación debe exponer consultas y datos bien estructurados, pero el framework de análisis se decidirá más adelante.

Opciones orientativas:

- **Pandas y NumPy:** primera opción para análisis exploratorio, limpieza y cálculos sobre datos exportados.
- **JupyterLab:** útil para exploración y prototipos, no como interfaz principal de producción.
- **Polars:** alternativa de alto rendimiento para volúmenes grandes o procesamiento intensivo.
- **Metabase o Apache Superset:** opciones para tableros y consultas de negocio con poca programación.
- **Django REST Framework:** útil si los datos serán consumidos por un frontend o una herramienta externa.

Decisión inicial recomendada: preparar una capa de consultas y exportación en Django, y comenzar los análisis con Pandas/Jupyter cuando haya datos reales. La elección de una plataforma de tableros se hará después de conocer los indicadores y el volumen de información.

## 9. Automatizaciones: decisión pendiente

Las tareas automáticas deben quedar separadas del ciclo de una petición web. Algunos ejemplos son:

- cierre o resumen diario;
- envío de reportes;
- alertas de ocupación o pagos pendientes;
- importación y exportación de información;
- limpieza de datos temporales;
- integración con servicios externos.

Opciones orientativas:

- **Celery + Redis:** opción general para tareas en segundo plano, colas y trabajos periódicos junto con Django.
- **Django-Q2 o Huey:** alternativas más sencillas para proyectos pequeños.
- **APScheduler:** apropiado para programaciones simples, con cuidado en ambientes con varios procesos.
- **Prefect:** adecuado para flujos de datos y procesos con dependencias, reintentos y observabilidad.
- **Apache Airflow:** reservarlo para flujos de datos complejos y numerosos; puede ser excesivo para la primera versión.

Decisión inicial recomendada: no introducir un orquestador todavía. Cuando aparezca la primera tarea asíncrona real, evaluar Celery + Redis para tareas de aplicación; si los procesos se convierten en flujos de datos independientes, evaluar Prefect.

## 10. Seguridad y calidad

- Usar el sistema de autenticación y permisos de Django.
- Validar los datos en formularios, serializers y modelos según corresponda.
- Aplicar protección CSRF, sesiones seguras y políticas de contraseñas.
- No guardar contraseñas, tokens ni datos sensibles en Git.
- Registrar errores sin exponer información sensible al usuario.
- Escribir pruebas para tarifas, entradas, salidas, pagos, permisos y transacciones.
- Ejecutar migraciones y pruebas en CI antes de desplegar.
- Mantener dependencias actualizadas y revisar vulnerabilidades periódicamente.

## 11. Flujo de desarrollo

```text
1. Crear y activar un entorno virtual.
2. Instalar las dependencias fijadas.
3. Configurar MariaDB y las variables del archivo .env.
4. Crear o aplicar migraciones.
5. Ejecutar pruebas.
6. Levantar Django localmente.
7. Versionar código, migraciones y documentación, nunca secretos.
```

Comandos esperados después de inicializar el proyecto:

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py test
python manage.py runserver
```

En Windows, la activación del entorno virtual usa `venv\\Scripts\\activate`.

## 12. Criterios para elegir las tecnologías futuras

La selección de herramientas de análisis y automatización debe considerar:

- volumen y frecuencia de los datos;
- necesidad de resultados en tiempo real o por lotes;
- complejidad de los flujos y cantidad de reintentos;
- facilidad de despliegue y mantenimiento;
- experiencia del equipo;
- integración con Django y MariaDB;
- monitoreo, trazabilidad y costo.

La primera meta técnica es construir un núcleo transaccional confiable. Cuando ese núcleo tenga modelos, pruebas y datos históricos consistentes, será posible elegir las herramientas futuras con evidencia real en lugar de anticipar una plataforma que todavía no se necesita.
