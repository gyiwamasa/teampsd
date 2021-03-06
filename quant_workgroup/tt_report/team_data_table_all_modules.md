---
title: "Team Data Table"
author: "Savet Hong"
date: "`r format(Sys.Date(), '%b %d, %Y')`"
output: html_document

---
  
To use the code in this Rmarkdown for each Module in separate Rmarkdown, please do as follow:
  
- ensure that all library packages (as listed in the library chunk) are installed
- copy chuck containing R libraries 
- copy section of code for each module
+ setup for each module
+ table with and/or without definition
- include "ModelParameters.xlsx" in the same folder, if not then change file pathway to the data source    

```{r library, include=FALSE}

library(readxl)
library(dplyr)
library(tibble)
library(tidyr)
library(ggplot2)
library(htmlTable)

```

## Care Coordination Module


```{r cc_setup, include=FALSE}
ccpar <- read_excel("ModelParameters.xlsx", sheet = "CCParams", col_names = FALSE)
ccpar.table <- ccpar[c(12,10,5,3,1,7),1:3] %>% # Select row and order to match UI
  mutate(X__2 = formatC(X__2, digits = 2, format = "f"),
         X__1 = sub(" in CC", "", X__1))


```

#### CC Table without definition (similar to the UI)

```{r cc_table_NOdef, echo=FALSE}

htmlTable(ccpar.table[,-3], header = c("",""), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lr")
```

#### CC Table with definition

```{r cc_table_def, echo=FALSE}
htmlTable(ccpar.table, header = c("","",""), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lrl")
```

## Medication Management Module


```{r mm_setup, include=FALSE}
mmpar <- read_excel("ModelParameters.xlsx", sheet = "MMParams", col_names = FALSE, range = "A1:F16")

mmpar.table1 <- mmpar[c(10, 15, 12, 5, 3, 1, 13, 14), c(1,3,2,4:6)] 
# Missing "Target Wait Time"

mmpar.table <- mmpar.table1 %>%
  mutate(X__5 = replace(X__5, X__1 == "Appointment Supply (75th percentile)", 
                        X__2[which(X__1 == "Appointment Supply (75th percentile)")]),
         X__2 = replace(X__2, X__1 == "Appointment Supply (75th percentile)", NA),
         X__3 = replace(X__3, X__1 == "Appointment Supply (75th percentile)", NA),
         X__4 = replace(X__4, X__1 == "Appointment Supply (75th percentile)", NA),
         X__5 = replace(X__5, X__1 == "Appointment Slots % (with X waiver)",
                        X__2[which(X__1 == "Appointment Slots % (with X waiver)")]),
         X__2 = replace(X__2, X__1 == "Appointment Slots % (with X waiver)", NA),
         X__1 = sub(" in MM", "", X__1)) %>%
  mutate_at(vars(2:5), funs(formatC(as.numeric(.),digits = 2, format = "f"))) %>%
  mutate_at(vars(2:5), funs(replace(., . == " NA", ""))) %>%
  add_row(X__3 = "AUD", X__2 = "DEP", X__4 = "OUD", X__5 = "OTHER", .before = 3)

```

#### MM Table without definition (similar to the UI)

```{r mm_table_NOdef, echo=FALSE}

htmlTable(mmpar.table[,-6], header = c(rep("",5)), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lrrrr")

```

#### MM Table with definition

```{r mm_table_def, echo=FALSE}

htmlTable(mmpar.table, header = c(rep("",6)), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lrrrrl")

```


## Aggregate Module

