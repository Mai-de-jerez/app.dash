# Práctica Asignación Parte 2

## Objetivos

Después de completar el laboratorio, podrás:

- Crear un diseño de tablero con un `RadioItem` y un `Dropdown`
- Agregar un gráfico de pastel y un gráfico de barras

**Tiempo estimado necesario:** 45 minutos

## Componentes del Tablero

1. **Seleccionar Región**
2. **Seleccionar Año**
3. **División para mostrar:**
   
   - Gráfico de Pastel para mostrar el Área Promedio Estimada de Incendios Mensuales para las Regiones seleccionadas en el Año seleccionado
   - Gráfico de Barras para mostrar el Conteo Promedio Mensual de Píxeles para Incendios de Vegetación Presuntos para las Regiones seleccionadas en el Año seleccionado

## Diseño esperado

![image](https://github.com/user-attachments/assets/d5f3ce76-5469-48eb-b13b-0ae599f71830)  

## Requisitos para crear el resultado esperado

- Un menú desplegable `menu`: Para elegir el año
- Un `RadioItem` para elegir la Región
- El diseño se organizará de la siguiente manera:
  - Una división externa con dos divisiones internas (como se muestra en el diseño esperado)
  - Una de las divisiones internas tendrá información sobre el `RadioItem` y el menú desplegable (que son las entradas)
  - La otra división es para agregar gráficos (los 2 gráficos de salida)
- Función de *callback* para calcular datos, crear gráficos y devolverlos al diseño

## Por hacer

1. Importar las bibliotecas requeridas y leer el conjunto de datos  
2. Crear un diseño de aplicación  
3. Agregar un título al tablero utilizando el componente HTML `H1`  
4. Agregar un `RadioItem` utilizando `dcc.RadioItems` y un menú desplegable utilizando `dcc.Dropdown`  
5. Agregar los componentes de gráficos: gráfico de pastel y gráfico de barras  
6. Ejecutar la aplicación  

## Prepara la herramienta

1. Abre una nueva terminal haciendo clic en la barra de menú y seleccionando `Terminal -> Nueva Terminal`, como se muestra en la imagen a continuación.

![image](https://github.com/user-attachments/assets/fcd14e69-d343-4894-96ad-af4967720807)


2.	Instala los paquetes de python necesarios para ejecutar la aplicación. Copia y pega el siguiente comando en la terminal.

```
pip3.8 install setuptools
python3.8 -m pip install packaging
python3.8 -m pip install pandas dash
pip3.8 -m pip install httpx==0.20 dash plotly
```

![image](https://github.com/user-attachments/assets/6699e8cb-aa37-4d0c-a8a9-6d45bc876b00)

## Vamos a crear la aplicación

3.	Crea un nuevo archivo llamado Dash_wildfire.py

![image](https://github.com/user-attachments/assets/6965ed9c-416f-42cf-bc4a-6c84a6556b8b)






