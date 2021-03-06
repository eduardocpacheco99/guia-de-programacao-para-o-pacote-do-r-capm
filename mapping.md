



## Mapeando unidades amostrais

Para o desenho por conglomerados em dois estágios (ver seção anterior), quase sempre é necesario mapear as UPA para saber adonde devemos ir. Afortunadamente o `capm` tem uma função para mapear as UPA. Se temos um shapefile das UPA estamos feitos - como neste caso. Na área de trabalho há cinco arquivos chamados "santos", cada um com uma extensão diferente. Todos esses arquivos são uma representação shapefile das UPA da área de amostragem (cidade de Santos). Esses arquivos também foram obtidos no IBGE (ver seção anterior).


```r
> MapkmlPSU(shape = 'santos',
+           psu = pilot.psu[, 1],
+           id = 1)
```

`MapkmlPSU` cria um arquivo "kml" para cada UPA selecionada e um "kml" com todas as UPA selecionadas. Esses arquivos podem ser abertos com Google Earth apenas cliando sobre os mesmos. [QGIS](http://qgis.org) é uma ferramenta de código aberto que também "renderiza" camadas de base para os arquivos "kml". 

É claro que o R nós permite plotar as localizações das UPA selecionadas. Não se preocupem se não entendem o seguente fragmento de código, pois é apenas outra alternativa para Google Earth e QGIS.

Se aparece o erro "503 Service Unavailable", devemos tentar mais tarde para ver se o servidor OSM volta a funcionar (ver a página de ajuda de `get_openstreetmap`).


```r
> # o pacote rgeos deve estar instalado.
> library(rgdal); library(broom); library(ggmap); library(ggsn)
> santos <- readOGR(dsn = '.', layer = 'santos')
```

```
OGR data source with driver: ESRI Shapefile 
Source: ".", layer: "santos"
with 666 features
It has 1 fields
```

```r
> santos.pilot <- santos[
+     as.character(santos@data[ , 1]) %in%
+         pilot.psu[ , 1], ]
> santos.pilot <- spTransform(
+     santos.pilot,
+     CRS('+init=epsg:4326'))
> santos.pilot@data$id <-
+     rownames(santos.pilot@data)
> santos.pilot.points <- tidy(santos.pilot)
> santos.pilot.df <- merge(santos.pilot.points,
+                          santos.pilot@data,
+                          by = 'id')
> 
> osm.all.psu <- get_openstreetmap(
+     bbox = c(-46.386, -23.991, -46.298, -23.929),
+     scale = 34000, color = 'bw')
> 
> ggmap(osm.all.psu, extent = 'device') + 
+     geom_polygon(
+         data = santos.pilot.df,
+         aes(x = long, e = lat, fill = PSU)) +
+     coord_equal() +
+     geom_path(data = santos.pilot.df,
+               aes(long, lat, group = group),
+               color = 'yellow', size = 1.2) +
+     scalebar(santos.pilot.df, 'bottomleft',
+              dist = 1, dd2km = T,
+              model = 'WGS84', st.size = 3) +
+     north(santos.pilot.df, symbol = 15)
```

![plot of chunk map_all_psu](figures/map_all_psu-1.png)

Sem importar o método usado para produzir os mapas, devemos desenhar um percurso no mapa de cada UPA para poder ir por todas as ruas. Podemos definir um domicílio em um ponto arbitrário (localização inferior esquerda) como o primeiro domicílio e a partir do mesmo, podemos seguir o percurso contando os domicílios (incluindo os dois lados dos fragmentos de rua totalmente contidos na UPA).  

O seguinte mapa mostra a quarta UPA selecionada.


```r
> osm.psu4 <- get_openstreetmap(
+     bbox = c(-46.349, -23.962, -46.345, -23.957),
+     scale = 5000)
> ggmap(osm.psu4) +
+     geom_polygon(data = santos.pilot[4, ],
+                  aes(x = long, e = lat),
+                  fill = NA,
+                  color = 'yellow', size = 2) +
+     coord_equal()
```

```
Regions defined for each Polygons
```

![plot of chunk map_4th_psu](figures/map_4th_psu-1.png)
