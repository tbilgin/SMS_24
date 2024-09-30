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
Um den Korrelationskoeffizient zu berechnen:
```
R2 = summary(trend)$r.squared
sqrt(R2)
```













