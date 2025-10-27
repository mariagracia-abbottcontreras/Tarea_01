# Tarea_01: ¿Cuáles son las tendencias de reproducciones de los videos de Going Seventeen?

- Autora: María Gracia Abbott
- Fecha de realización: 26 de octubre de 2025


# 1. Introducción
Hace un tiempo, mientras miraba videos de "Run BTS", un show de variedades que tiene como protagonistas al grupo de K-Pop BTS, me crucé por casualidad con un video de "Going Seventeen". Lo disfruté muchisimo y buscando más me di cuenta que era el show de variedades de otro grupo de K-Pop llamado Seventeen (y que parece que es muy común que los grupos de K-Pop tengan sus propios shows de variedades). Seventeen es un grupo masculino de 13 miembros con una afinidad a ser graciosos y competitivos (entendible si tienen que luchar con otras 12 personas por tener tiempo de pantalla), por lo que pronto estaba viendo sus videos religiosamente cada miércoles y actualizandome con los que habían salido en años anteriores. 

Por lo anterior, y en el marco del curso ICP5006-1 "Medición y análisis dimensional de datos políticos", decidí analizar las tendencias generales y personales relacionadas con los videos de Going Seventeen. Para ello, utilicé dos bases de datos extraídas desde Youtube API y construidas y limpiadas en RStudio. En el siguiente trabajo se exploran los datos generales de ambas bases, para luego analizar las tendencias de reproducciones que estas contenían.

Para conseguir lo anterior, se describirán en primer lugar las bases de datos utilizadas. Después se analizarán la cantidad de videos reproducidas personalmente del total de videos disponibles, para luego realizar y comparar los 10 videos más vistos tanto a nivel general como a nivel personal. Por último, se analizarán los días que más contaron con reproducciones personales.Finalmente, se realizará una pequeña reflexión en torno a lo descubierto y aprendido de este ejercicio (spoiler: aprendí mucho más de mis hábitos digitales de lo que me hubiera gustado).

# 0.1 Índice
- 01_scripts:
  Es posible encontrar:
        - ![Base de datos - Tarea 1](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/01_scripts/Base2%20de%20datos%20-%20Tarea%201.Rmd)
          (En este script se explica la construcción de las bases de datos)
        - ![Tarea 1](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/01_scripts/Tarea%201_.Rmd)
          (En este script se visualizan las tablas y gráficos de mejor manera)
- 02_data:
    Es posible encontrar:
        - ![Playlist de Youtube de GOSE](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/02_data/youtube_playlist_GOSE_stats_complete.csv)
        - ![Historial personal de GOSE](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/02_data/historial_personal_gose.csv)
- 03_output:
    - Gráficos y visualizaciones

# 0.2 Paquetes instalados
Primero, estos son los paquetes que utilicé para realizar el análisis dispuesto abajo. En caso de no tenerlos instalados recuerda hacerlo utilizando la función "install.packages".

```{r - Instalar paquetes}
library(dplyr)
library(ggplot2)
library(tidyverse)
library(readr)
```

# 1. Bases de datos
Para la realización de esta tarea se construyeron dos bases. Ambas fueron extraídas desde Youtube API, necesitando la ayuda de "ChatGPT" y "Grok" para llegar a los codigos que me permitieron alcanzar las bases de datos que deseaba. En el archivo "Bases de datos - Tarea 1" es posible encontrar el procedimiento detallado, incluyendo los procesos de limpieza de las variables junto con sus observaciones. La primera base de datos corresponde a la unión de todas las playlists del artista que contuvieran videos de Going Seventeen. Correspondiendo estas a "Going Seventeen 2019", "Going Seventeen 2020" y "Going Seventeen", esta última conteniendo los videos desde el 2021 hasta la actualidad. Segundo, se filtró el historial personal de reproducciones de manera que contuviera unicamente aquellas relacionadas con la primera base realizada. A continuación, se importarán las bases de datos y se realizará un análisis descriptivo de estas mediante la realización de tablas descriptivas para cada una de ellas.

## 1.1  Importar y definir bases de datos como objetos

```{r}
#Definimos la base de datos relacionada con los datos de las playlists de Going Seventeen
playlist_df <- read_csv("C:/Users/Marita Abbott/Documents/R/R Magister/Tareas/Tarea_01/02_data/youtube_playlist_GOSE_stats_complete.csv")


#Definimos la base de datos de los datos de mi historial personal relacionado con Going Seventeen
historial_df <- read_csv("C:/Users/Marita Abbott/Documents/R/R Magister/Tareas/Tarea_01/02_data/historial_personal_gose.csv")
```


