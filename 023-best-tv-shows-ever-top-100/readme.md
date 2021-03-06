# A brief look to Hollywood's 100 Favorite TV Shows
Joshua Kunst  


```r
# http://faculty.cs.niu.edu/~hutchins/csci230/sorting.htm
```


http://www.hollywoodreporter.com/



```r
url <- "http://www.hollywoodreporter.com/lists/best-tv-shows-ever-top-819499/item/desperate-housewives-hollywoods-100-favorite-820451"

items <- html(url) %>% 
  html_nodes(".list--ordered__item")

length(items) == 100 # Yay!
```

```
## [1] TRUE
```

```r
data <- ldply(items, function(item){
  # item <- sample(items, size = 1)[[1]]
  # item <- items[[100-78+1]]
  
  name <- item %>% html_node(".list-item__title") %>% html_text()
  
  info <- item %>% html_node(".list-item__deck") %>% html_text()
  
  years <- str_extract_all(info, "\\d+") %>% unlist() %>% as.numeric()
  
  company <- str_replace(info, "\\(.*\\)", "") %>% str_trim()
  
  if (name == "The Mary Tyler Moore Show") name <- "Mary Tyler Moore"
  if (name == "The Office (U.S.)") name <- "The Office"
  if (name == "Sherlock (U.K.)") name <- "Sherlock"
  if (grepl("With Children", name)) name <- "Married with Children"
  
  
  imdbdata <- httr::GET("http://www.omdbapi.com/",
                        query = list(t = name, y = years[1], type = "series")) %>%
    content() %>%
    as.data.frame()
  
  main_genre <- str_split(imdbdata$Genre, "\\,") %>% unlist() %>% .[1]
  
  dsummary <- data_frame(name, from = years[1], to = years[2], company, main_genre) 
  
  dataserie <- cbind(dsummary, imdbdata)
  
  dataserie
  
}, .progress = "win")

names(data) <- tolower(names(data))

data <- tbl_df(data) %>% 
  mutate(place = rev(seq(100)),
         to = ifelse(is.na(to), 2005, to)) %>% 
  arrange(place) 


ggplot(data) +
  geom_segment(aes(x = to, xend  = from, y = place, yend = place, group = place, color = main_genre),
               size = 2, alpha = 0.75,  lineend = "round") +
  scale_color_hc() + 
  theme_minimal() +
  theme(legend.position = "bottom")
```

![](readme_files/figure-html/unnamed-chunk-3-1.png) 

```r
data %>% sample_n(1) %>% select(imdbid)
```



imdbid    
----------
tt2193021 

```r
data %>% filter(imdbid == "tt0098936")
```



name          from     to  company   main_genre   title        year        rated   released      runtime   genre                      director   writer                    actors                                                           plot                                                                                                                     language                                   country   awards                                                   poster                                                                                             metascore   imdbrating   imdbvotes   imdbid      type     response    place
-----------  -----  -----  --------  -----------  -----------  ----------  ------  ------------  --------  -------------------------  ---------  ------------------------  ---------------------------------------------------------------  -----------------------------------------------------------------------------------------------------------------------  -----------------------------------------  --------  -------------------------------------------------------  -------------------------------------------------------------------------------------------------  ----------  -----------  ----------  ----------  -------  ---------  ------
Twin Peaks    1990   1991  ABC       Drama        Twin Peaks   1990–1991   TV-MA   08 Apr 1990   47 min    Drama, Mystery, Thriller   N/A        Mark Frost, David Lynch   Kyle MacLachlan, Michael Ontkean, Mädchen Amick, Dana Ashbrook   An idiosyncratic FBI agent investigates the murder of a young woman in the even more idiosyncratic town of Twin Peaks.   English, Icelandic, Afrikaans, Norwegian   USA       Won 3 Golden Globes. Another 11 wins & 43 nominations.   http://ia.media-imdb.com/images/M/MV5BMTExNzk2NjcxNTNeQTJeQWpwZ15BbWU4MDcxOTczOTIx._V1_SX300.jpg   N/A         8.9          93,786      tt0098936   series   True           20

```r
# http://www.myapifilms.com/imdb?idIMDB=tt0460649&format=JSON&aka=0&business=0&seasons=1&seasonYear=0&technical=0&lang=en-us&actors=N&biography=0&trailer=0&uniqueName=0&filmography=0&bornDied=0&starSign=0&actorActress=0&actorTrivia=0&movieTrivia=0&awards=0&moviePhotos=N&movieVideos=N&similarMovies=0
```


---
title: "readme.R"
author: "jkunst"
date: "Wed Sep 23 11:15:02 2015"
---
