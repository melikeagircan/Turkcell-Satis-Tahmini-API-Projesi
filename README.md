
Projenin Amacı
Bu projenin amacı, bir ürünün belirli bir çeyrekte yapılan indirimlerin satış miktarına etkisini ölçmektir. Proje, aşağıdaki adımları içerir:
Veri Ön İşleme: Çeyrek bazlı analiz için verilerin hazırlanması.
Model Eğitimi: İndirimlerin satışlara etkisini tahmin etmek için bir makine öğrenimi modeli eğitilmesi.
Tahmin API'si: Kullanıcıdan gelen verilerle tahmin yapılmasını sağlayan bir API geliştirilmesi.

project/
│
├── main.py                                                                                    # API uygulaması
├── verionisleme_trainmodel.py                             # Veri işleme ve model eğitimi
├── salesprediction_decisiontree_model.pkl                # Eğitilmiş model dosyası
├── tabledf.csv                                                                                            # Veri seti
└── README.md                                                                         # Proje açıklamaları


🛠 Kullanılan Teknolojiler ve Kütüphaneler
Programlama Dili: Python 3.x
Veritabanı: PostgreSQL (Northwind)
API Framework: FastAPI
Makine Öğrenmesi: Scikit-learn
Veri İşleme: Pandas, NumPy
Veri Erişimi: SQLAlchemy
Dokümantasyon: Swagger (FastAPI ile otomatik)

Veri Kaynağı ve Erişim
Northwind veritabanı PostgreSQL kullanılarak bağlantı kurulmuş ve tablolar filtrelenerek kullanılmıştır.
Veri Seti Kaynağı: https://github.com/engindemirog/Northwind-Database-Script-for-Postgre-Sql/blob/master/script.sql
Veritabanı Bağlantısı: SQLAlchemy ile sağlanmıştır.
Bağlantı Kurma: Eğer Pandas ve PostgreSQL bağlantısı için psycopg2 veya SQLAlchemy yüklü değilse, şu komut ile yükleyebilirsiniz:
```bash
pip install pandas psycopg2 sqlalchemy
```
PostgreSQL'den VS Code'a olan bağlantıyı sağlamak için `db_to_vscode.py` dosyasını çalıştırabilirsiniz. (Postgre içerisinde yazılan sorgu da bu dosyanın içinde.)

Dosya Açıklamaları
1. verionisleme_trainmodel.py
Bu dosya, veri işleme ve model eğitimi adımlarını içerir.
Fonksiyonlar
preprocess_data(df)

Veriler üzerinde aşağıdaki işlemleri gerçekleştirir:
Eksik değerlerin doldurulması.
Aykırı değerlerin Winsorize yöntemiyle sınırlandırılması.
Toplam fiyat hesaplanması.
Tarih manipülasyonu ve çeyrek hesaplaması (yearquarter sütunu).
İndirim etkinliğinin hesaplanması (discount_effective sütunu).
Kategorik değişkenlerin one-hot encoding ile dönüştürülmesi.
train_and_save_model()

Veri setini yükler (tabledf.csv).
Veriler üzerinde ön işleme adımlarını uygular.
discount_effective sütununu hedef değişken olarak kullanarak bir Decision Tree modeli eğitir.
SMOTE ile veri dengesizliğini giderir.
GridSearchCV ile modelin hiperparametrelerini optimize eder.
En iyi modeli salesprediction_decisiontree_model.pkl dosyasına kaydeder.
Kullanım
Terminalde aşağıdaki komutla çalıştırabilirsiniz:
python verionisleme_trainmodel.py

Bu işlem tamamlandığında, eğitilmiş model salesprediction_decisiontree_model.pkl dosyasına kaydedilecektir.

2. main.py
Bu dosya, FastAPI kullanılarak bir tahmin API'si oluşturur.
Fonksiyonlar
predict_approval(applicant: Applicant)
Kullanıcıdan gelen verileri alır ve doğrular.
Veriler üzerinde ön işleme adımlarını uygular.
Eğitilmiş modeli kullanarak tahmin yapar.
Tahmin sonucunu ve işlenmiş verilerin detaylarını döndürür.
API Endpoint
POST /predict
Açıklama: Kullanıcıdan gelen verilerle tahmin yapar.
Girdi (JSON):
{
  "order_date": "1996-07-08",
  "customer_id": "HANAR",
  "city": "Rio de Janeiro",
  "product_id": 51,
  "product_name": "Manjimup Dried Apples",
  "units_in_stock": 20,
  "unit_price": 42.4,
  "quantity": 35,
  "discount": 0.15,
  "category_id": 7,
  "category_name": "Produce"
}

