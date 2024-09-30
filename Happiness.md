# 1. Einführung und Entdeckung vom Datensatz

Wir werden den Datensatz zuerst herunterladen, im R Studio öffen und schauen schnell, wie es aussieht: 
```
library(readxl)
DataForTable2_1 <- read_excel("DataForTable2.1.xls")
mydata <- DataForTable2_1
head(mydata)
str(mydata)
summary(mydata)
colnames(mydata)
dim(mydata)
```
Wie viele Länder sind in unserem Datensatz?

```

```

## 2. Welche Faktoren spielen die grösste Rolle in Happiness?

Zuerst sehen wir mal das Figur:
```
plot(mydata)
```
Werden wir unglücklicher mit der Zeit?
```

```
Ok, jetzt berechnen wir die Korrelationskoeffiziente:
```
cor(mydata)
cor(mydata[,-1])
cor(mydata[,-1], use="pairwise.complete.obs")
```
Diese sind die Korrelationskoeffiziente. Also die sagen uns die Rolle jede Faktor auf die Glücklichkeit spielt. Jetzt zeichnen wir mal die Figur:
```
happiness.corrs = cor(mydata[,-1], use="pairwise.complete.obs")[,2]
barplot(happiness.corrs)
barplot(happiness.corrs, horiz = T)
barplot(happiness.corrs, las=1, horiz = T)
happiness.corrs = cor(mydata[,-1], use="pairwise.complete.obs")[c(1,3:8),2]
barplot(happiness.corrs, las=1, horiz = T, cex.names = 0.7)
par(mar = c(5, 10, 4, 2))
barplot(happiness.corrs, las=1, horiz = T, cex.names = 0.7)

```

## 3. Tidyverse

Zuerst installieren wir die Pakete:
```
install.packages('tidyverse')
library(tidyverse)
install.packages("corrr")
library(corrr)
```

Und jetzt die Korrelationanalyse:
```
mydata %>%                                                          
  select(-`Country name`) %>%
  correlate()
```
Wir kännen auch an einzelne Spalten schauen und zwar mit Durchschnittwerten:

```
mydata %>%                                                        
  group_by(`Country name`) %>%                            
    summarise_at(vars(`Life Ladder`, `Log GDP per capita`),       
               list(avg = mean)) %>%  
  select(-`Country name`) %>%
  correlate()
```









