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

# 3. Tidyverse

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

# 4. Lineare Regression

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



