```{r agg_setup, include=FALSE}
aggpar <- read_excel("ModelParameters.xlsx", sheet = "AggParams", col_names = FALSE, range = "A1:I12")

#Table data setup
aggpar.table.head <- aggpar[c(6,2),]
aggpar.table.body <- aggpar[c(2,1,5,3,4),] %>%
  mutate_at(vars(2:8), funs(formatC(as.numeric(.),digits = 2, format = "f"))) %>%
  rename(Intake = X__2, Psy = X__3, EBPsy = X__4, CC = X__5, MM = X__6, 
         Adjunctive = X__7, Group = X__8, Variable = X__1, Definition = X__9)

aggpar.table.body.shrt <- t(aggpar.table.body[,2:8]) 
colnames(aggpar.table.body.shrt) <- aggpar.table.body$Variable

aggpar.test <- as.data.frame(aggpar.table.body.shrt) %>%
  mutate(X__1 = row.names(.)) %>%
  select(X__1, everything()) %>%
  mutate_if(is.factor,funs(as.character)) %>%
  add_row(X__1 = as.character(aggpar[6,1]), `Appointment Supply (median)` =
            as.character(round(as.numeric(aggpar[6,2]), 1)), 
          `True Missed Appointment %` = "pts/wk",.before = 1) %>%
  add_row(`Appointment Supply (median)` = "Appointment Supply (median)", 
          `True Missed Appointment %` =
            "True Missed Appointments %",`Return Visit Interval (median)` = 
            "Return Visit Interval (median)",
          `Median Engagement` = "Median Engagement", 
          `Service Proportions from Team Data` = "Service Proportions from Team Data", .before = 2) 


#Table definition
agg_def <- aggpar[c(6,2,1,5,3,4),c(1,9)]
```

#### Aggregate Table without definition (similar to the UI)

```{r agg_table, echo=FALSE}

htmlTable(aggpar.test, header = c(rep("",6)), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lrrrr")

```
#### Aggregate Table Definition

```{r agg_table_def, echo=FALSE}
htmlTable(agg_def, header = c("Concept", "Definition"), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data: Aggregate Table Concept Definition", align = "ll")

```

## Psy Module

```{r psy_datafiles, include=FALSE}
psypar1a <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B35:D36")

psypar1b <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B44:D47")

flow <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B3:D7")

rvi <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B8:D12")

med <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B13:D14")

rvi_after <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B15:D23")

med_after <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B24:D32")
```