## 1.2 Tablas descriptivas
A continuación, se muestran las tablas descriptivas realizadas con el fin de revisar los principales datos contenidos dentro de las bases de datos utilizadas. 

### 1.2.1 Playlist de Going Seventeen
Al realizar la tabla descriptiva de los videos relacionados con Going Seventeen notamos que, a la fecha de extracción de los datos, esta contaba con 263 observaciones. Es decir, 263 videos relacionados de alguna forma con Going Seventeen, subidos por Seventeen. El video con más reproducciones cuenta con 22.051.524 vistas, mientras que el que menos tiene cuenta unicamente con 202.996, siendo el promedio de reproducciones de los videos de 5.450.476. Por otro lado, la base cuenta con datos relacionados a los likes y comentarios que cada video tiene, los cuales se pueden notar en la tabla incluída abajo.

```{r}
T_descriptiva_playlist <- summary(playlist_df)
print(T_descriptiva_playlist)

```

### 1.2.2 Historial personal de Going Seventeen
Por otro lado, al realizar una tabla descriptiva de mi historial personal, filtrado anteriormente para que contenga unicamente las vistas relacionadas con Going Seventeen, notamos que contiene 549 observaciones. Es decir, he visto 549 veces algún video de alguna de las playlists incluidas anteriormente. Por otro lado, al notar los valores mínimos y máximos, se rescata que la primera vez que vi un video de Going Seventeen fue el 30 de junio del 2022, mientras que la última vez que vi alguno de sus videos fue el 21 de octubre del 2025, fecha en la que extraje mi historial de reproducciones. Asimismo, el 50% de mis reproducciones (274–275 vistas) ocurrieron después del 20 de marzo de 2024, y el otro 50% antes de esa fecha. Dado que el período posterior (581 días) es más corto que el anterior (629 días), parece que mi frecuencia de visualización aumentó ligeramente.

```{r}
T_descriptiva_historial <- summary(historial_df)
print(T_descriptiva_historial)
```

# 2. Análisis de datos
En esta sección se mostrarán diversos análisis que se hicieron en relación a las bases utilizadas. Primero, se buscó conocer la cantidad de videos que he reproducido del total y su porcentaje asociado. Luego, en base a tablas de frecuencias, analicé los 10 videos más vistos de la playlist de Going Seventeen, los 10 videos más vistos personalmente de mi historial de reproducciones y las 10 fechas en las que mayor cantidad de videos reproduje. Las tablas de frecuencia van a su vez asociadas con un gráfico que permite visualizar de otra manera los resultados obtenidos.

Las siguientes secciones se dividen en los videos que he visto de Going Seventeen, luego las tablas de frecuencia y gráficos relacionados con los títulos de los videos, para finalizar con la tabla y el gráfico del análisis de fechas.

### 2.1.1 Videos que he visto de Going Seventeen
El primer análisis que quería hacer era saber efectivamente cuantos videos y que porción del total he visto por lo menos una vez. Así, en base a los calculos realizados abajo, ahora sé que he visto un total de 198 videos del total de 263 videos que tiene el show Going Seventeen. Lo cual corresponde a un 75.29%. Es decir, existe menos de un 25% de videos que no he visto. Puede que en algún momento vuelva a este código a revisar cuales son en específico, pero por ahora me quiero dejar sorprender.

```{r}
#Definición videos que he visto por lo menos una vez
n_videos_que_he_visto <- historial_df |> 
  inner_join(playlist_df, by = "titulo") |> 
  distinct(titulo) |> 
  nrow() 

# ¿A qué porcentaje del total corresponde?
porcentaje_vistos <- round((n_videos_que_he_visto/263) *100, 2)

```


## 2.1 Títulos de videos
Relacionado con lo anterior, quería saber cuales eran los videos más vistos de todos los videos de Going Seventeen. Asimismo, quería conocer cuales eran los videos que personalmente más había reproducido y con esto saber si existen coincidencias entre los más vistos por el público general y yo.

### 2.1.1 Tablas de frecuencias
#### 2.1.1.2 Playlist de Going Seventeen
Este apartado busca responder cuáles son los videos más vistos de la playlist generada del programa Going Seventeen. A través de la tabla de frecuencias desarrollada abajo, es posible notar que el primer video es "The Tag #1" con un total de 22.051.524 vistas, seguido de "Best Friends #2" con 19.569.210 reproducciones y "TTT #1 (Hyperrealism Ver.)" con 19.061.442 vistas. En total, los tres videos más reproducidos resultan un 4.24% del total de reproducciones que tiene en total Going Seventeen. Mientras que si se considera el top 10 de videos más vistos este número asciende a 11.54%. Es decir 10 de 263 videos concentran el 11.54% del total de reproducciones de todos los videos.

