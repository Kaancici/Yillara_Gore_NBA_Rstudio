 # NBA'nin Yıllara Göre Değişimi Veri Setinin Görselleştirlimesi

 Bu proje, Eskişehir Teknik Üniversitesi İstatistik Bölümü lisans programında 2023-2024 Güz döneminde yürütülen Veri Görselleştirme dersi kapsamında gerçekleştirilen bir çalışmadır. Bu projede, NBA üzerine odaklanılarak, yıllara göre ligdeki belirli istatistiklerin değişimini incelemek amacıyla iki farklı veri seti kullanılmıştır. Bu veri setleri, NBA oyuncularının atış verileri ve sezonluk istatistikleri üzerinedir.
Projenin temel odak noktası, NBA'deki istatistiksel eğilimleri yıllar itibariyle anlamak ve bu değişimleri etkileyen faktörleri görsel olarak ortaya koymaktır. İlk veri seti, oyuncuların atışlarına ilişkin atış lokasyonu, atış türü ve başarısı gibi bilgiler içerirken, ikinci veri seti ise sezonluk istatistikleri, yani maç sayısı, gerçek atış yüzdesi, asist oranları, blok oranları, oyunda kalma süresi, kazanma yüzdesi gibi önemli değişkenleri içermektedir.
Bu projede kullanılan veri setleriyle veri görselleştirme dersinde gördüğümüz görselleştirme teknikleri kullanılarak yapılmıştır.

## Veri Setleri

NBA Atış lokasyonu ve istatistiklerini 2004-2023 yılları arasını içeriyor. 2. veri setimiz ise 2011-2023 yılları arası oyuncuların sezon içi istatistiklerini veren bir veri setimizdir. 
Bu projede;

• Performans hangi istatistiklerle ilişkilidir, scatterplot ile karşılaştırıldı.

• Pozisyonlara göre yıllık atış yüzdeleri karşılatırılması, Streamgraph ile karşılaştırıldı.

• Yıllara göre 3'lük sayıların yüzdeleri ile 2'lik sayıların yüzdeleri barchart ile karşılaştırıldı .

• Yıllara göre oyuncuların ofensif performansları karşılaştırması violinchart ile yapıldı.

• Yıllara göre oyuncuların defansif performansları karşılaştırması violinchart ile yapıldı.

## Paketler
```
install.packages("devtools")
devtools::install_github("hrbrmstr/streamgraph")
library(devtools)
library(streamgraph)
install.packages("ggplot2")
library(ggplot2)
install.packages("dplyr")
library(dplyr)
install.packages("RColorBrewer")
library(RColorBrewer)
install.packages("tidyr")
library(tidyr)
install.packages("highcharter")
library(highcharter)
install.packages("plotly")
library(plotly)
install.packages("vioplot")
library(vioplot)
install.packages("leaflet")
library(leaflet)
install.packages("leaflet.extras")
library(leaflet.extras)
install.packages("gridExtra")
library(gridExtra)
install.packages("shiny")
library(shiny)

```
## Grafiklerimiz ve Yorumları
### 1) Performans hangi istatistiklerle ilişkilidir, scatterplot 
```

 plot1 <- ggplot(NBAson2, aes(x = G, y = PER)) +
  geom_point(size = 2, color = '#01408d', alpha = 0.5) +
  labs(title = 'Maç Sayısı ve Performans ',
       x = 'Oynanan Maç Sayısı',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))

plot2 <- ggplot(NBAson2, aes(x = TS, y = PER)) +
  geom_point(size = 2, color = '#e9062a', alpha = 0.5) +
  labs(title = 'Gerçek Atış (%) ve Performans',
       x = 'Gerçek Atış (%)',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))

plot3 <- ggplot(NBAson2, aes(x = AST, y = PER)) +
  geom_point(size = 2, color = '#01408d', alpha = 0.5) +
  labs(title = 'Asist (%) ve Performans',
       x = 'Asist (%)',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))


plot4 <- ggplot(NBAson2, aes(x = BLK, y = PER)) +
  geom_point(size = 2, color = '#e9062a', alpha = 0.5) +
  labs(title = 'Bloklama (%) ve Performans',
       x = 'Bloklama (%)',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))


plot5 <- ggplot(NBAson2, aes(x = MP, y = PER)) +
  geom_point(size = 2, color = '#01408d', alpha = 0.5) +
  labs(title = 'Oyunda Kalma Süresi ve Performans ',
       x = 'Oyunda Kalma Süresi',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))


plot6 <- ggplot(NBAson2, aes(x = WS, y = PER)) +
  geom_point(size = 2, color = '#e9062a', alpha = 0.5) +
  labs(title = 'Kazanma (%) ve Performans ',
       x = 'Kazanma (%)',
       y = 'Performans') +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))

grid.arrange(plot1, plot2, plot3, plot4, plot5, plot6, ncol = 3)
```

