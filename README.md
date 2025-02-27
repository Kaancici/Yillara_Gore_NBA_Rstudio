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

colnames(NBAson2) <- gsub("%", "", colnames(NBAson2))
colnames(NBAson2)

remove_outliers <- function(data, column) {
  Q1 <- quantile(data[[column]], 0.25, na.rm = TRUE)
  Q3 <- quantile(data[[column]], 0.75, na.rm = TRUE)
  IQR_value <- Q3 - Q1
  lower_bound <- Q1 - 1.5 * IQR_value
  upper_bound <- Q3 + 1.5 * IQR_value
  return(data[data[[column]] >= lower_bound & data[[column]] <= upper_bound, ])
}

NBAson2 <- remove_outliers(NBAson2, "PER")  
NBAson2 <- remove_outliers(NBAson2, "TS")   
NBAson2 <- remove_outliers(NBAson2, "AST")  
NBAson2 <- remove_outliers(NBAson2, "BLK")
NBAson2 <- remove_outliers(NBAson2, "MP")  
NBAson2 <- remove_outliers(NBAson2, "WS")
NBAson2 <- remove_outliers(NBAson2, "OWS")
NBAson2 <- remove_outliers(NBAson2, "DWS")

plot1 <- ggplot(NBAson2, aes(x = G, y = PER)) +
  geom_density2d_filled() +  # Yoğunluk haritası
  labs(title = 'Oynanan Maç ve Performans',
       x = 'Oynanan Maç',
       y = 'Performans') +
  theme_bw()

plot2 <- ggplot(NBAson2, aes(x = TS, y = PER)) +
  geom_density2d_filled() +
  labs(title = 'Gerçek Atış ve Performans',
       x = 'Gerçek Atış',
       y = 'Performans') +
  theme_bw()

plot3 <- ggplot(NBAson2, aes(x = AST, y = PER)) +
  geom_density2d_filled() +
  labs(title = 'Asist ve Performans',
       x = 'Asist',
       y = 'Performans') +
  theme_bw()

plot4 <- ggplot(NBAson2, aes(x = BLK, y = PER)) +
  geom_density2d_filled() +
  labs(title = 'Bloklama ve Performans',
       x = 'Bloklama',
       y = 'Performans') +
  theme_bw()

plot5 <- ggplot(NBAson2, aes(x = MP, y = PER)) +
  geom_density2d_filled() +
  labs(title = 'Oyunda Kalma Süresi ve Performans',
       x = 'Oyunda Kalma Süresi',
       y = 'Performans') +
  theme_bw()

plot6 <- ggplot(NBAson2, aes(x = WS, y = PER)) +
  geom_density2d_filled() +
  labs(title = 'Kazanma ve Performans',
       x = 'Kazanma',
       y = 'Performans') +
  theme_bw()