Kullanım
Terminalde aşağıdaki komutla API'yi çalıştırabilirsiniz:
uvicorn main:app --reload
API çalıştırıldıktan sonra, tarayıcınızda şu URL'yi açarak FastAPI dokümanlarına erişebilirsiniz:
http://127.0.0.1:8000/docs


Proje Akışı
Veri Hazırlığı:

tabledf.csv dosyasındaki veriler, verionisleme_trainmodel.py dosyasındaki preprocess_data fonksiyonu ile işlenir.
Çeyrek bazlı analiz için yearquarter sütunu oluşturulur.
İndirimlerin satışlara etkisini ölçmek için discount_effective sütunu hesaplanır.
Model Eğitimi:

train_and_save_model fonksiyonu, discount_effective sütununu hedef değişken olarak kullanarak bir Decision Tree modeli eğitir.
Model, GridSearchCV ile optimize edilir ve salesprediction_decisiontree_model.pkl dosyasına kaydedilir.
Tahmin API'si:

main.py dosyasındaki API, kullanıcıdan gelen verilerle tahmin yapar.
API, indirim oranının satışlara etkisini (Increased veya Not Increased) döndürür.
Sonuç
Bu proje, çeyrek bazlı zamanlarda bir ürünün indirim oranının satışlara etkisini ölçmek için kapsamlı bir çözüm sunar. Veri ön işleme, model eğitimi ve tahmin API'si gibi temel adımları içerir. Proje, hem veri analizi hem de makine öğrenimi uygulamaları için güçlü bir temel sağlar.

Eğer herhangi bir sorunla karşılaşırsanız veya geliştirme önerileriniz varsa, lütfen iletişime geçin!



































Veri Önişleme (EDA)
Veri seti analiz edilerek ihtiyaçlara uygun hale getirilmiştir.
Eksik Veri Kontrolü
- Veri kümesinde eksik veya null değer bulunmamaktadır.
- `df.info()` ve `df.isna().sum()` fonksiyonları ile doğrulama yapılmıştır.
- `df.describe()` ile istatistiksel yorumlama yapılmıştır.

Aykırı Değer Analizi
- **IQR Yöntemi**: Aykırı değerler, interquartile range (IQR) yöntemi ile tespit edilmiştir.
- **Winsorize Yöntemi**: Aykırı değerlerin etkisini azaltmak için Winsorize yöntemi uygulanmıştır.

Aykırı değerlerin etkisini azaltarak veriyi daha sağlıklı hale getirdikten sonra, sayısal değişkenlerin dağılımları incelenmiştir. Örneğin, `unit_price` değişkeninin dağılımı histogram ve kutu grafikleri ile görselleştirilmiştir.



Feature Engineering
- Çeyrek bazlı satışlardaki indirim etkisini anlamak için `order_date` sütununun veri tipi değiştirilmiştir.
- Yeni sütunlar eklenmiştir: `total_price`, `yearquarter`, `discount_effective`.
- **Önemli Gözlem**: Veri seti, 1996 yılının ikinci yarısından 1998 yılının ilk yarısına kadar olan verileri içermektedir.
- **`discount_effective` Sütunu**: Satış miktarı, bir önceki çeyrekteki satış miktarından fazla mı? Bu koşul, indirimin satış artışına neden olup olmadığını kontrol etmektedir. Ayrıca oluşturmuş olduğumuz label smote bir şekilde dağılmamaktadır.

Daha detaylı gözlem için `eda.py` dosyasını çalıştırabilirsiniz.



