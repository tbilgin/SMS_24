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
```

# Datenverarbeitung mit TidyR
```
install.packages('tidyverse')
library(tidyverse)
```

Hier ist ein Beispiel. 
```
mydata %>% count(Year, wt = Generosity)
```
Antworte die untene Fragen:
1) Wie ist diese Zeile benutzbar? Was könnte die Frage sein? Und die Hypothese?
2) Stelle zwei andere Fragen, die du mit tidyverse antworten kannst.
3) Was sind deine Hypothesen für deine Fragen?
4) Schreib dein Skript mit mindestens ein tidyverse Zeile per Hypothese
5) In welchen Aspekten findest du tidyverse hilfreich? Wir hatten zwei gennant in der Präsentation. Finde noch 1-2 ud erkläre warum basierend auf deine Skripte.



## Weitere Beispiele

Beschreibe was die untene Koden machen: 
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


Tip für die letzte Frage: Dank zu tidyverse haben wir hier unser Dataset mehr 'tidy' gemacht und subsetting, filtering etc waren dann sehr einfach zu kodieren, und auch lesbar ;)  


Lese hier für mehr: http://cbdm-01.zdv.uni-mainz.de/~galanisl/danalysis/index.html

