# Multithread Yapısı

## Process ve Thread

Process program tarafından çalıştırılan her bir uygulamadır. Thread ise processler tarafından yaratılan en küçük iş birimidir, aynı process içinde paralel işler yapmaya yarar. Multithreading çok daha verimli uygulamalar yazmak için önemlidir. Çünkü gerçek hayatta pek çok uygulama bir iş yaparken ya bir input bekler yada yaptığı işin sonucunun dönmesini bekler. Bu durumda CPU boşta kalır, işte multithreading ile bu idle zamanlar daha verimli hale gelir.

### Thread sınıfı ve Runnable interface

Her bir process en az bir tane thread içermek zorundadır, bu threade  _main thread_  denilir. main thread gerekli durumda başka threadleri de yaratabilir.

Java’da multithread kavramı Thread sınıfı ve Runnable interface üzerine kurulmuştur. Yeni bir thread yaratmak ve ona bir görev atamak için bu iki sınıfı kullanabiliriz; birinci yöntemde doğrudan Thread sınıfını extend edip run metodunu override edebiliriz, böylece yeni bir thread sınıfı yaratmış oluruz ve onun nesnelerini yaratarak doğrudan thread yaratma ve çalıştırma imkanına sahip oluruz. İkinci yöntemde ise Runnable sınıfını implemente eder ve run metodunu override ederek yeni bir task oluştururuz ve bu oluşturduğumuz taskı bir thread nesnesine verip çalıştırmasını isteriz.

### Thread'ler Arası Senkronizasyon

Çok threadli bir uygulama söz konusu olduğu zaman threadlerin aktivitelerini kontrol etmek gerekebilir. Bazı durumlarda iki yada daha fazla thread paylaşılan bir kaynağa aynı anda erişmek ve üzerinde değişiklik yapmak isteyebilir. Örneğin bir threadin bir dosyaya yazma işlemi yaptığı sırada bir başka threadin de aynı işlemi yapmak istemesi gibi. Böyle durumlarda kaynağa ilk ulaşan threadin işini tamamlayıncaya kadar ilgili kaynağın lock dediğimiz kilit mekanizması ile erişime kapatılması daha sonra ise tekrar erişime açılması gerekir. Java programlama dilinde her nesne bu lock mekanizması ile koruma altına alınabilir ve bu işlem synchronized ifadesi ile sağlanır.
#### Metodlar ile synchronized kullanımı

synchronized ifadesi ile metodlara erişim kontrol altına alınabilir. Bir sınıftaki herhangi bir metod synchronized ifadesini aldığı zaman o metoda bir thread girdiğinde metodun bulunduğu nesne otomatikman olarak lock mekanizması ile erişime kapatılır. Bu durumda başka bir thread o sınıf içindeki hiçbir synchronized metoda erişemez. synchronized metod üzerinde işlem yapan thread metoddan çıktığı zaman ise lock kaldırılır ve tüm nesne yeniden erişilebilir hale gelir.

```java
	public class ArrayOperations {

		int sum = 0;

		synchronized int sum(int[] array) {

			for (int value : array) {
				sum += value;
				System.out.println("Sum : " + sum + " Thread name : " + Thread.currentThread().getName());
				try {
					Thread.sleep(10);
				} catch (InterruptedException e) {
					System.out.println("Main thread interrupted");
				}
			}
			return sum;
		}
	}
```
```java
	public class MyThread extends Thread {

		int[] array;
		static ArrayOperations op = new ArrayOperations();

		public MyThread(int[] array) {
			super();
			this.array = array;
		}

		public void run() {
			System.out.println(this.getName() + " is starting");
			System.out.println("Sum of the array is : " + op.sum(this.array));
			System.out.println(this.getName() + " is finishing");
		}
	}
```
```java
	public class Main {

		public static void main(String[] args) {

			int[] array = {1, 23, 45, 9, 52, 78};

			MyThread thread1 = new MyThread(array);
			MyThread thread2 = new MyThread(array);

			thread1.start();
			thread2.start();

			try {
				thread1.join();
				thread2.join();
			} catch (InterruptedException e) {
				System.out.println("Main thread is interrupted");
			}

			System.out.println("Main thread is finishing");
		}
	}
```
Çıktı : 

	Thread-0 is starting  
	Thread-1 is starting  
	Sum : 1 Thread name : Thread-0  
	Sum : 24 Thread name : Thread-0  
	Sum : 69 Thread name : Thread-0  
	Sum : 78 Thread name : Thread-0  
	Sum : 130 Thread name : Thread-0  
	Sum : 208 Thread name : Thread-0  
	Sum of the array is : 208  
	Sum : 209 Thread name : Thread-1  
	Thread-0 is finishing  
	Sum : 232 Thread name : Thread-1  
	Sum : 277 Thread name : Thread-1  
	Sum : 286 Thread name : Thread-1  
	Sum : 338 Thread name : Thread-1  
	Sum : 416 Thread name : Thread-1  
	Sum of the array is : 416  
	Thread-1 is finishing  
	Main thread is finishing

