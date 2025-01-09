# Logistische Regression
## Pakete installieren 
```
install.packages("mlbench")
library(mlbench)

install.packages('tidyverse')
library(tidyverse)
```
## Datensatz

Dieser Datensatz stammt ursprünglich vom National Institute of Diabetes and Digestive and Kidney Diseases. Ziel des Datensatzes ist es, anhand bestimmter diagnostischer Messungen, die im Datensatz enthalten sind, diagnostisch vorherzusagen, ob ein Patient an Diabetes erkrankt oder nicht. Die Auswahl dieser Fälle aus einer größeren Datenbank wurde mehreren Einschränkungen unterworfen. Insbesondere sind hier alle Patientinnen mindestens 21 Jahre alt und stammen aus Pima-Indianern.
Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. In Proceedings of the Symposium on Computer Applications and Medical Care (pp. 261--265). IEEE Computer Society Press.

Hier ist die [Beschreibung](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database).
```
data("PimaIndiansDiabetes2", package = "mlbench")
head(PimaIndiansDiabetes2)
glimpse(PimaIndiansDiabetes2)
```
## Datenbereinigung
```
diabetes.data <- na.omit(PimaIndiansDiabetes2)
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  head()
```
```
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  glimpse()
```
## Erste Figur
```
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  ggplot(aes(glucose, prob)) +
  geom_point(alpha = 0.2)
```
## Modellbildung
```
model <- glm( diabetes ~., data = diabetes.data, family = binomial)
summary(model)
```
## Regressionsfigur
```
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  ggplot(aes(glucose, prob)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "binomial")) +
  labs(
    x = "Glukose",
    y = "Diabetes"
  )
```
## Resultate
```
exp(summary(model)$coeff[,1])
```

# Hausaufgabe: Darstellung der Regressionen

Wir haben im Unterricht die Regression für Glukose gezeichnet. Jetzt bitte zeichne die Plots auch für die Körper-Gewicht Index und Familiengeschichte und antworte die folgenden Fragen:
- Wie anders sehen die Graphs aus?
- Wo siehst du die Gewichte der Punkte?
- Wie sind die Kurven? Welche sind steiler?
- Wie gross sind die Abweichungen?
- Jetzt vergleiche die p-Werte und Odds Ratio (Quotenverhältnis) für diese drei Variable.
Wir werden diese im Unterricht diskutieren.

```
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  ggplot(aes(mass, prob)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "binomial")) +
  labs(
    x = "Körper-Gewicht-Index",
    y = "Diabetes"
  )
```
<img width="510" alt="image" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/7a687b93-838c-47ac-b692-c698d9c67ccd">

```
diabetes.data %>%
  mutate(prob = ifelse(diabetes == "pos", 1, 0)) %>%
  ggplot(aes(pedigree, prob)) +
  geom_point(alpha = 0.2) +
  geom_smooth(method = "glm", method.args = list(family = "binomial")) +
  labs(
    x = "Familiengeschichte",
    y = "Diabetes"
  )
```
<img width="516" alt="image" src="https://github.com/tbilgin/DataScienceCourse/assets/26571015/af7bb53d-1210-42d2-82dd-ebf4af8ffcb7">