```{r psy_setup, include=FALSE}
# Team Data Table/Appointment Supply columns
ord1 <- c("Appointment Supply (75th percentile)", "AUD within 3 months %", "DEP within 3 months %",
          "OUD within 3 months %", "PTSD within 3 months %")
psypar1 <- rbind.data.frame(psypar1a[2,], psypar1b) %>%
  mutate_if(is.numeric, funs(round(.,1))) %>%
  mutate_if(is.numeric, funs(as.character)) %>%
  slice(match(ord1, X__1))

# psypar1$X__2[which(psypar1$X__1 == "Appointment Supply (75th percentile)")] <-  
#   paste0(psypar1$X__2[which(psypar1$X__1 == "Appointment Supply (75th percentile)")], 
#          " appt/wk")

#Patient Flow 
flow.order <- c("Starters Who Initiate %","Starters Who Return Later %", "Starters Who Quit %",
                "Initiators Who Complete %","Initiators Who Return Later %", 
                "Initiators Who Quit Early %", 
                "Completers Who Graduate %","Completers Who Return %")

## Need to calculate
## Starters who Quit
## Initiators who Return Later
## Completers who Return
flow1 <- flow %>%
  add_row(X__1 = "Starters Who Quit %") %>% 
  add_row(X__1 = "Initiators Who Return Later %") %>%
  add_row(X__1 = "Completers Who Return %") %>%
  mutate( X__2 = replace(X__2, X__1 == "Starters Who Quit %", 1 - 
                           (X__2[which(X__1 == "Starters Who Initiate %")] + 
                              X__2[which(X__1 == "Starters Who Return Later %")])),
          X__2 = replace(X__2, X__1 == "Initiators Who Return Later %", 1 - 
                           (X__2[which(X__1 == "Initiators Who Complete %")] + 
                              X__2[which(X__1 == "Initiators Who Quit Early %")])),
          X__2 = replace(X__2, X__1 =="Completers Who Return %", 1 - 
                           (X__2[which(X__1 == "Completers Who Graduate %")]))) %>%
  slice(match(flow.order, X__1)) %>%
  mutate(X__2 = paste0(round(X__2, 2)*100, "%"))

# Return Visit Interval less than 3months
rvi1 <- c("N/A", "See table below", "See table below",  
          round(rvi$X__2[which(rvi$X__1 == "Initiators Who Complete RVI")],2) ,
          "See table below",
          round(rvi$X__2[which(rvi$X__1 == "Initiators Who Quit Early RVI")],2), 
          round(rvi$X__2[which(rvi$X__1 == "Completers RVI")],2),
          "See table below")

# Median Engagement Duration less than 3months
med1 <- c("NA", "See table below", "See table below",
          round(med$X__2[which(med$X__1 == "Initiators Who Quit Early Duration")], 2),
          "See table below", 
          round(med$X__2[which(med$X__1 == "Initiators Who Graduate Duration")],2),
          "12 wks", "See table below")


# Return Visit Interval after 3months
visit.order <- c("From 1 Visit", "From 2 to 7 Visits", "From 8 or More Visits")
rvi2 <- rvi_after %>%
  mutate(Visit = sub("^(.*) Who .*", "\\1", X__1),
         X__3 = sub(".* (.*) RVI$", "\\1", X__1)) %>%
  select(-X__1) %>%
  spread(X__3, X__2) %>%
  mutate(Visit = replace(Visit, Visit =="Starters", "From 1 Visit"),
         Visit = replace(Visit, Visit =="Initiators", "From 2 to 7 Visits"),
         Visit = replace(Visit, Visit =="Completers", "From 8 or More Visits")) %>%
  slice(match(visit.order, Visit)) %>%
  select(Visit, Low, Medium, High) %>%
  rename(`1st Quartile` = Low, Mean = Medium, `4th Quartile` = High) %>%
  mutate_if(is.numeric, funs(round(.,2)))


# Median Engagement Duration after than 3months
med2 <- med_after %>%
  mutate(X__3 = sub(".* (.*) Duration$", "\\1", X__1),
         Visit = sub("^(.*) Who.*","\\1", X__1)) %>%
  select(-X__1) %>%
  spread(X__3, X__2) %>%
  mutate(Visit = replace(Visit, Visit =="Starters", "From 1 Visit"),
         Visit = replace(Visit, Visit =="Initiators", "From 2 to 7 Visits"),
         Visit = replace(Visit, Visit =="Completers", "From 8 or More Visits"))%>%
  slice(match(visit.order, Visit)) %>%
  select(Visit, Low, Medium, High) %>%
  #rename(`1st Quartile` = Low, Mean = Medium, `4th Quartile` = High) %>%
  mutate_if(is.numeric, funs(round(.,2)))


# Upper Table without Definition
psy1 <- psypar1[,-3] %>%
  rename(AppSupply = X__1, rate = X__2) %>%
  add_row(AppSupply = "", rate = "") %>%
  add_row(AppSupply = "", rate = "") %>%
  add_row(AppSupply = "", rate = "") %>%
  cbind(.,flow1[,-3], rvi1, med1)

# Lower Table without Definition
psy2 <- merge.data.frame(rvi2, med2, by = "Visit") %>%
  add_row(`1st Quartile` = "1st Quartile", Mean = "Mean", 
          `4th Quartile` = "4th Quartile", `Low` = "1st Quartile",
          `Medium` = "Mean", `High` = "4th Quartile", .before = 1)

```

