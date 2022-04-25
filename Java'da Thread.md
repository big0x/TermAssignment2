
# Thread Nedir?

Her bir işlemin altında çalışan alt işlemlere thread adı verilir. Birden fazla işlemi aynı anda yapmayı sağlayan yapıya thread denir. Programımızda işlemler belirli bir sırayla değil de aynı zamanda yapılır.

```java
public class ThreadOrnegi{

    public static void main(String[] args) {
        uzunBirIslem();
        System.out.println("Merhaba Thread");
    }

    private static void uzunBirIslem() {
        try {
            // Burada uzun bir işlem yapılıyor.
            Thread.sleep(5 * 1000);
            System.out.println("Uzun işlem sonucu");
        } catch (InterruptedException ex) {
            System.err.println(ex);
        }
    }
}
```
Örnekte basit bir **Merhaba Thread** yazısının yazılması için **uzunBirIslem** metodunun bitmesi beklenmektedir.

## Thread kullanımı

Thread kullanımı için  **Thread**  sınıfını extendslemek veya  **Runnable**  implemente etmek üzere iki yöntem kullanılır.

Thread sınıfının kullanımı için Thread sınıfını extendsledikten sonra  **run**  metodu override edilir ve gerekli komutlar yazılır.
```java
public class ThreadOrnegi extends Thread {

    public static void main(String[] args) {
        ThreadOrnegi threadOrnegi = new ThreadOrnegi();
        threadOrnegi.start();
        System.out.println("Merhaba Thread");
    }

    @Override
    public void run() {
        try {
            // Burada uzun bir işlem yapılıyor.
            Thread.sleep(5 * 1000);
            System.out.println("Uzun işlem sonucu");
        } catch (InterruptedException ex) {
            System.err.println(ex);
        }
    }
}
```
> Main metodu da bir thread oluşturur.

**Runnable**  arayüzünün kullanımı için arayüz metotları sınıf tarafından uygulanır(implements).

Arayüz uygulandıktan sonra Thread sınıfının kurucusuna sınıf parametre olarak geçilerek thread çalıştırılır.

```java
public class ThreadOrnegi implements Runnable {

    public static void main(String[] args) {
        Thread t1 = new Thread(new ThreadOrnegi());
        t1.start();
        System.out.println("Merhaba Thread");
    }

    @Override
    public void run() {
        try {
            // Burada uzun bir işlem yapılıyor.
            Thread.sleep(5 * 1000);
            System.out.println("Uzun işlem sonucu");
        } catch (InterruptedException ex) {
            System.err.println(ex);
        }
    }
}
```
>Java çoklu kalıtımı desteklemediğinden **Runnable** arayüzünün kullanımı faydalı olacaktır.

## Thread yönetimi

Birden fazla thread aynı anda tek bir kaynağı kullanmaya çalıştığında belirsiz durum ortaya çıkar.

```java
public class ThreadOrnegi implements Runnable {

    public static void main(String[] args) {
        Thread t1 = new Thread(new ThreadOrnegi());
        Thread t2 = new Thread(new ThreadOrnegi());

        t1.start();
        t2.start();
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName());
        }
    }
}
```
Yukarıdaki örnek her çalıştırıldığında farklı bir sonuç verecektir. Bu belirsizliğin önüne geçmek için thread’ler arasında senkronizasyon iyi bir şekilde yapılması gerekir. Senkronizasyonun iyi bir şekilde yapılması için çeşitli teknikler kullanılır. Senkronizasyon için threadler belirli bir süre durdurulabilir.
```java
public class ThreadOrnegi implements Runnable {

    public static void main(String[] args) {
        Thread t1 = new Thread(new ThreadOrnegi(1000));
        Thread t2 = new Thread(new ThreadOrnegi(2000));
        
        t1.start();
        t2.start();
    }

    int sure;

    public ThreadOrnegi(int sure) {
        this.sure = sure;
    }

    @Override
    public void run() {
        try {
            Thread.sleep(this.sure);
            for (int i = 0; i < 100; i++) {
                System.out.println(Thread.currentThread().getName());
            }
        } catch (InterruptedException ex) {
            System.err.println(ex);
        }

    }
}
```

Bu yöntem senkronizasyonu sağlarken thread yapısının sağladığı avantajı ortadan kaldırır.

Diğer bir yöntem ise  **synchronized**  anahtar kelimesini kullanmaktır.

```java
public class ThreadOrnegi implements Runnable {

    public static void main(String[] args) {
        Thread t1 = new Thread(new ThreadOrnegi());
        Thread t2 = new Thread(new ThreadOrnegi());

        t1.start();
        t2.start();
    }

    @Override
    public void run() {
        synchronized (ThreadOrnegi.class) {
            for (int i = 0; i < 100; i++) {
                System.out.println(Thread.currentThread().getName());
            }
        }
    }
}
```

Diğer bir yöntem ise thread’lere öncelik vermektir.

```java
public class ThreadOrnegi implements Runnable {

    public static void main(String[] args) {
        Thread t1 = new Thread(new ThreadOrnegi());
        t1.setPriority(Thread.MIN_PRIORITY);
        Thread t2 = new Thread(new ThreadOrnegi());
        t2.setPriority(Thread.MAX_PRIORITY);

        t1.start();
        t2.start();
    }
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName());
        }
    }
}
```
>Öncelik vermek thread’lerin senkron çalışmasını garanti etmez.


