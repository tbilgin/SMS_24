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




Lese hier für mehr: http://cbdm-01.zdv.uni-mainz.de/~galanisl/danalysis/index.html
