# Lambda Expressions ve Fonksiyonel(Functional) Interface
## Lambda Expressions

### Tanım
Lambda ifadeleri, Java SE 8 ile javaya eklenen, parametreleri
alan ve bir değer döndürebilen, bir class’a bağlı olmasına gerek kalmadan tanımlanabilen kısa bir kod bloğudur. Kısaca daha az kod ile daha hızlı geliştirmeler yapmaya yarar. Metodlara benzer ve bir metod interface’i içine direk kod içine yerleştirilebilir. Collection’lar için oldukça kullanışlıdır. Collection’dan verilerin iterate edilmesine, filtrelenmesine ve extractlenmesinde kullanılır. Lambda ifadelerinin sonucu bir değişkene atanabilir ya da bir fonksiyona parametre olarak geçilebilir.

### Syntax
En basit lambda ifadesi, tek bir parametre ve bir ifade içerir:
```java

parametre -> ifade;

```

Birden fazla parametre ile kullanımı ise;
```java

(p1,p2) -> ifade;

```

İfadeler sınırlıdır. Hemen bir değer döndürmeleri gerekir ve if
veya for gibi değişkenler, atamalar veya ifadeler içeremezler.
İfadelerin hemen bir değer döndürmeleri gerekir ve if veya for gibi değişkenler veya ifadeler içeremezler. Daha karmaşık
işlemler yapmak için süslü parantez ile bir kod bloğu
kullanılabilir. Lambda ifadesinin bir değer döndürmesi
gerekiyorsa, kod bloğunun da bir değer döndürmesi gerekir.


### Örnek Kullanım

#### Örnek-1
Listedeki her öğeyi yazdırmak için ArrayList'in forEach()
yönteminde bir lamba ifadesi kullanmak istersek:
```java
public class Main {
	public static void main(String[] args) {
		ArrayList<Integer> numbers = new ArrayList<>();
		numbers.add(5);
		numbers.add(9);
		numbers.add(8);
		numbers.add(1);
		numbers.forEach( (n) -> { System.out.println(n);} ) ;
```
#### Örnek-2
Map,stream,filter ve reduce kullanımı:

Örnek-1’de eklediğimiz sayılara 10 ekleyen bir lambda ifadesi
yazarsak
```java
List<Integer> lm = numbers.stream()
					.map(n->n+10)
					.collect(Collectors.toList());

System.out.println(lm)
```
Çıktı :

		[15, 19, 18, 11]

Listedeki tek sayıları bulmak için filter kullanımı:

```java
List<Integer> lf = numbers.stream()
					.filter(n->n%2==1)
					.collect(Collectors.toList());
					
System.out.println(lf)
```

Çıktı : 
	
	[5, 9, 1]
	
Listedeki sayıları toplamak istersek;
```java
Optional<Integer> sum = numbers.stream()
					.reduce((n1,n2)->n1+n2);

System.out.println(sum);
```
Çıktı :

	Optional[23]

## Fonksiyonel(Functional) Interface 

Functional Interface’ler diğer interfacelerden farklı olarak tek
abstract metodu tanımlayan interfacelerdir. Bu abstract metodun yanında private yada default yada static metodlarda içerebilirler fakat önemli olan sadece tek bir abstract metodunun olmasıdır. Lambdalar kullanılırken JVM(Java Sanal Makinesi) arka tarafta lambdaları birer interface implementasyonuna çevirerek büyük kolaylıklar sağlar.

Lambda ifadeleri tek başlarına bir anlam ifade etmezler. Çünkü anonim metodlardır, yani metod ismi ve ait oldukları bir sınıfları yoktur. Sadece özünde bir anonim metod implementasyonudur. O nedenle lambda ifadeleri bir fonksiyonel interface ile ilişkilendirilmelidir.

***Örnek Kullanım***
```java
	interface MyValue {  
		double getValue();  
	}
```

Lambda ifadelerini fonksiyonel interfaceler ile ilişkilendirme :
```java
interface MyValue {  
    double getValue();  
}

MyValue value = () -> 3.4;
```

>Lambda ifadesinin ve abstract metodun imzalarının tamamen birbirine uyması gerekir. Aksi durumda compiler hata verir.

Fonksiyonel interfacelerin önemli özelliklerinden biri de içerdikleri abstract metodun kendisiyle uyumlu birden fazla lambda metodu ile ilişkilendirilebiliyor olmasıdır.
```java
	public interface NumericTest {

		boolean test(int a, int b);

	}
```
```java

public class Main {

	public static void main(String[] args) {

		NumericTest isFactor = (m, n) -> m % n == 0;

		System.out.println("5 and 3 is factor " + isFactor.test(5, 3));

		System.out.println("27 and 3 is factor " + isFactor.test(27, 3));


		NumericTest lessThan = (x, y) -> x < y;

		System.out.println("5 is less than 3 " + lessThan.test(5, 3));

		System.out.println("11 is less than 17 " + lessThan.test(11, 17));


		NumericTest absEqual = (x, y) -> (x < 0 ? -x : x) == (y < 0 ? -y : y);

		System.out.println("absolute value of -9 is equal to -4 " + absEqual.test(-9, -4));

		System.out.println("absolute value of 17 is equal to -17 " + absEqual.test(17, -17));

	}

}

```
Çıktı: 

	5 and 3 is factor false  
	27 and 3 is factor true  
	5 is less than 3 false  
	11 is less than 17 true  
	absolute value of -9 is equal to -4 false  
	absolute value of 17 is equal to -17 true