![2e1537f8-5728-448e-8cd4-ddd0bf12ad30](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/93273d89-bff1-44ef-957c-789ba5be20ec)

Asist yüzdesi ile performans arasında pozitif bir ilişki gözlenmektedir, yüksek asist yüzdesine sahip oyuncular genellikle yüksek performans skorlarına sahiptir.
Bloklama yüzdesi ile performans  arasında da benzer bir ilişki vardır; daha yüksek bloklama yüzdesine sahip oyuncular genellikle daha yüksek performans skorlarına sahiptir.
Oyunda kalma süresi ile performans arasında bir eğilim gözlenmektedir; daha uzun süre sahada kalan oyuncular genellikle daha yüksek performans  skorlarına sahip olabilir, bu da oyuncuların performanslarını uzun süre sürdürebildiğini gösterir.  
Kazanma yüzdesi ile performans arasında genel bir eğilim vardır; takımının daha yüksek bir kazanma yüzdesine sahip olduğu oyuncular genellikle daha yüksek performans skorlarına sahiptir, bu da oyuncuların takımlarının başarısına olumlu bir katkı sağladığını gösterir.


### 2)Pozisyonlara göre yıllık atış yüzdeleri karşılatırılması, Streamgraph
```
ui <- fluidPage(
  titlePanel("Pozisyonlara Göre Yıllık Ortalama Sayı Grafiği"),  
  mainPanel(
    streamgraphOutput("myStreamgraph")
  )
)
server <- function(input, output) {
  data_cleaned <- NBAson2 |> filter(!is.na(count) & !is.na(year) & !is.na(Pos) & !is.na(PER))
  
  position_avg_points <- data_cleaned |>
    group_by(year, Pos) |>
    summarize(avg_points = mean(as.numeric(PER), na.rm = TRUE))
  
  position_avg_points <- position_avg_points[complete.cases(position_avg_points), ]
  
  all_years <- expand.grid(year = unique(position_avg_points$year), Pos = unique(position_avg_points$Pos))
  
  position_avg_points <- merge(all_years, position_avg_points, by = c("year", "Pos"), all.x = TRUE)
  
  output$myStreamgraph <- renderStreamgraph({
    streamgraph(position_avg_points, key = "Pos", value = "avg_points", date = "year", interpolate = "basis", width = 1000, height = 500)
  })
}
shinyApp(ui, server)
```
![Screenshot 2024-01-05 095353](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/05c39bed-a01f-42d8-a08b-07153933e7e2)

Grafikte C oyuncuları, 2011-2015 yılları arasında takımların skor yükünü taşırken, bu dönemden sonra skor üretme oranları düşmüştür. Ancak, yenilenen NBA anlayışıyla birlikte, C oyuncuları yeniden skor açısından takımın önemli bir parçası haline gelmiştir.
 Aynı şekilde, 2011-2015 yılları arasında kanat oyuncularının skor üretme oranları ve çok yönlülükleri önemsizken, bu dönemden sonra bu oyuncular skor üretme oranlarını artırmış ve çok yönlülükleri ile takımlara önemli katkılar sağlamaya başlamıştır.
  Eskiden PG ve SG pozisyonlarında oynayan oyuncular, guard pozisyonları gereği çok skorer olamamışlardır. Ancak, yenilenen NBA anlayışıyla birlikte, PG ve SG oyuncuları da skor üretme oranlarını artırmıştır.


### 3)Yıllara göre 3'lük sayıların yüzdeleri ile 2'lik sayıların yüzdeleri barchart
```
colors <- c("#01408d", "#e9062a")

merged_data <- NBA_2004_2023_Shots %>%
  filter(SEASON_1 >= 2011) %>%
  group_by(SEASON_1, SHOT_TYPE) %>%
  summarise(topat = sum(SHOT_MADE)) %>%
  spread(SHOT_TYPE, topat, fill = 0)

tidy_data <- reshape2::melt(merged_data, id.vars = "SEASON_1", variable.name = "Shot_Type", value.name = "Shots_Made")
tidy_data$SEASON_1 <- as.factor(tidy_data$SEASON_1)

ggplot(tidy_data, aes(x = SEASON_1, y = Shots_Made, fill = Shot_Type)) +
  geom_bar(stat = "identity", position = "fill") +
  labs(title = "NBA Atış Analizi",
       x = "Sezon",
       y = "Toplam Yapılan Atışlar",
       fill = "Atış Türü") +
  scale_fill_manual(values = colors, name = "Atış Türü", labels = c("3 Sayılık Atış Yüzdesi", "2 Sayılık Atış Yüzdesi")) +  # Translate legend labels and set colors
  scale_y_continuous(labels = scales::percent_format(scale = 100)) +
  theme_minimal() +
  theme(
    legend.text = element_text(size = 12, face = "bold"),  
    legend.title = element_text(size = 14, face = "bold"),  
    axis.text = element_text(size = 12),  
    axis.title = element_text(size = 14, face = "bold"), 
    plot.title = element_text(size = 16, face = "bold")  
  )+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))
```
![cb3b6fd0-6ec0-4a2d-8a3c-1966a51bfce2](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/cad8c7f4-1eea-44ca-bc06-5273025d1ec1)

 NBA'deki üç sayı trendi, ligin temel dinamiklerini kökten değiştiren önemli bir faktör olarak öne çıkmaktadır. Bu eğilim, hem hücum stratejilerini hem de savunma taktiklerini, oyuncu profillerini ve oyunun genel yapısını etkileyerek bir dizi önemli değişikliğe yol açmıştır.  Grafikten de açıkça görülebileceği gibi,   NBA'de üç sayı kullanımı son yıllarda hızla artış göstermiştir. 2011 yılında, toplam üç sayı atışlarının oranı %33,7 iken, bu oran 2022 yılında %41,1'e yükselmiştir. Bu istatistik, ligdeki takımların ve oyuncuların üçlük atışlarına daha fazla odaklandığını ve bu alanda daha etkili olduklarını göstermektedir. 