grid.arrange(plot1, plot2, plot3, plot4, plot5, plot6, ncol = 3)
```

![Screenshot 2025-02-27 215620](https://github.com/user-attachments/assets/b6527847-44dd-4205-94e1-2617115559a9)

Oynanan maç sayısı ile performans arasında belirgin bir doğrusal ilişki bulunmamaktadır. Ancak sezon boyunca 50’den fazla maç oynayan oyuncuların performanslarının daha stabil ve yüksek olduğu gözlemlenmiştir. Daha az maç oynayan oyuncuların performans değişkenliği daha fazladır; bazı oyuncular az maçta yüksek verim gösterebilirken, bazıları ise düşük PER değerlerine sahip olabilir. Sezon boyunca istikrarlı oynayan oyuncular, genellikle daha güvenilir ve stabil bir performans sergilemektedir.

Gerçek atış yüzdesi (TS) ile performans arasında güçlü bir pozitif ilişki bulunmaktadır. Yüksek TS yüzdesine sahip oyuncuların PER değerleri genellikle daha yüksektir. TS %50-60 aralığında yoğunlaşan oyuncuların büyük bir kısmı PER 10-15 seviyelerinde bulunmaktadır. Buna karşılık TS %40’ın altında olan oyuncuların performansları genellikle düşük olup, bu da şut verimliliğinin bireysel performans üzerinde önemli bir etkisi olduğunu göstermektedir. Şut yüzdesi düşük olan oyuncuların PER değerleri genellikle düşük kalsa da, istisna olarak diğer alanlarda katkı sağlayan oyuncular da mevcuttur.

Asist yüzdesi ile performans arasında güçlü bir pozitif korelasyon gözlemlenmektedir. 5-10 asist arasında olan oyuncuların yoğunlukla PER 10-15 aralığında olduğu görülmektedir. Daha fazla asist yapan oyuncular genellikle takımın oyununu yönlendiren ve hücumda etkin olan oyuncular olduklarından, PER değerleri de yükselmektedir. Ancak skorer veya savunma odaklı olup düşük asist sayısına rağmen yüksek PER elde eden oyuncular da bulunmaktadır.

Blok oranı arttıkça PER değerleri de artış göstermektedir. Blok yüzdesi 0-1 arasında olan oyuncuların büyük çoğunluğu PER 10-15 aralığında yoğunlaşmışken, 2-4 blok ortalaması olan oyuncuların daha yüksek PER değerlerine sahip olduğu görülmektedir. Savunmada etkili olan ve boyalı alanda caydırıcılığı yüksek olan oyuncuların PER değerleri de genellikle yükselmektedir. Ancak, blok yapmayan oyuncuların da yüksek PER elde edebileceği unutulmamalıdır; bu oyuncular genellikle hücum katkılarıyla ön plana çıkmaktadır.

Oyunda kalma süresi ile performans arasında genel bir eğilim bulunmaktadır, ancak bu ilişki tamamen doğrusal değildir. Daha uzun süre sahada kalan oyuncuların genellikle daha yüksek PER değerlerine sahip olduğu gözlemlenmiştir. Yoğunluk merkezi 1000-2000 dakika arasında olup PER 10-15 seviyelerinde yoğunlaşmaktadır. Ancak, bazı oyuncular düşük süre almasına rağmen çok yüksek PER değerlerine ulaşabilmektedir. Bu durum, yedekten girip kısa sürede verimli oynayan oyuncuların varlığını göstermektedir.

Takımın kazanma yüzdesi ile bireysel performans arasında pozitif bir ilişki bulunmaktadır. WS değeri 2’nin üzerinde olan oyuncuların PER değerleri genellikle yüksek olup, kazanan takımlarda oynayan oyuncuların bireysel performansları da daha iyi olma eğilimindedir. Ancak, bireysel performansı yüksek olmasına rağmen kötü bir takımda oynayan oyuncular da mevcuttur. Bu nedenle, takım başarısı ve bireysel başarı her zaman paralel ilerlememektedir.

#### Genel Yorum
Genel olarak, asist yüzdesi, blok yüzdesi ve kazanma yüzdesi performans (PER) üzerinde en güçlü etkilere sahip metrikler olarak öne çıkmaktadır. Şut verimliliği (TS), bireysel performansı belirlemede önemli bir faktördür, çünkü daha verimli şut atan oyuncular genellikle daha yüksek PER değerlerine ulaşmaktadır. Daha uzun süre sahada kalan oyuncuların performansları genellikle daha yüksek olmasına rağmen, bu durum her zaman geçerli değildir. Özellikle yedekten girip yüksek verimle oynayan oyuncular düşük sürede bile yüksek PER elde edebilmektedir. Takım başarısı ile bireysel performans arasında pozitif bir korelasyon olsa da, bireysel olarak çok iyi performans gösteren ancak kötü bir takımda oynayan oyuncuların PER değerleri kazanma yüzdesinden bağımsız olarak yüksek olabilir. Sonuç olarak, bir oyuncunun performansını değerlendirirken tek bir istatistiğe odaklanmak yerine, şut verimliliği, asist, blok, süre ve takım başarısını birlikte değerlendirmek en doğru yaklaşımdır.


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
colors <- c("#e9062a","#01408d")

merged_data <- NBA_2004_2023_Shots |>
  filter(SEASON_1 >= 2011) |>
  group_by(SEASON_1, SHOT_TYPE) |>
  summarise(topat = sum(SHOT_MADE)) |>
  spread(SHOT_TYPE, topat, fill = 0)

tidy_data <- reshape2::melt(merged_data, id.vars = "SEASON_1", variable.name = "Shot_Type", value.name = "Shots_Made")
tidy_data$SEASON_1 <- as.factor(tidy_data$SEASON_1)

ggplot(tidy_data, aes(x = SEASON_1, y = Shots_Made, fill = Shot_Type)) +
  geom_bar(stat = "identity", position = "fill") +
  labs(title = "NBA Atış Analizi",
       x = "Sezon",
       y = "Toplam Yapılan Atışlar",
       fill = "Atış Türü") +
  scale_fill_manual(values = colors, name = "Atış Türü", labels = c("2 Sayılık Atış Yüzdesi","3 Sayılık Atış Yüzdesi")) +  # Translate legend labels and set colors
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
![113cbe48-6743-4a82-9334-92a821b7a058](https://github.com/Kaancici/yillara_gore_nba/assets/150475924/bd26ea85-c5fe-42b1-ae81-89e26c9f90be)



 NBA'deki üç sayı trendi, ligin temel dinamiklerini kökten değiştiren önemli bir faktör olarak öne çıkmaktadır. Bu eğilim, hem hücum stratejilerini hem de savunma taktiklerini, oyuncu profillerini ve oyunun genel yapısını etkileyerek bir dizi önemli değişikliğe yol açmıştır.  Grafikten de açıkça görülebileceği gibi,   NBA'de üç sayı kullanımı son yıllarda hızla artış göstermiştir. 2011 yılında, toplam üç sayı atışlarının oranı %33,7 iken, bu oran 2022 yılında %41,1'e yükselmiştir. Bu istatistik, ligdeki takımların ve oyuncuların üçlük atışlarına daha fazla odaklandığını ve bu alanda daha etkili olduklarını göstermektedir. 
Bu değişiklik, hem hücum hem de savunma stratejilerinde adapte olmayı gerektirmiş, takımların oyuncu kadrolarını ve oyun planlarını revize etmelerine neden olmuştur. Ayrıca, oyuncu profillerinde de belirgin bir değişiklik gözlemlenmiş, uzak mesafeden etkili şut atan oyuncuların talebi ve değeri artmıştır. 
Sonuç olarak, NBA'deki üç sayı trendi, ligi daha hızlı, daha uzak mesafeden şut atan ve genel oyun stratejilerini değiştiren bir yapıya evrilmiştir. Bu değişim, basketbolun evriminde önemli bir kilometre taşı olarak kabul edilebilir.


### 4)Yıllara göre oyuncuların ofensif performansları karşılaştırması violinchart
```
data <- NBAson2 %>%
 filter(!is.na(OWS))  
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

