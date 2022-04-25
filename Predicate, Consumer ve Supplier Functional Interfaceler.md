
# Predicate, Consumer ve Supplier Functional Interfaceler

Farklı tiplerdeki _lambda_ ifadelerine temel oluşturmak için *[java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)* paketi altında fonksiyonel arayüzler bulunmaktadır. Bunları 4 kategoriye ayırabiliriz.

-   **Consumer** : Paremetre alıp işlem yapar ve geriye sonuç döndürmez.
 ```java
			void accept(T t);
```
      
-   **Predicate**: Parametre alır ve şarta bağlı olarak boolean sonuç döndürür.
```java
		boolean test(T t);
```
-   **Supplier**: Hiç parametre almaz ancak T tipinde değer döndürür.
```java
		T get();
```
-  **Function**: Parametre alır, işler ve geriye bir sonuç üretir.
```java
		R apply(T t);
```

## Consumer Arayüzü

Jenerik olan T tipinde (yani isteğe göre herhangi bir Java tipi alabilir) bir parametre alan ve aynı zamanda geriye sonuç döndürmeyen bir metoda sahiptir. Consumer tek bir soyut metoda sahip olduğundan dolayı functional interface’dir. Temel olarak yaptığı iş kendisine verilen lambda ifadesini yerine getirmektir.

```java

@FunctionalInterface
public interface Consumer<T> {
	
	void accept(T t);
	default Consumer<T> andThen(Consumer<? super T> after){...}
}

```

Örnek olarak : 
```java
public class EasyConsumerExample {

	public static void main(String[] args) {
		// accept() metoduna String tipte bir parametre göndererek çalışmasını sağladık.
		Consumer<String> consumer = name -> System.out.println("İsminiz : " + name);
		consumer.accept("Mehmet Yurdakul"); 
	}
}
```
Çıktı :

	İsminiz : Mehmet Yurdakul

>Consumer arayüzü içerisinde default olarak tanımlanmış _andThen()_ metodu 2 Consumer’ı birbirine zincirleyerek accept() metodu çağrıldığında sıralı olarak çalışabilmelerini sağlar.

Consumer arayüzünü kullanırken jenerik tip kullandık, ancak primitive tip kullanmak istersek :

-   **IntConsumer**: Parametre olarak int değer alır.
-   **LongConsumer**: Parametre olarak long değer alır.
-   **DoubleConsumer**: Parametre olarak double değer alır.
-   **BiConsumer**: T ve U tiplerinde iki parametre alır .
-   **ObjIntConsumer**: T ve int tiplerinde iki parametre alır.
-   **ObjDoubleConsumer**: T ve double tiplerinde iki parametre alır.
-   **ObjLongConsumer**: T ve long tiplerinde iki parametre alır.
## Predicate Arayüzü
Predicate arayüzü T tipinde bir girdi alır ve kendisine sunulan şartın sağlanıp sağlanmadığını kontrol ederek geriye “true ya da false” değer döner. Boolean dönüş değerine sahip bir işlev olarak düşünülebilir. _Filtreleme ve gruplama_ gibi işlemlerde kullanılır.

```java

@FunctionalInterface
public interface Predicate<T> {
	
	boolean test(T t);
	default Predicate<T> and(Predicate<? super T> other){...}
	default Predicate<T> negate(){...}
	default Predicate<T> or(Predicate<? super T> other){...}
	static<T> Predicate<T> isEqual(Object targetRef){...}
	static<T> Predicate<T> not(Predicate<? super T> target){...}
}
```

Örnek olarak : 
```java
public class PredicateExample {
	public static void main(String[] args) {

		Predicate<Integer> isAEvenNumber = number -> number % 2 == 0 ;
		boolean result = isAEvenNumber.test(60);
		System.out.println("Girilen sayı çift mi ? " + result); // true

		Predicate<String> isALongSentence = (String sentence) -> sentence.length() > 60;
		boolean sonuc = isALongSentence.test("Bu uzun bir cümle ama uzunluğu 60 karakterden uzun değil.");
		System.out.println("Girilen cümle 60 karakterden uzun mu ?" + sonuc); // false
	}
}
```
Predicate arayüzü, _test()_ metodu haricinde içerisinde 3 metod daha barındırır. Bu metodların yaptıkları işler :

-   **and():** İki Predicate’i birleştirir ve mantıkta ki ve (&&) işlemine tabi tutar.
-   **or():**  and() ile aynıdır ancak bu sefer mantıkta ki or işlemine tabi tutulur.
-   **negate():**  Not ile aynı işlevi görür. Sonuç neyse tersini geri döndürür.

Predicate arayüzü aynı zamanda _primitive_ tipte değerler alabilecek versiyonlarıda bulunmakta.

-   **BiPredicate**: T ve U tiplerinde iki parametre alır boolean değer üretir.
-   **IntPredicate**: Parametre olarak int değer alıp sonuç üretir.
-   **DoublePredicate**: Parametre olarak double değer alıp sonuç üretir.
-   **LongPredicate**: Parametre olarak long değer alıp sonuç üretir.

## Supplier  Arayüzü

Supplier arayüzü, Consumer arayüzünün tam tersini yapar. Yani girdi olarak bir şey almaz ancak geriye bir değer döndürür.

```java

@FunctionalInterface
public interface Supplier<T> {
	
	T get();
}
```

Mesela, _User_ adında bir classımızın olsun ve yalnızca name adında bir propertye sahip olduğunu düşünelim. Supplier arayüzü yardımıyla bu sınıftan bir nesne oluşturulım. Constructor’ına parametre olarak isim bilgisini verelim.

```java

public class User{

	private String name;

	public User(String name) {
	this.name = name;
	}

	public String getName() {
	return name;
	}

	public void setName(String name) {
	this.name = name;
	}

	@Override
	public String toString() {
	return "User{" + "name='" + name + '\'' +'}';
	}
}
```
```java

public class SupplierExample {
	public static void main(String[] args) {
		Supplier<User> supplier = () -> new User("Mehmet");
		User user = supplier.get();
		System.out.println(user);

		// BooleanSupplier
		BooleanSupplier isLogin = () -> false;
		System.out.println(isLogin.getAsBoolean());
	}
}
```
Primitive tipler ve boolean değer içinde yardımcı alt arayüzler bulunmaktadır.

-   **BooleanSupplier:**  Geriye boolean değer döndürür.
-   **IntSupplier:**  Geriye int değer döndürür.
-   **DoubleSupplier:**  Geriye double değer döndürür.
-   **LongSupplier:**  Geriye long değer döndürür.

