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


## Obtenga el esqueleto de la aplicación

4. Puedes usar este archivo esqueleto como estructura para crear el código base para completar la siguiente tarea.

   
```
import pandas as pd
import dash
from dash import html, dcc
from dash.dependencies import Input, Output, State
import plotly.graph_objects as go
import plotly.express as px
from dash import no_update
import datetime as dt
#Create app
app = dash.Dash(__name__)
# Clear the layout and do not display exception till callback gets executed
app.config.suppress_callback_exceptions = True
# Read the wildfire data into pandas dataframe
df =  pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DV0101EN-SkillsNetwork/Data%20Files/Historical_Wildfires.csv')
#Extract year and month from the date column
df['Month'] = pd.to_datetime(df['Date']).dt.month_name() #used for the names of the months
df['Year'] = pd.to_datetime(df['Date']).dt.year
#Layout Section of Dash
#Task 2.1 Add the Title to the Dashboard
app.layout = html.Div(children=[html.H1(..................),
# TASK 2.2: Add the radio items and a dropdown right below the first inner division
#outer division starts
     html.Div([
                   # First inner divsion for  adding dropdown helper text for Selected Drive wheels
                    html.Div([
                            html.H2(.........),
                    #Radio items to select the region
                    #dcc.RadioItems(['NSW',.....], value ='...', id='...',inline=True)]),
                    dcc.RadioItems([{"label":"New South Wales","value": "NSW"},
                                    {..........},
                                    {..........},
                                    {..........},
                                    {..........},
                                    {..........},
                                    {"label":"...","value": ..}], value = "...", id='.....,inline=True)]),
                    #Dropdown to select year
                    html.Div([
                            html.H2('.........', style={...........}),
                        dcc.Dropdown(.....................)
                    ]),
#Second Inner division for adding 2 inner divisions for 2 output graphs
#TASK 2.3: Add two empty divisions for output inside the next inner division.
                    html.Div([
                
                        html.Div([ ], id='........'),
                        html.Div([ ], id='.........')
                    ], style={'.........}),
    ])
    #outer division ends
])
#layout ends
#TASK 2.4: Add the Ouput and input components inside the app.callback decorator.
#Place to add @app.callback Decorator
@app.callback([Output(component_id=.........., component_property=..........),
               Output(component_id=.........., component_property=..........)],
               [Input(component_id=.........., component_property=..........),
                Input(component_id=.........., component_property=..........)])
   
#TASK 2.5: Add the callback function.
#Place to define the callback function .
def reg_year_display(input_region,input_year):
    
    #data
   region_data = df[df['Region'] == input_region]
   y_r_data = region_data[region_data['Year']==input_year]
    #Plot one - Monthly Average Estimated Fire Area
   
   est_data = .........................
   fig1 = px.pie(.............., title="{} : Monthly Average Estimated Fire Area in year {}".format(input_region,input_year))
   
     #Plot two - Monthly Average Count of Pixels for Presumed Vegetation Fires
   veg_data = .............................
   fig2 = px.bar(..............., title='{} : Average Count of Pixels for Presumed Vegetation Fires in year {}'.format(input_region,input_year))
    
   return [.......,
            ......... ]
if __name__ == '__main__':
    app.run()

```

## TAREA 2.1: Agregar el título al tablero


Actualice la etiqueta `html.H1()` para incluir el título de la aplicación.

El título de la aplicación es `Australia Wildfire Dashboard`.
Utilice el parámetro de estilo proporcionado a continuación para centrar el título con `center`, usar el código de color `#503D36` y usar el tamaño de fuente `26`.

```
1    html.H1('Australia Wildfire Dashboard', 
2                                  style={'textAlign': 'center', 'color': '#503D36',
3                                  'font-size': 26}),
```


Después de actualizar `html.H1()` con el título de la aplicación, el `app.layout` se verá así:

![image](https://github.com/user-attachments/assets/69565a6c-907d-480e-856f-95370bcfb7a1)

## TAREA 2.2: Agregue los elementos de radio y un menú desplegable justo debajo de la primera división interna.

Elementos de radio para seleccionar la `Región`.

Los elementos de radio funcionan de forma similar al menú desplegable: debe llamar a dcc.RadioItems y pasar la lista de elementos. Use la propiedad inline=True para mostrar los elementos de radio en una línea horizontal.

* Puede extraer las regiones del dataframe usando `df.Region.unque()` o pasar la lista de todas las regiones directamente como `['NSW','QL','SA','TA','VI','WA','NT']`.
* Asignar el `id` de los elementos de radio como `región`.
* Etiquetar como `Seleccionar Región`.
* Valor como `NSW`.
  

Para su referencia, a continuación se muestran las abreviaturas utilizadas en el conjunto de datos para las regiones.

NSW - New South Wales  
NT - Northern Territory  
QL - Queensland  
SA - South Australia  
TA - Tasmania  
VI - Victoria  
WA - Western Australia


```
 1 html.Div([
 2          html.H2('Select Region:', style={'margin-right': '2em'}),
 3            #Radio items to select the region
 4            dcc.RadioItems(['NSW','QL','SA','TA','VI','WA'], 'NSW', id='region',inline=True)]),
 ```           


or you can use labels:value pair a well in raioditems as below
1
2
3
4
5
6
7
8
9
#OR you can use labels:value pair a well in raioditems as below
            #Radio items to select the region
                    dcc.RadioItems([{"label":"New South Wales","value": "NSW"},
                                    {"label":"Northern Territory","value": "NT"},
                                    {"label":"Queensland","value": "QL"},
                                    {"label":"South Australia","value": "SA"},
                                    {"label":"Tasmania","value": "TA"},
                                    {"label":"Victoria","value": "VI"},
                                    {"label":"Western Australia","value": "WA"}],"NSW", id='region',inline=True)]),

Copied!
Dropdown to choose the Year

The dropdown has an id as year.
The label as Select Year
The values allowed in the dropdown are years from 2005 to 2020
The default value when the dropdown is displayed is 2005.
1
2
3
4
5
            html.Div([
                        html.H2('Select Year:', style={'margin-right': '2em'}),
                        dcc.Dropdown(df.Year.unique(), value = 2005,id='year')
                        #notice the use of unique() from pandas to fetch the values of year from the dataframe for dropdown
                    ]),







