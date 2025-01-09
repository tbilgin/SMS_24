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

# Übungen mit tidyverse
Zuerst installieren wir die Pakete:
```
install.packages('tidyverse')
library(tidyverse)
install.packages("corrr")
library(corrr)
```

```
mydata  %>%  filter(Countryname == "Switzerland" & Year > 2010) %>% group_by(Year)
```
<img width="799" alt="Bildschirmfoto 2023-09-27 um 10 32 50" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/47adbeaf-e4f1-41a6-a28c-d4bf3470b3d5">



```

mydata %>% group_by(Countryname) %>%  select(-(LogGDPpercapita:Generosity)) %>% filter(Perceptionsofcorruption == min(Perceptionsofcorruption))

```
<img width="418" alt="Bildschirmfoto 2023-09-27 um 10 37 19" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/df690d2e-9efe-41a6-938a-6180232f3124">

```
mydata %>% group_by(Countryname) %>% summarise(avg_GDP = mean(LogGDPpercapita), st_dev = sd(LogGDPpercapita))
```
<img width="308" alt="Bildschirmfoto 2023-09-27 um 10 39 05" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/638c953c-47d7-400c-a4c5-40c05c8b1f26">


# 2. Korrelationsanalyse

```
cor(mydata)
cor(mydata[,-1])
cor(mydata[,-1], use="pairwise.complete.obs")
```
Diese sind die Korrelationskoeffiziente. Also die sagen uns die Rolle jede Faktor auf die Zufriedenheit spielt. Jetzt zeichnen wir mal die Figur:
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

Und jetzt mit tiyverse
```
mydata %>%                                                          
  select(-`Country name`) %>%
  correlate()
```
Wir können auch einzelne Spalten mit Durchschnittswerten betrachten:

```
mydata %>%                                                        
  group_by(`Country name`) %>%                            
    summarise_at(vars(`Life Ladder`, `Log GDP per capita`),       
               list(avg = mean)) %>%  
  select(-`Country name`) %>%
  correlate()
```

Jetzt Plots:

```
plot(mydata %>%                                                        
       group_by(`Country name`) %>%                            
       summarise_at(vars(`Life Ladder`, `Log GDP per capita`),       
                    list(avg = mean)) %>%  
       select(-`Country name`))
plot(mydata %>%                                                          
  select(`Life Ladder`, `Log GDP per capita`))
```

# 3. Lineare Regression

Für ein linear Regressionsmodell zu bilden, das den Zusammenhang zwischen Einkommen, erstellen wir zuerst einen neuen Datensatz, einen Subset:

```
Einkommen <- mydata %>%                                                          
  select(`Life Ladder`, `Log GDP per capita`)
colnames(Einkommen)<- c("Zufriedenheit","BIP")
```

Und jetzt bilden wir das Model:
```
trend <- lm(Zufriedenheit ~ BIP, data = Einkommen)
summary(trend)
```

<img width="207" alt="Bildschirmfoto 2023-09-27 um 14 05 57" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/b2f97f3a-94fc-43b4-867f-afb47248c08f">

## Trendlinie erstellen

```
plot(Einkommen$BIP, Einkommen$Zufriedenheit)
abline(trend,col="red")
```



## ein ausführlicher Blick auf die Ausgabe

Residuen: Differenz zwischen den beobachteten und den erwarteten Werten. Wir können die gleichen Werte erstellen, indem wir die aktuellen Werte nehmen und sie von den erwarteten Werten des Modells subtrahieren:
```
hist(trend$residuals)
hist(Einkommen$Zufriedenheit-trend$fitted.values)
```
Es gibt eine Ermahnung, warum?
```
hist(na.omit(Einkommen)$Zufriedenheit-trend$fitted.values)
```
Jetz können wir auch die Quartile betrachten:
```
summary(trend$residuals)
summary(na.omit(Einkommen)$Zufriedenheit-trend$fitted.values)

```

Um den Korrelationskoeffizient zu berechnen:
```
R2 = summary(trend)$r.squared
sqrt(R2)
```

# Multiple lineare Regression

```
mydata.num <- mydata[, seq(2,9)]
colnames(mydata.num)[2] <- "Zufriedenheit"
summary(lm(Zufriedenheit ~ ., data = mydata.num))
```

# Linerität

Um zu verinfachen:
```
colnames(mydata)[3] <- "Zufriedenheit"
```
Erstellen wir einige Plots:
```
plot(mydata$`Log GDP per capita`, mydata$Zufriedenheit)
```

# Korrelationen zwischen unabhängigen Variablen

```
model <- lm(Zufriedenheit ~ ., data = mydata.num)
library(car)
vif(model)
```

# Normalität

Wir brauchen ein Paket:
```
install.packages("Hmisc")
library(Hmisc)
```
Jetzt erstellen wir die Plots:
```
par(mar = c(1, 1, 1, 1))
hist.data.frame(mydata.num)
```
Und testen:
```
lapply(mydata.num, shapiro.test)
```


# 4. Vergleich mit eurer Erwartung

<img width="840" alt="Bildschirmfoto 2025-01-09 um 11 40 16" src="https://github.com/user-attachments/assets/81a76705-4853-42bd-a12a-07b697193269" />

















