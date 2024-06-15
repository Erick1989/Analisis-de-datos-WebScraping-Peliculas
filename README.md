# Analisis-de-datos-WebScraping-Peliculas
# Objetivos
- 1 Aprender a instalar y cargar las librerías necesarias.
- 2 Leer una página web y extraer datos específicos.
- 3 Limpiar y estructurar los datos extraídos.
- 4 Realizar análisis de datos para obtener medidas de resumen estadístico.
- 5 Guardar los datos y resultados del análisis.
- 6 Antes de iniciar es necesario Tener R y RStudio instalados en tu computadora
  
# Link de los pasos para instalar R 
https://www.upm.es/sfs/Rectorado/Gabinete%20del%20Rector/Notas%20de%20Prensa/2015/05/documentos/Instrucciones%20de%20instalaci%C3%B3n%20de%20R%20y%20RStudio.pdf

# Paso 1: Instalación y carga de las librerías
### Instalar las librerías necesarias
- install.packages("rvest")
- install.packages("dplyr")
- install.packages("stringr")
- install.packages("tidyverse")
### Cargar las librerías
- library(rvest)
- library(dplyr)
- library(stringr)
- library(tidyverse)
# Paso 2: Leer la Pagina web (Películas con las mayores recaudaciones)
Por medio de la función read_html(). Se leerá la página de Wikipedia de Películas con las mayores recaudaciones.
Se debe introducir url <- ' seguido del link de la página <- read_html(url)

### Código R
 - url <- 'https://es.wikipedia.org/wiki/Anexo:Pel%C3%ADculas_con_las_mayores_recaudaciones'
 webpage <- read_html(url)

# Paso 3: Extraer datos específicos 
Para este ejemplo se extraerá el primer párrafo de la sección principal y la tabla de información (wikitable) sobre R.
Extraer el primer párrafo

### Código R
- #Extraer el primer párrafo
first_paragraph <- webpage %>%
     html_node('p') %>%
     html_text()
- #Mostrar el primer párrafo
 print(first_paragraph)
### Para extraer la tabla de información (wikitable)
Identificamos la tabla usando el selector adecuado y la convertimos en un dataframe.
- #URL de la página
 url <- "https://es.wikipedia.org/wiki/Anexo:Pel%C3%ADculas_con_las_mayores_recaudaciones"
 html <- read_html(url)
- #Extraer la tabla de información (wikitable)
 wikitable <- html %>% html_node(".wikitable") %>% html_table()
- #Mostrar la tabla de información
 print(wikitable)

# Paso 4 Limpiar y estructura datos
En ocasiones, es necesario limpiar el texto extraído. Podemos usar str_trim() de stringr para
eliminar espacios innecesarios.  
- #Limpiar el primer párrafo
 first_paragraph_clean <- str_trim(first_paragraph)
- #Mostrar el párrafo limpio
 print(first_paragraph_clean)

### Limpiar y estructurar la tabla de información
- #Limpiar y estructurar la tabla de información
 wikitable_clean <- wikitable %>%
     rename(Attribute = 1, Value = 2) %>%
     filter(!is.na(Attribute))
- #Mostrar la tabla de información limpia
 print(wikitable_clean)

# Paso 5: Análisis de Datos
Análisis de datos Realizaremos análisis de datos para extraer medidas de resumen estadístico. Para este ejemplo, supongamos que tenemos una columna con valores numéricos en la tabla (para fines ilustrativos, agregaremos una columna ficticia).
- #Agregar una columna ficticia de datos numéricos para el análisis
 set.seed(123) # Para reproducibilidad
 wikitable_clean$NumericValue <- sample(1:100, nrow(wikitable_clean), replace
                                      = TRUE)
- #Mostrar la tabla con la nueva columna
 print(wikitable_clean)

### Calcular medidas de resumen estadístico
Utilizaremos summarise() de dplyr para calcular medidas como la media, mediana, desviación estándar.
- #Calcular medidas de resumen estadístico
 summary_stats <- wikitable_clean %>%
     summarise(
         Mean = mean(NumericValue),
         Median = median(NumericValue),
         SD = sd(NumericValue),
         Min = min(NumericValue),
         Max = max(NumericValue)
     ) 
    
- #Mostrar las medidas de resumen estadístico
 print(summary_stats) 


### Llamar las mejores 6 películas por recaudación 
- #Crear un listado de recaudacion con las primeras 6 peliculas de dataframe
 tabla_6mejorespeliculas <- data.frame(
     Pelicula = c("Avatar", "Avengers: Endgame", "Avatar: The Way of Water", "Titanic", "Star Wars: Episodio VII - El despertar de la Fuerza", "Avengers: Infinity War"),
     Recaudacion = c("&&&&&2923706026.&&&", "&&&&&2799439100.&&&", "&&&&&2320250281.&&&", "&&&&&2264743305.&&&", "&&&&&2071310218.&&&", "&&&&&2052415039.&&&")
 )
 
 - #Quitar los símbolos &&&&&
 tabla_6mejorespeliculas$Recaudacion <- gsub("&", "", tabla_6mejorespeliculas $Recaudacion)
 
- #Mostrar el dataframe resultante
 print(tabla_6mejorespeliculas)
                                  
# Paso 6: Guardar los datos y resultados del análisis
Podemos guardar los datos extraídos y los resultados del análisis en archivos CSV para su uso posterior.
- #Guardar el primer párrafo en un archivo de texto
 writeLines(first_paragraph_clean, 'primer_parrafoPeliculas.txt') 
 
- #Guardar la tabla de información en un archivo CSV
 write.csv(wikitable_clean, 'infobox_R.csv', row.names = FALSE)

- #Guardar las medidas de resumen estadístico en un archivo CSV
 write.csv(summary_stats, 'summary_stats.csv', row.names = FALSE)

# Resumen
- 1 Instalar y cargar las librerías necesarias para web scraping y análisis de datos.
- 2 Leer una página web y extraer datos específicos.
- 3 Limpiar y estructurar los datos extraídos.
- 4 Realizar análisis de datos para obtener medidas de resumen estadístico.
- 5 Guardar los datos y resultados en archivos. 



