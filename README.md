# Análisis Financiero de Ventas Adventure Works Cycles
Empresa multinacional especializada en la producción y fabricación de bicicletas, piezas y accesorios, con presencia en mercados de Norteamérica, Europa y Asia. 

## 🎯Objetivos:

El objetivo principal es comprender los factores que inciden en las ventas, costos y rentabilidad de la empresa, con el fin de facilitar la toma de decisiones estratégicas basadas en datos sobre el rendimiento de la empresa, incluyendo un Análisis de Mercado General y otro específico de Estados Unidos.

- Crear un modelo de datos relacional que refleje las necesidades del negocio.
- Mejorar la calidad de los datos a través de una limpieza efectiva.
- Utilizar DAX para calcular métricas clave de indicadores clave de rendimiento y análisis financiero
- Desarrollar un dashboard Financiero en Power BI para analizar el rendimiento de ventas de AWC. 

## 💻Descarga e Instalación de Power BI:

[Descarga de Power Bi Desktop](https://www.microsoft.com/es-es/download/details.aspx?id=58494) para abrir el dashboard

## 🗂️ Desarrollo del Proyecto

Aquí puede ingresar a ver el [Dashboard de Análisis Financiero Ventas Adventure Works Cycles](https://github.com/MFlorenciaLoCascio/BD_AventureWorks2019_ProyectoHenry/blob/main/An%C3%A1lisis_Financiero_Ventas_Adventure_Works_Cycles.pbix)

### 📊 AVANCE 1 

#### 1️⃣ Descargue los archivos:
Base de datos [AdventureWorksDW2019.bak](https://learn.microsoft.com/es-es/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)

Base de datos [DimCustomer.xlsx](https://github.com/MFlorenciaLoCascio/BD_AventureWorks2019_ProyectoHenry/blob/main/DimCustomer.xlsx)

#### 2️⃣ Restaure la base de datos `AdventureWorksDW2019` en SQL Server. 

+ Importe en Power Bi, las siguientes tablas:
    
  + DimProduct
  + DimProductCategory
  + DimProductSubcategory
  + DimDate
  + DimPromotion
  + DimSalesTerritory
  + DimGeography
  + FactInternetSales

+ Luego, importa el archivo de Excel `DimCustomer`

#### 3️⃣ Definí relaciones y cardinalidades entre tablas:

La tabla DimCustomer no está conectada al modelo de datos.
Se determina que la tabla FactInternetSales es la que debe conectarse con DimCustomer, ya que ambas tablas comparten la columna CoustomerKey, que se utilizará para la conexión. 
Se crea una relación entre DimCustomer y FactInternetSales usando las columnas CoustomerKey. Con una cardinalidad de 1 a muchos (1:N) 

#### 4️⃣ Realicé las siguientes transformaciones de tabla DimCustomer en Power Query:

🔸 Eliminación de columnas:

  +  Tabla DimCustomer: Columna18, Columna31, Surflix, SpanishEducation, FrenchEducation, SpanishOccupation y FrenchOccupation, y 12 filas con valores nulos en la columna CoustomerKey.
  +  Tabla FactInternetSales: CarrierTrackingNumber y CoustomerPONumber

🔸 Combinación de columnas:

+ En la tabla DimCustomer, se combinaron las columnas CountryRegionCode, CountryRegionCode_1, CountryRegionCode_2, CountryRegionCode_3, CountryRegionCode_4 y CountryRegionCode_5.

🔸 Modificación de tipos de datos:

+ Tabla DimCoustomer: El tipo de dato de la columna "Title" se cambió de "cualquiera" a "texto".○
+ Tabla DimPromotion: El tipo de dato de la columna DiscountPct se modificó a porcentaje.

🔸 Combinaciones y Fusiones de Tablas:

+ Combinaciones:
  + DimCustomer y DimGeography: Estas tablas se combinaron para integrar información geográfica (ciudad, provincia y código) directamente en la tabla de clientes (DimCustomer). Se utilizó una fusión de tipo "Externa Izquierda" (Left Join) basada en la columna GeographyKey presente en ambas tablas.

+ Fusiones:
 + DimProduct, DimProductCategory y DimProductSubcategory: Se fusionaron estas tres tablas para organizar la información de categorías y subcategorías como clasificaciones dentro de la tabla de productos (DimProduct).
   
    + Primer paso: Se realizó una fusión de tipo "Externa Izquierda" (Left Outer) entre las tablas DimProduct (utilizando la columna ProductSubcategoryKey) y DimProductSubcategory (utilizando la columna ProductSubcategorykey).
Luego, se expandieron las columnas EnglishProductSubcategoryName y ProductCategoryKey. Finalmente, se renombró la columna “EnglishProductSubcategoryName” a “ProductSubcategoryName”.

    + Segundo paso: Se fusionaron las tablas DimProduct (utilizando la columna DimProductSubcategory.1.ProductCategoryKey) y ProductCategory (utilizando la columna ProductCategoryKey) con una fusión de tipo "Externa Izquierda" (Left Outer).
Se expandió la columna EnglishProductCategoryName y se renombró la columna “DimProductCategory.EnglishProductCategoryName” a “ProductCategoryName”. Para finalizar, se eliminaron las columnas DimProductSubcategory.1.ProductCategoryKey y DimProductSubcategory.

🔸 Columnas eliminadas relacionadas con idiomas:
  
+ Tabla DimProduct:
    + Se mantuvieron las columnas: EnglishDescription y EnglishProductName.
    + Se eliminaron: FrenchDescription, ChineseDescription, ArabicDescription, HebrewDescription, ThaiDescription, GermanDescription, JapaneseDescription, TurkishDescription, SpanishProductName y FrenchProductName.

+ Tabla DimProductCategory:
    + Se mantuvo la columna: EnglishProductCategoryName.
    + Se eliminaron: SpanishProductCategoryName y FrenchProductCategoryName.
 
+ Tabla DimProductSubcategory:
    + Se mantuvo la columna: EnglishProductSubcategoryName
    + Se eliminaron: SpanishProductSubcategoryName y FrenchProductSubcategoryName

+ Tabla DimGeography:
    + Se mantuvo la columna: EnglishCountryRegionName
    + Se eliminaron: SpanishCountryRegionName y FrenchCountryRegionName.

+ Tabla DimCustomer:
    + Se mantuvieron las columnas: EnglishEducation y EnglishOccupation.
    + Se eliminaron: SpanishEducation, FrenchEducation, SpanishOccupation y FrenchOccupation.

+ Tabla DimDate:
    + Se mantuvieron las columnas: EnglishDayNameOfWeek y EnglishMonthName.
    + Se eliminaron: SpanishDayNameOfWeek, FrenchDayNameOfWeek, SpanishMonthName y FrenchMonthName.
 
+ Tabla DimPromotion:  
    + Se mantuvieron las columnas: EnglishPromotionName, EnglishPromotionType y EnglishPromotionCategory.
    + Se eliminaron: SpanishPromotionName, FrenchPromotionName, SpanishPromotionType, FrenchPromotionType, SpanishPromotionCategory y FrenchPromotionCategory. 

### 📈 AVANCE 2 

#### Modelo Relacional y Mockup del proyecto

Realicé el diseño del mockup que aborde problemas de negocio específicos, siguiendo el patrón Z para facilitar la comprensión y el análisis.
Generé el modelo relacional eficiente controlando todas las relaciones que generó Power BI de manera automática, así como la cardinalidad de estas relaciones.

🎯 El objetivo es abordar un problema de negocio específico: analizar los ingresos, costos y rentabilidad de la empresa, con un enfoque particular en el mercado de Estados Unidos.

Se plantean preguntas claves que el reporte debe responder, como el total de ingresos, la cantidad vendida, la utilidad bruta y neta, el COGS, la distribución de clientes por país y la utilidad por segmento de producto. Se incluyen fórmulas DAX para calcular estas métricas. También se describe la creación de visualizaciones como mapas, tablas y gráficos para mostrar la información de manera efectiva.

### 📋 AVANCE 3 

Generación de medidas y columnas calculadas utilizando DAX para analizar los ingresos, costos y otros indicadores clave.
Se realizan las siguientes acciones:

  + Se agrega una columna personalizada en Power Query para el nombre del mes en formato corto.
  + Se deshabilita la carga de ciertas tablas en Power Query para optimizar el modelo.
  + Se crea una tabla Calendario para una mejor gestión de las fechas.
  + Se crea una columna calculada en la tabla DimDate para indicar el trimestre.
  + Se generan medidas DAX para calcular indicadores como ingresos, costos, márgenes de utilidad.
  + Se organizan las medidas en una tabla de medidas y carpetas por tipo para facilitar su uso.
  + Se crea una medida adicional para calcular el costo total incluyendo el envío.

### 💻 AVANCE 4

Creación de un tablero completo en Power BI que permita analizar el rendimiento de Adventure Works, con un enfoque especial en el mercado de Estados Unidos.
Se presentan de manera clara los ingresos, costos, rentabilidad y otros indicadores claves.

1- Parámetros de mapa: 
  + Crear parámetros para controlar la información mostrada en los mapas, permitiendo al usuario filtrar por indicadores como Ingresos, Utilidad Bruta, Utilidad Neta, COGS, % Margen Bruto, % Margen Neto y Costo de Envío.

2- Indicadores:
  + El tablero incluye indicadores clave de rendimiento (KPIs) como Ingresos, Utilidad Neta, Utilidad Bruta, COGS, % Margen Neto, % Margen Bruto y Costo de Envío

3- Segmentador de mapa:
  + Utilizar un segmentador conectado a los parámetros del mapa para que el usuario pueda filtrar la información geográfica mostrada en los mapas de clientes por país.

4- Grupo de cálculo "Variacion_Tiempo": 
  + Este grupo se crea para mostrar información detallada del mercado de Estados Unidos, incluyendo el período actual, el período anterior, la variación entre ambos y la variación porcentual.

5- Elementos visuales y de navegación:
  + Incorporar al reporte botones de navegación, el logo de la empresa, imágenes complementarias e imágenes que funcionen como botones con acciones asignadas.

## ANÁLISIS DE MERCADO GENERAL 

![Análisis Mercado General](https://github.com/MFlorenciaLoCascio/BD_AventureWorks2019_ProyectoHenry/assets/139195222/6d445a3b-9147-4e39-9eb5-70f1192f81ce)

## ANÁLISIS DE MERCADO ESTADOS UNIDOS

![Análisis_Mercado_EEUU](https://github.com/MFlorenciaLoCascio/BD_AventureWorks2019_ProyectoHenry/assets/139195222/7f9d14b9-3f5a-49c3-b628-1d46dafddb55)

#### ➡️ Análisis del Tablero General:

- KPIs financieros: Mostrar cifras actuales y comparativas del año anterior para Ingresos, Utilidad Neta y COGS.
- Variaciones porcentuales: Mostrar la variación porcentual de Ingresos y COGS respecto al período anterior
- Rentabilidad: Resaltar la Utilidad Bruta y el Margen de Utilidad Neta.
- Distribución geográfica: Incluir un mapa de clientes por país.
- Segmentación de productos: Mostrar el rendimiento por Categorías de Productos y Subcategorías.

#### ➡️ Análisis del Tablero de Estados Unidos:

- Mapa de provincias: Mostrar el rendimiento por estado.
- Detalles financieros por ciudad: Presentar información detallada de COGS y Utilidad Bruta por ciudad.
- Evolución de ingresos acumulados: Incluir un gráfico de línea que muestre la evolución de los ingresos acumulados.
- Iteraciones: Considerar la posibilidad de realizar iteraciones para mejorar la interpretación visual de los datos.

### 📊 Insights y Recomendaciones:

#### 🗒️ Análisis de Mercado General

**1. Cantidad de Ventas con respecto a los años anteriores**

Cantidad Total de Ventas: 60 mil unidades.
Ventas Esperadas: 64 mil unidades.
Insight: Las ventas actuales están ligeramente por debajo de las expectativas. Es importante ajustar las estrategias de marketing y ventas para cerrar esta brecha.

**2. COGS (Costo de los Bienes Vendidos) con respecto a los años anteriores**

COGS Total: $17 millones.
COGS Esperado: $19 millones.
Insight: El COGS actual es menor que el esperado, lo cual es positivo ya que indica un control efectivo de costos. 
Continuar optimizando los procesos de producción y negociación con proveedores podría reducir aún más estos costos y mejorar la rentabilidad.

**3. Ingresos con respecto a los años anteriores**

Ingresos Totales: $29 millones.
Ingresos Esperados: $32 millones.
Insight: Los ingresos actuales están por debajo de las expectativas. Evaluar las estrategias de fijación de precios, promociones y ventas cruzadas puede ayudar a aumentar los ingresos y alcanzar las expectativas.

**4. Utilidad Bruta con respecto a los años anteriores**

Utilidad Bruta Total: $12 millones.
Utilidad Bruta Esperada: $13 millones.
Insight: La utilidad bruta está cercana a la expectativa, pero aún hay un margen de mejora. Aumentar las ventas mientras se mantienen los costos controlados es crucial para alcanzar y superar la utilidad bruta esperada.

**5. Utilidad Neta con respecto a los años anteriores**

Utilidad Neta Total: $9 millones.
Utilidad Neta Esperada: $10 millones.
Insight: La utilidad neta está por debajo de la expectativa, lo que sugiere que hay margen para mejorar la eficiencia operativa y reducir los gastos generales. Implementar medidas de ahorro de costos y optimizar los procesos internos puede ayudar a mejorar la utilidad neta.

**6. Rentabilidad por Categoría:**

Accesorios tienen una gran proporción de ventas. 
Se sugiere mejorar la eficiencia en la cadena de suministro para reducir los costos y aumentar la rentabilidad de esta categoría.

**7. Subcategorías más vendidas:**

- Neumáticos y ruedas son los productos más vendidos. 
Crear estrategias de marketing y promociones específicas para estos productos para aumentar las ventas.

- Componentes de carretera también muestran una alta demanda.
Se considera ampliar la línea de productos o mejorar la oferta de estos componentes.

**8. Clientes por País:**

Estados Unidos muestra una alta densidad de clientes. 
Considerar estrategias de expansión y localización de productos y servicios para estos mercados.

**9. Costos y Margen Bruto:**

El COGS (Costo de los bienes vendidos) esperado es considerablemente alto. 
Revisar proveedores y procesos de producción para identificar oportunidades de reducción de costos.

#### 🗒️ Análisis de Mercado de Estados Unidos

**1. Ventas por Categoría:**

- Los Accesorios representan una gran parte de las ventas y las Bicicletas tienen una venta significativa.
Ofrecer promociones cruzadas con accesorios para bicicletas para aumentar el ticket promedio.

- La ropa tiene menor demanda con respecto a las demas categorías.
Se propone lanzar nuevas colecciones de ropa y realizar campañas de marketing para maximizar la visualización. 

**2. COGS y Utilidad Bruta por Ciudad:**

Bellflower y Burbank tienen altos costos de bienes vendidos (COGS) y utilidad bruta. 
Investigar más sobre estas ciudades para identificar qué está impulsando los costos y para optimizar la cadena de suministro en estas áreas.

**3. Ingresos Acumulados:**

Hay un crecimiento continuo en los ingresos acumulados desde 2011 hasta 2013, con un notable aumento hacia el final de 2013. En 2014 bajaron significativamente.
Se recomienda invertir en marketing y expandir la presencia de mercado.

**4. Estacionalidad de Ventas:**

Las ventas son más altas en los meses de diciembre y noviembre. 
Planificar inventarios y promociones especiales para estos meses para aprovechar al máximo la temporada alta.

## 📍 Recomendaciones Generales para ambos mercados

- Optimización de Inventario y Cadena de Suministro: Implementar técnicas de gestión de inventario y negociar mejores términos con proveedores para reducir el COGS en ambos mercados.

- Estrategias de Marketing: Desarrollar campañas de marketing específicas para las regiones con mayor densidad de clientes (Europa y Estados Unidos). Aprovechar las estacionalidades y las promociones cruzadas para maximizar las ventas.

- Expansión de Productos: Introducir nuevos productos y variaciones en ambas regiones para atraer a una base de clientes más amplia y diversificar las fuentes de ingresos.

- Eficiencia Operativa: Revisar y optimizar los procesos internos para reducir costos operativos y mejorar la utilidad neta en ambos mercados.

- Fidelización de Clientes: Implementar programas de fidelización y mejorar la satisfacción del cliente para incrementar las ventas repetitivas y fortalecer la base de clientes existente.

## 📈 Conclusión:

El mercado general y el mercado de Estados Unidos muestran un rendimiento sólido con varias oportunidades claras para mejorar y crecer. Implementar estrategias específicas para aumentar las ventas, reducir costos y mejorar la eficiencia operativa puede llevar a un mayor éxito y rentabilidad en ambas regiones. Mantener un enfoque en la optimización continua y la adaptación a las tendencias del mercado será clave para el crecimiento sostenido a largo plazo.

⚙️ Herramientas utilizadas:
- SQl Server
- Power Bi
- Power Query
- DAX
- Figma
- Canva