```{r psy_datafiles2, include=FALSE}
vacolor <- c("#003F72", "#0083BE","#598527")
rvi_graph <- read_excel("ModelParameters.xlsx", sheet = "PSYParams", col_names = FALSE, range = "B15:C32")

rvi_mod <- rvi_graph %>%
  mutate(Type = ifelse(grepl("RVI", X__1), "Freq", "Time"),
        `Flow Group` = sub("(.*) Low|Medium|High", "\\1", X__1),
         `Flow Group` = sub("RVI", "Later", `Flow Group`),
         `Flow Group` = sub(" Duration", "", `Flow Group`),
         `Flow Group` = gsub("^ *|(?<= ) | *$", "", `Flow Group`, perl = TRUE),
         percentile = sub(".* (Low|Medium|High) .*", '\\1', X__1),
         percentile = replace(percentile, percentile == "Medium", "Median")) %>%
  rename( week = X__2) %>%
  select(Type,`Flow Group`, percentile, week)

eng <- rvi_mod %>%
  filter(Type == "Freq") %>%
  mutate(week = 1 / week,
         week = replace(week, week==Inf, 0),
         `Flow Group` = factor(`Flow Group`, levels = c("Starters Who Return Later",
                                                        "Initiators Who Return Later",
                                                        "Completers Who Return Later")),
         percentile = factor(percentile, levels = c("Low", "Median", "High")))


rvi_only <- rvi_mod %>%
  filter(Type == "Time") %>%
  mutate(`Flow Group` = factor(`Flow Group`, levels = c("Starters Who Return Later",
                                                        "Initiators Who Return Later",
                                                        "Completers Who Return")),
         percentile = factor(percentile, levels = c("Low", "Median", "High")))

```



```{r single,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center", include= FALSE}
## Option 1: Side-by-side

p1 <- ggplot(eng, aes(percentile, week, fill = percentile)) +
  geom_bar( stat = "identity", width = 1) +
  scale_fill_manual(values = vacolor, name=element_blank(), guide = FALSE) +
  facet_wrap(~`Flow Group`, strip.position = "bottom", scales = "free_x") +
  labs(x = "", y = "Weeks", title = "Return Visit Interval", #subtitle = "(Weeks Between Visit)",
       caption = "Low = 25th %ile, Median = 50th %ile, High = 75th %ile") +
  theme_bw() +
  theme(panel.spacing = unit(0, "lines"), 
         strip.background = element_blank(),
         strip.placement = "outside",
        strip.text.x = element_text(size = 11),
        axis.text.x = element_text( hjust = 1, colour = vacolor),
        plot.title =  element_text(hjust = 0.5))

p2 <- ggplot(rvi_only, aes(percentile, week, fill = percentile)) +
  geom_bar( stat = "identity", width = 1) +
  scale_fill_manual(values = vacolor, name=element_blank(), guide = FALSE) +
  facet_wrap(~`Flow Group`, strip.position = "bottom", scales = "free_x") +
  labs(x = "", y= "Weeks", title = "Duration of Engagement", subtitle = "(Weeks Between Visit)",
       caption = "Low = 25th %ile, Median = 50th %ile, High = 75th %ile") +
  theme_bw() +
  theme(panel.spacing = unit(0, "lines"), 
         strip.background = element_blank(),
         strip.placement = "outside",
        strip.text = element_text(size = 11),
        axis.text.x = element_text( hjust = 1, colour = vacolor),
        plot.title =  element_text(hjust = 0.5),
        plot.subtitle =  element_text(hjust = 0.5))

```



#### Psy Table without definition (similar to the UI)
```{r psy_table, echo=FALSE}
htmlTable(psy1, header = c(rep("",2), "Patient Flow", "", "Return Visit \nInterval (wks)", "Median Engagement \nDuration (wks)"), rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Data", align = "lllrcc")


htmlTable(psy2, header = c("", "Return Visit", "Interval after 3", "Months (wks)",
                           "Engagement", "Time after 3", "Months (wks)"),
          rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", 
          #caption ="Team Data", 
          align = "lcccccc" )

```


```{r p1,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center"}
p1
```

```{r p2,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center"}
p2
```

## Psy Module 
```{r library, include=FALSE}

library(readxl)
library(tidyverse)
library(plotly)

```


