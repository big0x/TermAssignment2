# Exception

Exception, programın kod akışında runtime sırasında meydana gelen olağan dışı durumlardır. Bu durumlarda compiler bir ***exception object*** yaratır. Bu obje runtime sırasında oluşturulan exceptionların bilgisini tutar.

## Error mu Exception mı?
Java Error, ***Throwable***'ın alt sınıfıdır. Yakalanmasına gerek olmayan ciddi bir sorunu temsil eden durumdur ve bu yüzden fırlatılması için ***throw*** keywordüne gerek yoktur.

Java Exception ise uygulamanın yakalaması ve handlelaması gereken durumları temsil eder. Bir çok alt sınıfı vardır.


### Throwable Nedir?
Throwable tüm hataların ve exceptionların üst sınıfıdır. Yalnızca Throwable sınıfının alt sınıfı olan nesneler ***throw*** lanabilir.

### Throw Nedir?
Hata oluşturmak için kullanılan keyworddür. Programın istediğimiz bir yerinde exception oluşturmak istersek kullanırız. Örnek olarak:
```java
throw new BusinessException("Kullanıcı Bulunamadı.")
```
### Throws Nedir?
Bir metotta exception yapısı var ise yani bazı olasılıklarda hata fırlatması gerekiyorsa throws keywordü kullanılır. Örnek olarak:
```java
public void create() throws BusinessException{
}
```


## Exception Türleri

![HashCodec](https://hashcodec.com/java-programming/Throwable.png)

## Checked Exception

Derleme zamanında işlenen exceptionlardır. Yazdığımız kodda exception atılma ihtimali varsa IDE bize kodu compile etmeden uyarı verir ve ***throws*** ile handlelamamızı ister.

### ClassNotFoundException
Bir sınıfı yüklemeye çalıştığımızda JVM o sınıfı verdiğimiz sınıf yolunda(classpath) bulamadığında ortaya çıkan exceptiondır.

### IOException
IOException, akışlar, dosyalar ve dizinler kullanılarak bilgilere erişilirken oluşan özel durumlar için temel sınıftır. Akışın kendisi bozulduğunda veya verileri okurken bir hata oluştuğunda bir IOException oluşturabilir.

### FileNotFoundException

Bir dosya yolunun(path) geçersiz olması durumunda ortaya çıkan exceptiondır. Eğer verdiğimiz path'de böyle bir klasör bulunmuyorsa bu exceptionla karşılaşırız.

### SQLException

Database ile ilgili erişim veya herhangi bir sorunda ortaya çıkan exceptiondır. Program veri kaynağıyla etkileşim sıranında bir problem yaşadığında bu exceptionla karşılaşırız.

### NoSuchMethodException
Method bulunamadığında karşılaştığımız exceptiondır. Derleme zamanında var olan fakat runtime da bulunamayan bir metot çağırıldığında bu exceptionla karşılaşırız.

## Unchecked Exception 

RuntimeException classından türetilmiş bir exception sınıfıdır. IDE'nin bizi uyarmadığı exception türleridir. Compile time'da fırlatılır.

### NullPointerException
Herhangi bir nesneye işaret etmeyen ve hiçbir şeye veya boş değere atıfta bulunmayan bir değişkene erişildiğinde karşılaştığımız exceptiondır. Null gelen bir değerin üzerinde işlem yaptığımızda bu exceptionla karşılaşırız.

### IllegalArgumentException

IllegalArgumentException, bir metot, geçersiz bir argümandan geçtiğinde karşılaştığımız bir exceptiondır. Yani metodumuz böyle bir nesne beklemiyorsa bu exceptionla karşılaşırız.

### ArithmeticException

Ortada matematiksel bir sorun olduğunda karşılaştığımız exceptiondır. Genelde 2 sayıyı bölmeye çalıştığımızda ve paydadaki sayı 0 ise karşılaştığımız exceptiondır.

### IndexOutOfBoundsException

Bir indexin o dizide olmadığını ya da rangeinin dışında olması durumunda karşılaştığımız exceptiondır.