```{r}
tf_playlist <- playlist_df |> 
  group_by(titulo) |> 
  summarise(total_vistas = sum(vistas)) |> 
  mutate(porcentaje = round(total_vistas/sum(total_vistas) *100, 2)) |> 
  arrange(desc(total_vistas)) |> 
  head(10)

print(tf_playlist)

```

#### 2.1.1.2 Historial personal de Going Seventeen:
Por otro lado, al realizar el análisis de mi propio historial de reproducciones notamos que el video que más he visto es "Best Friends #2" con 12 reproducciones. El segundo lugar lo comparten "Planting Rice and Making Bets #1" y "Kickball #1" con 9 reproducciones. Mientras que el tercer lugar, con 8 reproducciones, lo tienen "I Know & I Don't Know #1", "Kickball #2" y "BOOmily Outing #3". Mis 10 videos más vistos alcanzan el 14.97% de mi total de reproducciones.

```{r}
tf_historial <- historial_df |> 
  count(titulo) |> 
  mutate(porcentaje = round(n/sum(n) * 100, 2)) |> 
  arrange(desc(n)) |> 
  head(10)

print(tf_historial)

```

#### 2.1.1.3 ¿Qué videos de mi top 10 se encuentran en el top 10 general?
Por otro lado, quise saber, y dejar plasmado en un código, si existían coincidencias entre mi top 10 de videos más reproducidos y el top 10 general. Encontrando que, efectivamente, tanto el primer como el segundo video más visto del top general de reproducciones se encuentran dentro de mi top personal. Puedo confirmar personalmente que tanto "Best Friends #2" y "The Tag #1" (siendo este además el primer video que vi de Going Seventeen) son excelentes videos y merecen todas las reproducciones que tienen.

```{r}
# Qué videos de mi top 10 se encuentran en el top 10 general
tf_historial |> 
  inner_join(tf_playlist, by = "titulo") |> 
  select(titulo) |> 
  pull()

```
### 2.1.2 Gráficos
De igual manera, quería graficar los resultados anteriores resaltando visualmente aquellos dos videos que se repiten en ambas listas.

#### 2.1.2.1 Gráfico: Playlist Going Seventeen
![Texto alternativo](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/03_output/top10_videos%2Bvistos_general_gose_plot.png)

Aquí puedes encontrar el código con el que lo realicé:
```{r}
#Gráfico tabla de frecuencias playlist general
ggplot(tf_playlist, 
       aes(x = reorder(titulo, total_vistas), 
           y = total_vistas, 
           fill = titulo %in% c("EP.32 #2 (Best Friends #2)", "EP.27 #1 (The Tag #1)"))) +
  geom_bar(stat = "identity") +
  geom_text(aes(label = total_vistas),
            hjust = -0.1,
            color = "black", size = 3) +
  scale_fill_manual(values = c("TRUE" = "#00AFBB", "FALSE" = "#B0BEC5"), guide = "none") +
  coord_flip() +
  scale_y_continuous(limits = c(0, max(tf_playlist$total_vistas) * 1.5),
                     expand = c(0, 0)) +
  labs(title = "Top 10 videos más vistos de Going Seventeen",
       x = "Nombre del video",
       y = "Número de vistas") +
  theme_minimal() +
  theme(
    axis.text.x = element_blank(),
    plot.title = element_text(face = "bold", size = 10),
    axis.text.y = element_text(size = 10),
    legend.position = "none"
  )

#Guardar y exportar gráfico
ggsave("C:/Users/Marita Abbott/Documents/R/R Magister/Tareas/Tarea 1/03_output/top10_videos+vistos_general_gose_plot.png")
```

#### 2.1.2.2 Gráfico: Historial personal de Going Seventeen
![Texto alternativo](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/03_output/top10_videos%2Bvistos_personal_gose_plot.png)

