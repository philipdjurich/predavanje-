# predavanje-
Statistika vježba
```{r}

install.packages(c('tinytex','rmarkdown'))
install.packages("tidyverse")
install.packages("rvest")
install.packages("xml2")
```

```{r}

library(tidyverse)
library(rvest)
library(xml2)
library(ggplot2)
library(rmarkdown)
```

```{r}

url <- "https://www.imdb.com/search/title/?count=100&release_date=2015,2015&title_type=feature"

webpage <- read_html(url)

```
```{r}
rank_data_html <- html_nodes(webpage,'.text-primary')
rank_data <- html_text(rank_data_html)
head(rank_data)


```
```{r}
rank_data<-as.numeric(rank_data)
head(rank_data)

```
```{r}
title_data_html <- html_nodes(webpage,'.lister-item-header a')
title_data <- html_text(title_data_html)
head(title_data)
```
```{r}
description_data_html <- html_nodes(webpage,'.ratings-bar+ .text-muted')
description_data <- html_text(description_data_html)
head(description_data)
```
```{r}
description_data<-gsub("\n","",description_data)
head(description_data)
```
```{r}
runtime_data_html <- html_nodes(webpage,'.text-muted .runtime')
runtime_data <- html_text(runtime_data_html)
head(runtime_data)
```
```{r}
runtime_data<-gsub(" min","",runtime_data)
runtime_data<-as.numeric(runtime_data)
head(runtime_data)
```
```{r}
genre_data_html <- html_nodes(webpage,'.genre')
genre_data <- html_text(genre_data_html)
head(genre_data)
```
```{r}
genre_data<-gsub("\n","",genre_data)
genre_data<-gsub(" ","",genre_data)
genre_data<-gsub(",.*","",genre_data)
genre_data<-as.factor(genre_data)
head(genre_data)
```
```{r}
rating_data_html <- html_nodes(webpage,'.ratings-imdb-rating strong')
rating_data <- html_text(rating_data_html)
head(rating_data)
```
```{r}
rating_data<-as.numeric(rating_data)
head(rating_data)
```

```{r}
votes_data_html <- html_nodes(webpage,'.sort-num_votes-visible span:nth-child(2)')
votes_data <- html_text(votes_data_html)
head(votes_data)
```

```{r}
votes_data<-gsub(",","",votes_data)
votes_data<-as.numeric(votes_data)
head(votes_data)
```
```{r}
directors_data_html<- html_nodes(webpage,'.text-muted+ p a:nth-child(1)')
directors_data<- html_text(directors_data_html)
head(directors_data)

```
```{r}
directors_data<-as.factor(directors_data)
actors_data_html <- html_nodes(webpage,'.lister-item-content .ghost+ a')
actors_data <- html_text(actors_data_html)
head(actors_data)
```


```{r}
actors_data<-as.factor(actors_data)

```


```{r}
metascore_data_html <- html_nodes(webpage,'.metascore')
metascore_data <- html_text(metascore_data_html)
head(metascore_data)

```
```{r}
metascore_data<-gsub(" ","",metascore_data)
length(metascore_data)
```
```{r}
for (i in c(71,95)){
  a<-metascore_data[1:(i-1)]
  b<-metascore_data[i:length(metascore_data)]
  metascore_data<-append(a,list("NA"))
  metascore_data<-append(metascore_data,b)
}
metascore_data<-as.numeric(metascore_data)
```
```{r}
length(metascore_data)
```
```{r}
summary(metascore_data)

```
```{r}
gross_data_html <- html_nodes(webpage,'.ghost~ .text-muted+ span')
gross_data <- html_text(gross_data_html)
head(gross_data)
```

```{r}
gross_data<-gsub("M","",gross_data)
gross_data<-substring(gross_data,2,6)
length(gross_data)
```
```{r}
gross_start<-NA
gross_end<-NA
gross_data<-append(gross_start,gross_data)
gross_data<-append(gross_data,gross_end)
gross_data<-as.numeric(gross_data)

for (i in c(1,38,70,74,80,95,99)){
  a<-gross_data[1:(i-1)]
  b<-gross_data[i:length(gross_data)]
  gross_data<-append(a,list("NA"))
  gross_data<-append(gross_data,b)
}
gross_data<-as.numeric(as.character(gross_data))
```
```{r}
length(gross_data)
```


```{r}
summary(gross_data)
```

```{r}
movies_df<-data.frame(Rank = rank_data, Title = title_data,
Description = description_data, Runtime = runtime_data,
Genre = genre_data, Rating = rating_data,
Metascore = metascore_data, Votes = votes_data,Director = directors_data, Actor = actors_data,Gross_Earning_in_Mil = gross_data )

str(movies_df)

