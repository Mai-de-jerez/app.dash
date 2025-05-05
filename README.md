# Proyecto Dash - Dashboards Interactivos

Este proyecto contiene varias aplicaciones interactivas creadas con **Dash**, **Plotly** y **Pandas**. Estas aplicaciones permiten visualizar y analizar datos de manera interactiva a través de gráficos y tablas dinámicas.

## Requisitos

Para ejecutar este proyecto, necesitas tener instalados los siguientes paquetes:

- **Python 3.x**: Asegúrate de tener una versión reciente de Python instalada en tu máquina.
- **Pandas**: Para el manejo y análisis de datos.
- **Plotly**: Para crear gráficos interactivos.
- **Dash**: El framework que utilizamos para construir aplicaciones web interactivas.

## Clonar el repositorio

Para comenzar a usar este proyecto, primero clona el repositorio en tu máquina local. Abre tu terminal y ejecuta:

```bash
git clone https://github.com/Mai-de-jerez/app.dash.git
```

## Instalación de dependencias

Dentro de la carpeta del proyecto, crea un entorno virtual (opcional pero recomendado):

```bash
python -m venv venv
```
## Activar el entorno virtual:

**En Windows:**

```bash
.\venv\Scripts\activate
```
**En macOS/Linux:**

```bash
source venv/bin/activate
```

## Instalar las dependencias usando pip:

```bash
pip install -r requirements.txt
```
## Generar el archivo requirements.txt si no existe:
```bash
pip freeze > requirements.txt
```
(Este paso es opcional y solo necesario si aún no tienes el archivo requirements.txt).

## Ejecutar la aplicación Dash (asumiendo que el archivo principal es app.py):
```bash
python app.py
```
## Ejecutar la aplicación Dash (si el archivo principal tiene otro nombre, por ejemplo, index.py):
```bash
python index.py
```
(Reemplaza index.py con el nombre de tu archivo principal si es diferente).

## Abrir tu navegador web.
Navegar a la siguiente URL para ver la aplicación:

```
http://127.0.0.1:8050/
```