Modelleme
Değişken değerler saptanmış ve iyileştirmeler yapılıp; makine öğrenmesi modelleri üzerine çalışılmıştır. Bu kapsamda oluşturulan modeller; müşterilerin alışveriş davranışlarını anlamak, bu davranışları manipüle etmek ve çeşitli tahminler yapabilmek için özellikle şu alanlarda kullanılacaktır:
Geciken sipariş tahmin -> Gelecek siparişlerde gecikme riski hesaplanacaktır
Gelecek siparişlerde yaşanacak olası gecikmeler önceden tahmin edilerek, müşterilere zamanında bilgi vermek amaçlanmaktadır.
Stok döngüsü takibi  
Stokların ne zaman tükenebileceği ve hangi ürünlerin daha fazla talep göreceği tahmin edilerek, üretim ve tedarik zinciri yönetimi optimize edilecektir.
İndirimlerin müşteri bazlı güncellenmesi -> Buna göre satışlara oransal yansıması
Müşteri davranışları göz önünde bulundurularak, indirim oranları kişiye özel hale getirilecek ve buna bağlı olarak satışlar üzerinde oransal bir etki sağlanacaktır.
Mevsimlere (çeyreklere) göre alışverişlerde indirim oranı önerisi
Çeyrek bazında, sezonluk alışveriş alışkanlıkları dikkate alınarak indirim oranları belirlenip, buna göre alışveriş stratejileri önerilecektir.

Kullanılan Algoritmalar:
KNN (K-Nearest Neighbors)
Decision Tree
Logistic Regression


Modelleme sürecinde algoritmaları kullanılmıştır ve bu modellerin performansları karşılaştırılarak en iyi sonuç veren model seçilmiştir.
>> Model performans değerlendirilmesinde; X, Y, Z metrikleri kullanılarak doğruluk oranları ölçülmüştür. En iyi performansı gösteren AAAAA modeli, tahminleme için kullanılmıştır.

































Model Performans Değerlendirmesi:
X, Y, Z metrikleri kullanılmıştır.
En iyi performansı gösteren AAAAA modeli tahminleme için seçilmiştir.
Elde edilen modellerle aşağıdaki tahminlemeler yapılmaktadır:
Geciken Sipariş Tahmini: Sipariş gecikme riskini hesaplama.
Stok Döngüsü Takibi: Ürün taleplerini tahmin ederek stok optimizasyonu sağlama.
Müşteri Bazlı İndirim: Müşteri davranışlarına göre indirim oranları belirleme.
Mevsimsel İndirim Önerileri: Sezonluk alışveriş alışkanlıklarına dayalı indirim stratejileri geliştirme.

🛠 Kurulum ve Çalıştırma
Projenin çalıştırılması için aşağıdaki adımları izleyebilirsiniz.
Gerekli Bağımlılıkları Yükleyin:
pip install -r requirements.txt
Veri Tabanını Yükleyin: PostgreSQL'de script.sql dosyasını çalıştırarak veritabanını oluşturun.
API'yi Başlatın:
uvicorn main:app --reload
API Dokümantasyonunu Açın:
Tarayıcıdan http://127.0.0.1:8000/docs adresine giderek Swagger UI’yi kullanabilirsiniz.





Sonuç
Bu proje, makine öğrenmesi teknikleri kullanarak indirimlerin satış üzerindeki etkisini analiz etmeye odaklanmaktadır. REST API ile entegre edilerek dış sistemlerin de bu tahminleri kullanması sağlanmıştır. Modelleme aşamasında farklı algoritmalar test edilerek en iyi sonuç veren model seçilmiş ve performans değerlendirmesi yapılmıştır.
Geliştirmeler & Öneriler:
Modelin doğruluğunu artırmak için daha fazla veri ile yeniden eğitilebilir.
Derin öğrenme teknikleri ile daha güçlü tahminleme modelleri oluşturulabilir.
Dashboard geliştirerek görselleştirme eklenebilir.
🚀 Katkıda bulunmak isteyenler için pull request'ler açıktır!








