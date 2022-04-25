
# Statik Metot

Statik ifadeler sadece bir yerde çalışır ve sadece bir kere çalışır. İşin en basit hali bu. Static bir değişken herhangi bir class yapısına bağlı değildir. Bellekte tek biryerde tutulur.
Static metodların çalışma durumlarıyla aynıdır. Static olan verileri yönetirler ve aynı zamanda girdi noktaları olarak kullanılırlar. Fakat onları normal metodların içinde çağırabiliriz.

Statik metotların en yaygın kullanımı, statik değişkenlere erişmektir. Bunlara sınıf adı ve bir nokta (.) ve ardından bir yöntemin adı ile erişilir. Bir metodu tanımlanırken "static" anahtar kelimesi ile açıklayabiliriz. Statik metotlara yeni bir nesne oluşturmaya gerek kalmadan erişilebilir. bir statik metot, yalnızca diğer statik metotları veya statik data üyelerini kullanabilir ve çağırabilir. Genellikle inputların üzerinde çalışmak, hesaplama yapmak ve değer döndürmek için kullanılır.

**_Alan ve Metod :_** Aşağıda örneğini yapacağımız programda iki tane alan bulunmaktadır. Int tipinde sayac adında static bir alan ve main adında girdi noktası şeklinde bir fonksiyon bulunur bu fonksiyon programın akışını kontrol eder. Main fonksiyonu, sayac ifadesine static ifadesini kullandığı için erişebilir.

Örnekler üzerinden gidersek:
```java
public  class  Program { 
	static  int sayac; //sayac adındaki değişkeni static  olarak tanımladık.  
	public  static  void  main(String[] args) { 
	
		// Sayacı arttırıyoruz 
		Program.sayac++; 
		System.out.println(Program.sayac); 

		// arttırmayı tekrarlıyoruz  
		// ... bellek temizlenmediği için aynısının üzerinden sayar 
		Program.sayac++; 
		System.out.println(Program.sayac); 
	}
}
```
Çıktı olarak bize:

	1 
	2

>Static ifadeler bellekte tek bir alan kullanır. Biz sayacı arttırdığımızda o aynı alanda tutulur. Yani tekrar kullanılmak üzere sıfırlanmaz veya silinmez.

***Normal ve Statik Değişken***

```java
public  class  Program { 
	static  int boyut = 100; 
	int kod = 1000; 
	
	public  static  void  main(String[] args) { 
	
		//Static değişkeni direk olarak Program ile çağırabiliyoruz.
		
		System.out.println(Program.boyut); 
		
		// Normal değişkeni ise newleyerek çağırabiliyoruz. 
		
		Program p = new Program(); 				
		System.out.println(p.kod); 
	}
}
```
	100
	1000


***Final ve Statik***

Final tipindeki bir alan program içinde değiştirilemez. Static alan değiştirilebilir. Eğer final tipindeki bir değişkeni değiştirmeye çalışırsak hata alırız.

```java
public  class  Program { 

	static  int dgskn1 = 5; 
	final  int dgskn2 = 100; 
	
	public  static  void  main(String[] args) { 
	
		dgskn1 ++; 
		// bu doğru kullanım 
		dgskn2 ++; 
		// Program burda hata verecektir. 
	}
}
```
	Exception in thread "main" java.lang.Error: 
		Unresolved compilation problem: 
		Cannot make a static reference to the non-static field _code

Son olarak static değişkenler hız açısından da programın en verimli şekilde çalışmasını sağlar. Normal değişkenlere göre daha hızlı ve basittir. Bu hızı görmek için bir örnek yaparsak:

```java
public  class  Program { 

	int  normalDegisken(int a) {
	// Normal bir değişkeni çağırma metodu  
	return a * 2; 
	} 
	
	static  int  staticDegisken(int a) { 

	// Burdada static değişkeni çağıran metod  
	return a * 2; 
	} 

	public  static  void  main(String[] args)  throws Exception { 
		Program p = new Program();
		long t1 = System.currentTimeMillis();
		
		// ... Normal mdegiskenli metodu cağırıyoruz  
		for (int x = 0; x < 100; x++) { 
			for (int i = 0; i < 10000000; i++) { 
				if (p.normalDegisken(i) < 0) { 
					throw  new Exception(); 
					} 
				} 
			} 

		long t2 = System.currentTimeMillis(); 

		// ... Burdada static değişkenli metodu çağırıyruz  
		for (int x = 0; x < 100; x++) { 
			for (int i = 0; i < 10000000; i++) { 
				if (Program.staticDegisken(i) < 0) { 
					throw  new Exception(); 
				} 
			}
		} 

		long t3 = System.currentTimeMillis(); 
		// ... EN son aradaki farklarıda ekrana yazdıralım 
		System.out.println(t2 - t1); 
		System.out.println(t3 - t2); 
	} 
}
```
	955 ms 
	880 ms

Kaynaklar:
[Static Method](https://www.techopedia.com/definition/24034/static-method-java#:~:text=In%20Java%2C%20a%20static%20method,that%20object%20of%20a%20class.)
[Java ile Static İfadesi Kullanımı](https://thecodeprogram.com/java-ile-static-ifadesi-kullanimi)