Bu değişiklik, hem hücum hem de savunma stratejilerinde adapte olmayı gerektirmiş, takımların oyuncu kadrolarını ve oyun planlarını revize etmelerine neden olmuştur. Ayrıca, oyuncu profillerinde de belirgin bir değişiklik gözlemlenmiş, uzak mesafeden etkili şut atan oyuncuların talebi ve değeri artmıştır. 
Sonuç olarak, NBA'deki üç sayı trendi, ligi daha hızlı, daha uzak mesafeden şut atan ve genel oyun stratejilerini değiştiren bir yapıya evrilmiştir. Bu değişim, basketbolun evriminde önemli bir kilometre taşı olarak kabul edilebilir.


### 4)Yıllara göre oyuncuların ofensif performansları karşılaştırması violinchart
```
data <- NBAson2
mean_data <- data |>
  group_by(year) |>
  summarise(mean_OWS = mean(OWS))
ggplot(data, aes(x = OWS, y = factor(year))) +  # Swap x and y axes
  geom_violin(trim = FALSE, fill = "#e9062a") +
  geom_boxplot(width = 0.2, fill = "white", color = "black") +
  geom_point(data = mean_data, aes(x = mean_OWS, y = factor(year)), color = "darkred", size = 3) +
  labs(title = "Oyuncuların Yıllara Göre Ofansif Performansı Karşılaştırması",
       x = "Yıllara Göre Ofansif Performansları",
       y = "Yıl") +
  theme(axis.title.x = element_text(size = 12),
        axis.title.y = element_text(size = 12),
        axis.text.x = element_text(size = 10),
        axis.text.y = element_text(size = 10),
        plot.title = element_text(size = 25)) +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))
```

![32084d01-4418-4244-bc6d-191980606b87](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/87b87d03-0472-4d0b-8a1c-c881063a0deb)

   Grafikten görülebileceği gibi, NBA'de oyuncuların ofansif performansları, yıllara göre ortalamaya yaklaşmıştır. Bu durum, oyuncuların ofansif yeteneklerinin arttığını göstermektedir.

### 5)Yıllara göre oyuncuların defansif performansları karşılaştırması violinchart
```
data <- NBAson2
mean_data <- data |>
  group_by(year) |>
  summarise(mean_DWS = mean(DWS))
ggplot(data, aes(x = DWS, y = factor(year))) +  # Swap x and y axes
  geom_violin(trim = FALSE, fill = "#01408d") +
  geom_boxplot(width = 0.2, fill = "white", color = "black") +
  geom_point(data = mean_data, aes(x = mean_DWS, y = factor(year)), color = "darkred", size = 3) +
  labs(title = "Oyuncuların Yıllara Göre Defansif Performansı Karşılaştırması",
       x = "Yıllara Göre Defansif Performansları",
       y = "Yıl") +
  theme(axis.title.x = element_text(size = 12),
        axis.title.y = element_text(size = 12),
        axis.text.x = element_text(size = 10),
        axis.text.y = element_text(size = 10),
        plot.title = element_text(size = 25)) +
  theme_bw()+
  theme(axis.text = element_text(size = 15),
        axis.title = element_text(size = 15))

![98fd6d84-398f-4699-a49b-b08a7334c608](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/5e01c3af-3c32-4fdc-a69f-f0caa145e459)

  Grafikten görülebileceği gibi, NBA'de oyuncuların defansif performansları, yıllara göre ortalamaya yaklaşmıştır. Bu durum, oyuncuların defansif yeteneklerinin arttığını göstermektedir.