```{r psy_datafiles, include=FALSE}
vacolor <- c("#003F72", "#0083BE","#598527")
rvi <- read_excel("ModelParameters (2).xlsx", sheet = "PSYParams", col_names = FALSE, range = "A15:C32")

# Expand Column A as vector
unique_Appointment_Group <- rvi$X__1[which(!is.na(rvi$X__1))] #unique `Appointment Group` (in order)
Appointment_Group_pos <- diff(which(!is.na(rvi$X__1))) # position/rows between `Appointment Group`
max.pos <- length(unique_Appointment_Group) 
Appointment_Group_vec <- c(rep(unique_Appointment_Group[-max.pos], Appointment_Group_pos),
                 rep(unique_Appointment_Group[max.pos], 3)) # repeat values until next occurance 

rvi_mod <- rvi %>%
  mutate(`Appointment Group` = Appointment_Group_vec,
         Type = ifelse(grepl("Freq", X__2), "Freq", "Time"),
         `Appointment Group` = sub("vis", "visit", `Appointment Group`),
         `Appointment Group` = sub("27", "2-7", `Appointment Group`),
         `Flow Group` = sub("(.*) Low|Medium|High", "\\1", X__2),
         `Flow Group` = sub(" Frequency", "", `Flow Group`),
         `Flow Group` = sub(" Duration", "", `Flow Group`),
         `Flow Group` = sub("(\\s)$", "", `Flow Group`),
         percentile = sub(".* (Low|Medium|High) .*", '\\1', X__2),
         percentile = replace(percentile, percentile == "Medium", "Median")) %>%
  rename( week = X__3) %>%
  select(Type,`Flow Group`, percentile, week)

eng <- rvi_mod %>%
  filter(Type == "Freq") %>%
  mutate(week = 1 / week,
         week = replace(week, week==Inf, 0),
         `Flow Group` = factor(`Flow Group`, levels = c("Starters Who Come Back Later",
                                                        "Initiators Who Come Back Later",
                                                        "Completers Who Return")),
         percentile = factor(percentile, levels = c("Low", "Median", "High")))
  # mutate(`Flow Group` = gsub(" Frequency","", `Flow Group`),
  #        `Appointment Group` = gsub(" freq", "", `Appointment Group`))

rvi_only <- rvi_mod %>%
  filter(Type == "Time") %>%
  mutate(`Flow Group` = factor(`Flow Group`, levels = c("Starters Who Return Later",
                                                        "Initiators Who Return Later",
                                                        "Completers Who Return")),
         percentile = factor(percentile, levels = c("Low", "Median", "High")))

```



```{r single,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center", include= FALSE}
## Option 1: Side-by-side

p1 <- ggplot(eng, aes(percentile, week, fill = percentile)) +
  geom_bar( stat = "identity", width = 1) +
  scale_fill_manual(values = vacolor, name=element_blank(), guide = FALSE) +
  facet_wrap(~`Flow Group`, strip.position = "bottom", scales = "free_x") +
  labs(x = "", y = "Weeks", title = "Return Visit Interval", #subtitle = "(Weeks Between Visit)",
       caption = "Low = 25th %ile, Median = 50th %ile, High = 75th %ile") +
  theme_bw() +
  theme(panel.spacing = unit(0, "lines"), 
         strip.background = element_blank(),
         strip.placement = "outside",
        strip.text.x = element_text(size = 11),
        axis.text.x = element_text( hjust = 1, colour = vacolor),
        plot.title =  element_text(hjust = 0.5))

p2 <- ggplot(rvi_only, aes(percentile, week, fill = percentile)) +
  geom_bar( stat = "identity", width = 1) +
  scale_fill_manual(values = vacolor, name=element_blank(), guide = FALSE) +
  facet_wrap(~`Flow Group`, strip.position = "bottom", scales = "free_x") +
  labs(x = "", y= "Weeks", title = "Duration of Engagement", subtitle = "(Weeks Between Visit)",
       caption = "Low = 25th %ile, Median = 50th %ile, High = 75th %ile") +
  theme_bw() +
  theme(panel.spacing = unit(0, "lines"), 
         strip.background = element_blank(),
         strip.placement = "outside",
        strip.text = element_text(size = 11),
        axis.text.x = element_text( hjust = 1, colour = vacolor),
        plot.title =  element_text(hjust = 0.5),
        plot.subtitle =  element_text(hjust = 0.5))

cowplot::plot_grid(p1, p2)
```



```{r p1,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center"}
p1
```

```{r p2,echo= FALSE, warning=FALSE, message=FALSE, fig.align="center"}
p2
```

