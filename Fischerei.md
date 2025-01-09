
# Zähldaten über die Fischerei: Wie viele Fische wurden gefangen?

Zuerst laden wir die Pakete und schauen an die Daten.
```
install.packages("COUNT")
library(COUNT)
library(ggplot2)
library(tidyverse)
data(fishing)
```
<img width="718" alt="Bildschirmfoto 2025-01-09 um 11 53 37" src="https://github.com/user-attachments/assets/946d752f-f617-4f10-bdb1-441f7828bcf1" />

## Wie wirkt sich die Tiefe auf die Anzahl der Fänge aus?
```
summary(glm(totabund ~ meandepth, data = fishing, family = poisson))
```
```
ggplot(fishing, aes(meandepth, totabund)) +
  geom_point(alpha = 0.2) 
```
```
ggplot(fishing, aes(meandepth, totabund)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "poisson"))
```
<img width="779" alt="image" src="https://github.com/user-attachments/assets/db999ee5-12d2-464a-96ca-b51474cd51f1">

Jetzt schauen wir an die anderen Paaren.

```
plot(fishing)
```
![image](https://github.com/tbilgin/DataScienceCourse/assets/26571015/5260f9bc-9864-4542-9bfb-ce0590dd1786)


## Wie wirkt sich die Grösse des Standortes auf die Anzahl der Fänge aus?

Wir werden eine Poisson Regression zwischen Ort (site) und der gefegten Fläche (sweptarea) modellieren.

```
summary(glm(totabund ~ sweptarea, data = fishing, family = poisson))

ggplot(fishing, aes(sweptarea, totabund)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "poisson"))
```
<img width="387" alt="Bildschirmfoto 2024-10-21 um 15 05 57" src="https://github.com/user-attachments/assets/9ac53a1b-9a0d-4d19-b5b2-379c18a9e2c3">

<img width="622" alt="Bildschirmfoto 2024-10-21 um 16 32 02" src="https://github.com/user-attachments/assets/909d9512-2572-4427-b123-daa585afdb76">


## Gab es mehr Fänge vorher?

```
summary(glm(period ~ totabund, data = fishing, family = binomial))

fishing %>%
  mutate(vor_kurzem = ifelse(period == "2000-2002", 1, 0)) %>%
  ggplot(aes(totabund, vor_kurzem)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "binomial"))
```

![image](https://github.com/user-attachments/assets/0c45737f-9e64-418f-8cf0-5c0fd4bb369f)

## eine weitere Logistiche Regression

Wir werden eine logistische Regression zwischen Jahr (year) und dem Zeitraum (period) modellieren, wo wir uns damit entscheiden, ob das Jahr vor kurzem war oder nicht. Nur so dass, wir eine perfekte logistische Anpassung, wie hier, erkennen dürfen.

```
glm(period ~ year, data = fishing, family = binomial)

fishing %>%
  mutate(vor_kurzem = ifelse(period == "2000-2002", 1, 0)) %>%
  ggplot(aes(year, vor_kurzem)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "binomial"))
```
![Bildschirmfoto 2023-12-15 um 16 06 31](https://github.com/tbilgin/DataScienceCourse/assets/26571015/afcb84c8-f5a7-4eea-bf7a-a73c50ce0ccc)
![image](https://github.com/tbilgin/DataScienceCourse/assets/26571015/a649be81-b1ff-4fdf-9f17-7d61b05504cf)

Die Warnung entsteht nur weil die logistische Regression die Punkte perfekt trennen könnte. Ihr musst nichts dagegen unternehmen.

## Wie wirkt sich die Dichte auf die Anzahl der Fänge aus?

Wir werden ein lineares Modell zwischen Dichte (density) und der Anzahl Fische (totabund) bilden.
```
dichte_anzahl=lm(totabund ~ density, data = fishing)
summary(dichte_anzahl)

plot(fishing$density, fishing$totabund)
lines(fishing$density,predict(dichte_anzahl), col="orange", lwd = 4)
```
<img width="664" alt="image" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/c7438c22-4403-45e8-b4e6-c77ccd46f07d">

![image](https://github.com/tbilgin/DataScienceCourse/assets/26571015/2a5ff02d-db49-4da0-8ca9-53ff19e233af)