Aquí está el código de cómo lo realicé:
```{r}
#Gráfico tabla de frecuencias historial personal
ggplot(tf_historial, 
       aes(x = reorder(titulo, n), 
           y = n, 
           fill = titulo %in% c("EP.32 #2 (Best Friends #2)", "EP.27 #1 (The Tag #1)"))) +
  geom_bar(stat = "identity") +
  scale_fill_manual(values = c("TRUE" = "#00AFBB", "FALSE" = "#B0BEC5"), guide = "none") +
  coord_flip() +
  scale_y_continuous(limits = c(0,12), breaks = 0:12) + 
  labs(title = "Top 10 videos que más he visto de Going Seventeen",
       x = "Nombre del video",
       y = "Cantidad de vistas",
       fill = NULL) +
  theme_minimal()

ggsave("C:/Users/Marita Abbott/Documents/R/R Magister/Tareas/Tarea 1/03_output/top10_videos+vistos_personal_gose_plot.png")
```

## 2.2 Fechas de vistas
Por otro lado, quería también saber cuantos videos como máximo había visto por día. Siendo sincera me sorprendió el número pues pensaba que iba a ser menor. De igual manera, este análisis lo realicé mediante una tabla de frecuencias, para luego graficarlo con el fin de obtener mayor facilidad de visualización.

### 2.2.1 Tabla de frecuencias
Al analizar la tabla de frecuencias damos cuenta que el máximo de videos que he reproducido por día han sido 19, lo cual sucedió el 03 de octubre del 2024. Si consideramos que cada video dura aproximadamente 30 minutos, podemos asumir que en total me pasé 570 minutos, o mejor dicho, 9.5 horas viendo Going Seventeen (:o). El segundo lugar, se lo lleva el 05 de noviembre de 2022 con 14 reproducciones (7 horas), mientras que el tercer lugar es del 07 de noviembre del 2022 (dos días después) y el 01 de abril de 2023 con 12 reproducciones cada uno, lo que corresponde a 6 horas por día (:o). Si sumamos mi top 3 días que más reproduje videos alcanzamos un total de 28.5 horas solamente mirando videos en un total de 4 días (:o :o :o). No está de más comentar que este ejercicio me ha servido para reflexionar sobre mis hábitos digitales.

```{r}
tf_historial_fecha <- historial_df |> 
  count(fecha_vista) |> 
  mutate(porcentaje = round(n/sum(n) *100, 2)) |> 
  arrange(desc(n)) |> 
  head(10)

print(tf_historial_fecha)

```

### 2.2.2 Gráfico
Como no, no podía faltar el gráfico para que visualicemos juntos de mejor manera la mayor evidencia de que quizás me convertí en lo que juré destruir (soy basicamente una niña ipad).
![Texto alternativo](https://github.com/mariagracia-abbottcontreras/Tarea_01/blob/main/03_output/top10_fechas_gose_plot.png)

Aquí está el código de cómo lo realicé:
```{r}
#Gráfico tabla de frecuencias fechas
ggplot(tf_historial_fecha, 
       aes(x = reorder(fecha_vista, n), 
           y = n)) +
  geom_bar(stat = "identity", fill = "lightpink", color = "black") +
  coord_flip() +
  scale_y_continuous(limits = c(0,19), breaks = 0:19) + 
  labs(title = "Top 10 días en los que más vi videos de Going Seventeen",
       x = "Fecha",
       y = "Número de vistas",
       fill = NULL) +
  theme_minimal()

ggsave("C:/Users/Marita Abbott/Documents/R/R Magister/Tareas/Tarea 1/03_output/top10_fechas_gose_plot.png")

```



# 3. Conclusión
A modo de conclusión, puedo decir que la realización de este ejercicio me ayudó mucho a internalizar y practicar (por varias horas) el uso de RStudio y la generación de datos. Diría que lo más dificil fue realizar las bases de datos. Primero porque no sabía como sacar datos de Youtube antes de este ejercicio y segundo porque me demoré (y me equivoqué) mucho más de lo que me hubiera gustado. Debo confesar que a la mitad de realizar el análisis, al no cuadrarme mucho los datos entregados, me di cuenta que no había considerado dos de las playlists incorporadas, por lo que tuve que partir de nuevo y arreglar los códigos de forma que incluyera todo. Sin embargo, no me arrepiento de nada y me divertí mucho haciendo este ejercicio. Puedo confirmar que los videos del top 10 general son buenísimos y que, en efecto, disfruto muchisimo los videos que aparecen en mi top 10 videos más vistos. Asimismo, sirvió mucho para darme cuenta de mis hábitos digitales y ser más consciente, especialmente con el último ejercicio, del tiempo que paso ante las pantallas y viendo videos de 13 adultos jugando juegos y peleandose por cupones de comida. Agradezco a mi profe por la libertad para realizar este ejercicio, a Grok por ayudarme más de lo que lo hizo ChatGPT a construir las bases y a Seventeen por la inspiración para realizarlo.
