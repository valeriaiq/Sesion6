# Postwork Sesión 6
Series de Tiempo

Importa el conjunto de datos match.data.csv a `R` y realiza lo siguiente:

1. Agrega una nueva columna `sumagoles` que contenga la suma de goles por partido.

2. Obtén el promedio por mes de la suma de goles.

3. Crea la serie de tiempo del promedio por mes de la suma de goles hasta diciembre de 2019.

4. Grafica la serie de tiempo.

```R
setwd("C:/Users/Documents/Postwork6")

library(dplyr)

match <- read.csv("match.data.csv") 
str(match)
class(match)

# Cambiar el formato de la fecha a date
match <- mutate(match, date = as.Date(date, "%Y-%m-%d"))

# Añadir la columna de goles totales sumando los goles de visitante y de local
match$sumagoles <- match$home.score + match$away.score

# Agrupar por mes y obtener goles promedio
match_new <- mutate(match, YearMonth = format(date, "%Y-%m"))
match2 <- match_new %>% group_by(YearMonth) %>% summarise(golesprom = mean(sumagoles))

# Obtener serie de tiempo hasta Diciembre de 2019
match.ts <- ts(match2$golesprom, start = c(1, 1), end = c(8,12), frequency = 12)
class(match.ts)

#Graficar serie de tiempo
plot(match.ts, ylab = "Goles Promedio", 
     xlab = "Años Transcurridos", 
     col = "deepskyblue3",
     main = "Goles Promedio por Mes en la Liga Española",
     sub = "Ago-2010 a Dic-2019")
