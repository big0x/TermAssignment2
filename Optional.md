# Optional Class Nedir? Kullanım Örneği

Optional Class kullanımı oldukça kolay ve kullanışlı bir yöntemdir. Yazılımda oluşan  _NullPointerException_ hatalarını en aza indirmeyi sağlamayı amaçlayan bir yapıdır. Class’larımızın null olmasının kontrolünü kolaylaştıran bir yapıdır.

Optional Class sayesinde;

-   Null kontrolü yapılmasına gerek kalmaz.
-   Kolay kod yazımı.
-   Kod okunabilirliğinin kolaylaşması.

----------

### **Optional Kullanımı**

----------

Optional Class’ı kullanarak bir nesne yaratmanın 3 farklı çeşidi bulunmakta. Bu çeşitlilik Optional nesnemizin null olup olmayacağını tanımlarken belirlememizi sağlamakta.

| Parametre | Açıklama |
|:--------|-------------:|
| empty     | boş bir nesne oluşturur. |
| of        | null değer kesinlikle alamaz |
| ofNullable| null değer alabilir. |
---------

#### **Optional Özelliği Olan Bir Nesne Oluşturmak**

----------

Boş bir nesne oluşturma işlemi;
```java
Optional<Kullanici> kullanici= Optional.empty();
```
Null değer almasını kesinlikle istemeyeceğimiz nesne oluşturmak;
```java
Optional<Kullanici> kullanici = Optional.of(kullanici);
```
Null değer alabilecek olan Optional nesnesi;
```java
Optional<Kullanici> kullanici= Optional.ofNullable(kullanici);
```
olarak yapılmaktadır.

----------

#### **Optional Nesnenin Değerlerini Alabilmek**

----------

Optional bir nesnenin değerlerine ulaşmak için  _.get()_ yazarak ve ardından tekrar nokta koyarak ilgili değişkenine erişebilmeniz mümkün olmaktadır.
```java
String ad=kullanici.get().ad;
String soyad=kullanici.get().soyad;
```
>. get() özelliği Optional nesnemizin null olmadığı zamanlarda çalışır. Diğer hallerde hata fırlatır.

----------

#### **Optional Nesne’yi Kontrol Edebilmek**

----------

Oluşturduğumuz nesnelerin içersinde hangi değerler olup olmadığını hala bilemeyebiliriz. Özellikle null değer alabilen ofNullable özelliği almış olan nesne için null kontrolü yapmak zorunlu olabilir. Optional nesne kullanıyorsak bu türe sahip nesnelerin kontrolünü sağlamak için özel parametreler kullanarak yapabilmekteyiz. Bu parametreler;


| Parametre | Açıklama |
|:--------|-------------:|
| **isPresent()**| null kontrolüdür. Boş ise false, Dolu ise true değerini döndürür. |
| **orElse()**| kullanici.get().ad().orElse(“burak”) şeklinde bir şart ile true ya da false değer döndürerek gerekli nesneyi seçebilirsiniz |
| **orElseGet()**| Nesne içerisinde eğer bir değer null ise varsaydığımız değeri gönderebiliyoruz. |
| **ifPresent()**| Eğer null değilse istediğimiz işlemleri ise bu parametre ile yapmaktayız. |
Örnek:
```java
public static void main(String[] args) {

        Optional<String> kullanici= Optional.of("Burak");
        Optional<String> kullanici2= Optional.empty();

        if (kullanici.isPresent()) {
            System.out.println("Değer bulunmakta");
        } else {
            System.out.println("Değer bulunamıyor.");
        }

        kullanici.ifPresent(g -> System.out.println("Kullanıcı Adı Var."));

    
        kullanici2.ifPresent(g -> System.out.println("Kullanıcı Adı Var."));

    }
```
Çıktı:

	Değer bulunmakta.
	Kullanıcı Adı Var.

Yukarıdaki tablodaki özellikleri kullanarak değerleri yönetebilmekteyiz.

----------

#### **Filtre Kullanımı**

----------

Optional nesnemizde de Java 8’de gelen filtreleme işlemi yapılabilmektedir.
```java
Optional<Kullanici> kullanici= Optional.ofNullable(null);

 kullanici
    .filter(k -> k.yas() > 25)
    .ifPresent(System.out::println);
   ```