Örnekte bir int arrayinin tüm elemanlarını toplamak için ArrayOperations sınıfı içerisinde synchronized bir sum() metodu yarattık. Daha sonra bu sınıftan sınıf düzeyinde static bir nesne yaratarak oluşturduğumuz thread sınıfından sum() metodunu çağırdık. Burada amaç threadlerin aynı nesne üzerinde işlem yapmasını sağlamak. sum() metodu içerisinde de multitaskingi mümkün kılmak için sleep() metodunu bilinçli olarak çağırdık. Son olarak main() metodu içerisinden 2 tane thread yaratarak bu threadlere oluşturduğumuz int arrayini toplamalarını istedik. Bu işlem sonunda yukarıdaki çıktıyı alarak synchronized ifadesinin threadleri nasıl blokladığını gördük.

##  Threadler arası iletişim

Bazı durumlarda birden fazla thread tarafından paylaşılan nesne geçici olarak kullanıma uygun olmayabilir. Bu durumda thread nesneyi kullanmak yerine **wait**() metodunu çağırarak kendini beklemeye alır ve diğer threadlerin işlemleri bitirmesi için beklemeye geçer. Bu durumu **notify**() yada **notifyAll**() metodlarını çağırarak nesne üzerinde işlem yapmak için bekleyen diğer threadlere bildirir. O threadler de işlemlerini bitirince aynı biçimde notify() yada notifyAll() metodlarını çağırarak diğer bekleyen threadleri uyarır. wait(), notify() ve notifyAll() metodları Object sınıfından gelir ve **synchronized kapsamı içinden çağrılmalıdır**.

```java
public class MessageGenerator {
  
    String state;
  
    synchronized void tick(boolean running) {
        if (!running) {
            state = "ticked";
            notify();
            return;
        }
      
        System.out.print("Tick ");
        state = "ticked";
        notify();
      
        try {
            while (!state.equals("tocked")) {
                wait();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
  
    synchronized void tock(boolean running) {
        if (!running) {
            state = "tocked";
            notify();
            return;
        }
      
        System.out.println("tock");
        state = "tocked";
        notify();
      
        try {
            while (!state.equals("ticked")) {
                wait();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
  ```
  ```java
  
  public class MyThread extends Thread {
    MessageGenerator generator;
  
    public MyThread(String name, MessageGenerator generator) {
        super(name);
        this.generator = generator;
    }
     
    public void run() {
      
        System.out.println(this.getName() + " is starting");
      
        if(this.getName().equals("Tick")) {
            for (int i = 0; i < 5; i++) {
                generator.tick(true);
            }
            generator.tick(false);
        }  else {
            for (int i = 0; i < 5; i++) {
                generator.tock(true);
            }
            generator.tock(false);
        }
      
        System.out.println(this.getName() + " is finishing");
    }
}

  ```
   ```java

public class Main {
    public static void main(String[] args) {
      
        MessageGenerator generator = new MessageGenerator();
      
        MyThread thread1 = new MyThread("Tick", generator);
        MyThread thread2 = new MyThread("Tock", generator);
      
        thread1.start();
        thread2.start();
      
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            System.out.println("Main thread is interrupted");
        }
        System.out.println("Main thread is finishing");
    }
}

 ```
    
 Çıktı :
	
	Tick is starting  
	Tock is starting  
	Tick tock  
	Tick tock  
	Tick tock  
	Tick tock  
	Tick tock  
	Tick is finishing  
	Tock is finishing  
	Main thread is finishing

Örnekte görüldüğü gibi iki thread farklı mesajları ekrana bastırmak için ayarlanmıştır ve threadler mesajları yazdırmak için birbirini beklemektedir. İlk thread ‘Tick’ mesajını yazdırdıktan sonra beklemeye geçer ve topu diğer threade atar. O thread de aynı şekilde ‘tock’ mesajını yazdıktan sonra beklemeye geçer ve yeniden ‘tick’ mesajı yazılması için diğer threadi uyarır.

## Deadlock kavramı

Multithread uygulamalarda birden fazla threadin paylaşılan kaynaklara erişmek için aynı anda birbirini beklemeye geçme durumudur. Örneğin bir thread bir A nesnesinin synchronized metodundan başka bir B nesnesinin synchronized metodunu çağırıyorsa ve aynı zamanda başka bir thread B nesnesinin bir synchronized metodu içerisinden A nesnesinin synchronized metodunu çağırıyor ise bu durumda deadlock oluşur. Bu durumda threadler çıkmaz bir yola girmiş olup birbirlerini sonsuza kadar beklerler. Böyle durumlar çok nadir oluştuğundan ve threadlerin aynı CPU cycle ında bu durumu gerçekleştirmesi gerektiğinden bu durumu debug etmek ve test etmek zordur. Ama yine de multithread bir uygulama yazıyorsak thread safe olan kod içeren sınıfların bağımlılıklarını kontrol etmek gerekir.
