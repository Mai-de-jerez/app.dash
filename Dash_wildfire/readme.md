# Práctica Asignación Parte 2

## Objetivos

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

3.	Crea un nuevo archivo llamado `Dash_wildfire.py`

![image](https://github.com/user-attachments/assets/6965ed9c-416f-42cf-bc4a-6c84a6556b8b)


## Código

4. Creamos el código:
   
```
import pandas as pd
import dash
from dash import html, dcc
from dash.dependencies import Input, Output, State
import plotly.graph_objects as go
import plotly.express as px
from dash import no_update
import datetime as dt
# Create app
app = dash.Dash(__name__)
# Clear the layout and do not display exception till callback gets executed
app.config.suppress_callback_exceptions = True
# Read the wildfire data into pandas dataframe
df =  pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DV0101EN-SkillsNetwork/Data%20Files/Historical_Wildfires.csv')
#Extract year and month from the date column
df['Month'] = pd.to_datetime(df['Date']).dt.month_name() #used for the names of the months
df['Year'] = pd.to_datetime(df['Date']).dt.year
# Layout Section of Dash
# Add the Title to the Dashboard
app.layout = html.Div(children=[html.H1('Australia Wildfire Dashboard', 
                                style={'textAlign': 'center', 'color': '#503D36',
                                'font-size': 26}),
# Add the radio items and a dropdown right below the first inner division
     #outer division starts
     html.Div([
                   # First inner divsion for  adding dropdown helper text for Selected Drive wheels
                    html.Div([
                            html.H2('Select Region:', style={'margin-right': '2em'}),

                    #Radio items to select the region
                    #dcc.RadioItems(['NSW','QL','SA','TA','VI','WA'], 'NSW', id='region',inline=True)]),
                    dcc.RadioItems([{"label":"New South Wales","value": "NSW"},
                                    {"label":"Northern Territory","value": "NT"},
                                    {"label":"Queensland","value": "QL"},
                                    {"label":"South Australia","value": "SA"},
                                    {"label":"Tasmania","value": "TA"},
                                    {"label":"Victoria","value": "VI"},
                                    {"label":"Western Australia","value": "WA"}],"NSW", id='region',inline=True)]),
                    #Dropdown to select year
                    html.Div([
                            html.H2('Select Year:', style={'margin-right': '2em'}),
                        dcc.Dropdown(df.Year.unique(), value = 2005,id='year')
                    ]),
#Add two empty divisions for output inside the next inner division. 
         #Second Inner division for adding 2 inner divisions for 2 output graphs
                    html.Div([
                
                        html.Div([ ], id='plot1'),
                        html.Div([ ], id='plot2')
                    ], style={'display': 'flex'}),

    ])
    #outer division ends

])
#layout ends
#Add the Ouput and input components inside the app.callback decorator.
#Place to add @app.callback Decorator
@app.callback([Output(component_id='plot1', component_property='children'),
               Output(component_id='plot2', component_property='children')],
               [Input(component_id='region', component_property='value'),
                Input(component_id='year', component_property='value')])
#Add the callback function.   
#Place to define the callback function .
def reg_year_display(input_region,input_year):  
    #data
   region_data = df[df['Region'] == input_region]
   y_r_data = region_data[region_data['Year']==input_year]
    #Plot one - Monthly Average Estimated Fire Area   
   est_data = y_r_data.groupby('Month')['Estimated_fire_area'].mean().reset_index()
   fig1 = px.pie(est_data, values='Estimated_fire_area', names='Month', title="{} : Monthly Average Estimated Fire Area in year {}".format(input_region,input_year))   
     #Plot two - Monthly Average Count of Pixels for Presumed Vegetation Fires
   veg_data = y_r_data.groupby('Month')['Count'].mean().reset_index()
   fig2 = px.bar(veg_data, x='Month', y='Count', title='{} : Average Count of Pixels for Presumed Vegetation Fires in year {}'.format(input_region,input_year))    
   return [dcc.Graph(figure=fig1),
            dcc.Graph(figure=fig2) ]
if __name__ == '__main__':
    app.run()
    

```

## Agregar el título al tablero


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

## Agregar los elementos de radio y un menú desplegable justo debajo de la primera división interna.

### Elementos de radio para seleccionar la `Región`.

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


* o puede usar pares de etiquetas:valor (diccionario python) en `dcc.RadiodItems` como se muestra a continuación:

```
            #Radio items to select the region
                    dcc.RadioItems([{"label":"New South Wales","value": "NSW"},
                                    {"label":"Northern Territory","value": "NT"},
                                    {"label":"Queensland","value": "QL"},
                                    {"label":"South Australia","value": "SA"},
                                    {"label":"Tasmania","value": "TA"},
                                    {"label":"Victoria","value": "VI"},
                                    {"label":"Western Australia","value": "WA"}],"NSW", id='region',inline=True)]),

```

### Menú desplegable para seleccionar `year`

El menú desplegable tiene como `id` al `year`.
La etiqueta es `Select year`.
Los valores permitidos en el menú desplegable son los años `2005 a 2020`.
El valor predeterminado al mostrar el menú desplegable es `2005`.


```
 1           html.Div([
 2                      html.H2('Select Year:', style={'margin-right': '2em'}),
 3                      dcc.Dropdown(df.Year.unique(), value = 2005,id='year')
 4                      #notice the use of unique() from pandas to fetch the values of year from the dataframe for dropdown
 5                   ]),
```

## Agregar dos divisiones vacías para la salida dentro de la siguiente división interna.

* Uso dos etiquetas `html.Div()` para crear las divisiones.
* Proporciono los identificadores de división `plot1` y `plot2`.

```
1   html.Div([ ], id='plot1'),
2   html.Div([ ], id='plot2')
```

## Agregar los componentes de salida y entrada dentro del decorador app.callback.

* Las entradas y salidas de la interfaz de nuestra aplicación se describen declarativamente como los argumentos del decorador `@app.callback`.
- En Dash, las entradas y salidas de nuestra aplicación son simplemente las propiedades de un componente específico.

* En este ejemplo, tenemos dos `inputs`:
- La entrada para Región es la propiedad `value` del componente con el ID `region`.
- La entrada para Año es la propiedad `value` del componente con el ID `year`.

* Nuestro diseño tiene dos `outputs`, por lo que necesitamos crear dos componentes de salida.

Es una lista con dos parámetros de salida: el ID del componente y la propiedad. Aquí, la propiedad del componente será `children`, ya que hemos creado una división vacía y pasado `dcc.Graph` (figura) después del cálculo.

Los ID de los componentes serán `plot1` y `plot2`.

```
@app.callback([Output(component_id='plot1', component_property='children'),
               Output(component_id='plot2', component_property='children')],
               [Input(component_id='region', component_property='value'),
                Input(component_id='year', component_property='value')])
```

## TAREA 2.5: Agregar la función de devolución de llamada.
* Cada vez que cambia una propiedad de entrada, la función que encapsula el decorador de devolución de llamada se llamará automáticamente.
* En este caso, definamos la función `reg_year_display()`, que encapsulará nuestro decorador.
* La función primero filtra nuestro marco de datos `df` por el valor seleccionado de la región de los elementos de radio y el año del menú desplegable, como se muestra a continuación: `region_data = df[df['Region'] == input_region]`  `y_r_data = region_data[region_data['Year']==input_year]`
* Para el gráfico circular cogeremos el promedio mensual de la estimación de área incendiada:
   * A continuación, agruparemos por mes y calcularemos el promedio del área estimada de incendio del marco de datos.
   * Utilice la función `px.pie()` para trazar el gráfico circular.
* Para el gráfico de barras sobre el promedio mensual de píxeles para presuntos incendios de vegetación:
   * A continuación, agruparemos por mes y calcularemos el promedio del recuento del marco de datos. Utilice la función `px.bar()` para trazar el gráfico de barras.
 
```
def reg_year_display(input_region,input_year):
    
    #data
   region_data = df[df['Region'] == input_region]
   y_r_data = region_data[region_data['Year']==input_year]
    #Plot one - Monthly Average Estimated Fire Area
   
   est_data = y_r_data.groupby('Month')['Estimated_fire_area'].mean().reset_index()
 
   fig1 = px.pie(est_data, values='Estimated_fire_area', names='Month', title="{} : Monthly Average Estimated Fire Area in year {}".format(input_region,input_year))
   
     #Plot two - Monthly Average Count of Pixels for Presumed Vegetation Fires
   veg_data = y_r_data.groupby('Month')['Count'].mean().reset_index()
   fig2 = px.bar(veg_data, x='Month', y='Count', title='{} : Average Count of Pixels for Presumed Vegetation Fires in year {}'.format(input_region,input_year))
    
   return [dcc.Graph(figure=fig1),
            dcc.Graph(figure=fig2) ]
```