![Screenshot 2025-02-27 223955](https://github.com/user-attachments/assets/87af0d5f-758d-42ac-9611-268b0a8612ee)


   Grafik, NBA’de yıllar içinde ofansif performansın daha standart hale geldiğini ve oyuncular arasındaki farkların azaldığını göstermektedir. 2011 yılında dağılım daha genişken ve bazı oyuncuların performansı ortalamadan çok daha yüksekteyken, 2022’ye gelindiğinde dağılımın daha sıkı olduğu ve uç değerlerin azaldığı gözlemlenmektedir. Özellikle 2015 ile 2019 yılları kıyaslandığında, 2015’te oyuncular arasında daha büyük farklar varken, 2019 itibarıyla çeyrekler arası farkın kapandığı ve performansların daha dengeli hale geldiği görülmektedir. Bu eğilim, ligin genel olarak daha stratejik ve dengeli bir oyun yapısına evrildiğini, bireysel üstün performanslardan çok takım oyununa dayalı bir sisteme doğru ilerlediğini düşündürmektedir.

### 5)Yıllara göre oyuncuların defansif performansları karşılaştırması violinchart
```
data <- NBAson2 %>%
 filter(!is.na(DWS))  
  mean_data <- data |>
  group_by(year) |>
  summarise(mean_OWS = mean(DWS))
ggplot(data, aes(x = DWS, y = factor(year))) +  # Swap x and y axes
  geom_violin(trim = FALSE, fill = "#01408d") +
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
![Screenshot 2025-02-27 224040](https://github.com/user-attachments/assets/d130786c-f491-48b0-b11b-8d384ba00f81)


  Grafik, NBA’de yıllar içinde defansif performansın daha dengeli hale geldiğini ve oyuncular arasındaki farkların azaldığını göstermektedir. 2011 yılında dağılım daha genişken ve bazı oyuncuların defansif katkısı ortalamadan belirgin şekilde yüksekken, 2022’ye gelindiğinde dağılımın daha sıkı olduğu ve uç değerlerin azaldığı gözlemlenmektedir. Özellikle 2015 ile 2019 yılları kıyaslandığında, 2015’te defansif performans açısından daha büyük farklılıklar görülürken, 2019 itibarıyla oyuncular arasındaki farkların daraldığı ve savunma katkısının daha istikrarlı hale geldiği fark edilmektedir. Bu eğilim, ligin genel olarak daha sistematik ve takım odaklı bir defans anlayışına geçtiğini, bireysel savunma başarısından çok takım savunmasının ön plana çıktığını düşündürmektedir.