1. Decision Tree (Karar Ağacı)
Regresyon Problemleri İçin:
R² (Determinasyon Katsayısı):
Modelin bağımlı değişkenin varyansını ne kadar açıkladığını gösterir.
Aralık: (-∞, 1].
1'e yakınsa: Model iyi açıklıyor.
0'a yakınsa: Model bağımsız değişkenleri açıklamakta zayıf.
Negatifse: Model, ortalamadan bile kötü tahmin yapıyor.
Önem: Modelin genel açıklayıcılığını ölçmek için kullanılır.
RMSE (Root Mean Squared Error - Kök Ortalama Kare Hatası):
Hataların karesinin ortalamasının kareköküdür.
Önem: Büyük hatalara duyarlıdır, bu nedenle modelin hata büyüklüğünü anlamak için kullanılır.
MAE (Mean Absolute Error - Ortalama Mutlak Hata):
Hataların mutlak değerlerinin ortalamasıdır.
Önem: RMSE'ye göre daha az ceza keser, daha açıklayıcıdır.
Sınıflandırma Problemleri İçin:
Accuracy (Doğruluk):
Toplam doğru tahmin edilen örneklerin, tüm örneklere oranıdır.
Önem: Dengeli veri setlerinde anlamlıdır, ancak dengesiz veri setlerinde yanıltıcı olabilir.
Precision (Kesinlik):
Pozitif olarak tahmin edilenlerin gerçekten pozitif olma oranıdır.
Önem: Yanlış pozitiflerin önemli olduğu durumlarda kullanılır.
Recall (Duyarlılık):
Gerçek pozitiflerin ne kadar iyi tespit edildiğini gösterir.
Önem: Yanlış negatiflerin önemli olduğu durumlarda kullanılır.
F1-Score:
Precision ve Recall’un harmonik ortalamasıdır.
Önem: Dengesiz veri setlerinde daha anlamlıdır.
ROC-AUC (Receiver Operating Characteristic - Alan Altındaki Eğri):
Modelin genel ayırt etme kapasitesini ölçer.
Önem: 1’e yakınsa model çok iyi ayırt ediyor, 0.5 civarındaysa model rastgele tahmin yapıyor.
2. K-Nearest Neighbors (KNN)
Regresyon Problemleri İçin:
RMSE ve MAE:
RMSE, uç değerlere duyarlıdır ve hataların büyüklüğünü ölçmek için kullanılır.
MAE, hataların mutlak değerlerinin ortalamasını verir ve daha az ceza keser.
Önem: KNN, mesafe tabanlı bir algoritma olduğu için bu metrikler önemlidir.
Sınıflandırma Problemleri İçin:
Accuracy:
Modelin genel doğruluğunu ölçmek için kullanılır.
Önem: Dengeli veri setlerinde anlamlıdır.
F1-Score:
Precision ve Recall’un dengeli bir ölçüsüdür.
Önem: Dengesiz veri setlerinde daha anlamlıdır.
ROC-AUC:
Modelin genel ayırt etme kapasitesini ölçer.
Önem: KNN, sınıflandırma problemlerinde ROC-AUC ile değerlendirilir.
3. Logistic Regression (Lojistik Regresyon)
Sınıflandırma Problemleri İçin:
Precision:
Pozitif olarak tahmin edilenlerin gerçekten pozitif olma oranıdır.
Önem: Yanlış pozitiflerin önemli olduğu durumlarda kullanılır.
Recall:
Gerçek pozitiflerin ne kadar iyi tespit edildiğini gösterir.
Önem: Yanlış negatiflerin önemli olduğu durumlarda kullanılır.
F1-Score:
Precision ve Recall’un harmonik ortalamasıdır.
Önem: Dengesiz veri setlerinde daha anlamlıdır.
ROC-AUC:
Modelin genel ayırt etme kapasitesini ölçer.
Önem: Logistic Regression, ROC-AUC ile değerlendirilir.
Özet: Hangi Metrik Daha Önemli?

Algoritma
Regresyon İçin Önemli Metrikler
Sınıflandırma İçin Önemli Metrikler
Decision Tree
R², RMSE, MAE
Accuracy,Precision, Recall, F1-Score
KNN
RMSE, MAE
Accuracy, F1-Score, ROC-AUC
Logistic Regression
-
Precision, Recall, F1-Score, ROC-AUC




	
		
		
Not:
Dengesiz Veri Setleri: Precision, Recall ve F1-Score daha anlamlıdır.
Uç Değerler: RMSE, uç değerlere duyarlı olduğu için dikkatle kullanılmalıdır.
ROC-AUC: Modelin genel performansını ölçmek için her zaman faydalıdır.



















