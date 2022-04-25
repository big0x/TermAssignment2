
# Exception Handling ( Hata Yakalama)
Exception runtime da yani uygulama çalışırken meydana gelen hatalardır. Bu hataların bir kısmı tolere edilebilirken bir kısmı ise uygulamanın tamamen durmasına neden olur. Javada kullanılması kolay ve esnek bir hata yakalama mekanizması vardır.

## Temelleri ( Try-Catch)

Hata açısından izlemek istediğimiz kodları try bloğuna yazmalıyız. try bloğu içerisinde bir hata oluşursa bu hata fırlatılır. Fırlatılan hatayı catch bloğu yakalar. Sistem tarafından oluşturulan hatalar otomatik olarak fırlatılır fakat bazı durumlarda bizim de manuel olarak hata fırlatmamız gerekir, bu durumda throw ifadesi kullanılır. Bazı durumlarda yazdığımız metodun hangi hataları fırlatabileceğini metod imzasında belirtmemiz gerekir. Bu durumda throws ifadesi kullanılır. Catch blokları yalnızca ilgili hata oluştuğu zaman çalıştırılır.
```java
public static void main(String[] args){
	try {
	// kod
	} catch(ExceptionType1 e) {
	// ExceptionType1 için handler
	} catch(ExceptionType2 e) {
	// ExceptionType2 için handler
	}
}
```

Örnek olarak bir NullPointerException handling edersek:

```java
public class Practice{
	public static void main(String[] args){
		try {
			String name = null;
			System.out.println(name.length());
		} catch (NullPointerException e) {
			System.out.println(e);
			System.out.println("Null value is not accepted.");
		}
	}
}
```


Exception olsun ya da olmasın çalışmasını istediğimiz bir blok olursa da catch bloğundan sonra finally bloğu da ekleyebiliriz. Örnek olarak bir stream işlemi sonrası ne olursa olsun o streamin kapatılması gerekir. Böyle durumlarda stream kapatma işlemini finally bloğuna yazarız.

```java
public static void main(String[] args){
	try {
	// kod
	} catch(ExceptionType1 e) {
	// ExceptionType1 için handler
	} finally {
	// finally bloğu
	}
}
```
## Exception Handling'de Üst-Alt Class
Bir try ifadesinin birden fazla catch bloğu ile ilişkilendirilebilir. Böyle bir durumda aralarında üst sınıf-alt sınıf ilişkisi bulunan hatalardan önce alt sınıf hatayı sonra üst sınıf hatayı catch blokları ile yakalamalıyız. Yani her zaman hata sıralamasında özelden genele doğru gidilmeli. Tersi durumda compiler hata verecektir.

## Try-With-Resources

Java-7 ile birlikte bu tarz güvenlik durumları için try-with-resource kavramı geldi. try-with-resource ile açılan kaynaklar try bloğu sonrası otomatik olarak kapatılır ve çoğu durumda finally bloğuna gerek kalmaz. Bu işlemin gerçekleşebilmesi için @AutoClosable interface'ini implemente etmeleri gerekir.
```java
public static void main(String[] args){
	try(//örnek olarak stream işlemi buraya yazıldığında ya da new leme işlemi, çalıştıktan sonra otomatik kapanır. ){
	// kod
	} catch(ExceptionType1 e) {
	// ExceptionType1 için handler
	}
}
```

