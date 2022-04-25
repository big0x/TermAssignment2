# Metod Referans
Büyük projelerdeki karmaşıklığı minimum seviyede tutabilmek için kullanabileceğimiz yapılardan birisi referans metodlardır.

Kullanım şekli : 
```java
IcerenClass::MethodAdı
```
İki farklı kullanımı vardır:  

**Statik Method Referansı**
```java
interface TestMethod {
	void test();
}

public class MethodReference {
	public static void testRun () {
		System.out.println("Static test method");
	}
	
	public static void main (String[] args) {
		TestMethod tMethod = MethodReference::testRun;
		tMethod.test();
	}
}
```


**Statik Olmayan Method Referansı(Instance ve Constructor Referans)**

```java

interface TestMethod {
	void test();
}

public class MethodReference {
	public void testRun () {
		System.out.println("Static test method");
	}
	public static void main (String[] args) {
		MethodReference referance = new MethodReference();
		TestMethod tMethod = referance::testRun;
		tMethod.test();
	}
}
```

Basit bir örnek üzerinden gidersek bir öğrenci listemiz olsun ve bu liste içerisinden filtreleme işlemleri yapılacak şekilde bir kod hazırlayalım.
```java
public class Student {

	public enum Language { JAVA,JAVASCRIPT,PYTHON }

	private int age;
	private String name;
	private Language language;

	public int getAge() {
	return age;
	}

	public void setAge(int age) {
	this.age = age;
	}

	public String getName() {
	return name;
	}

	public void setName(String name) {
	this.name = name;
	}

	public Language getLanguage() {
	return language;
	}

	public void setLanguage(Language language) {
	this.language = language;
	}
	
	@Override
	public String toString() {
	return "Student [age=" + age + ", name=" + name + ", language=" + language + "]";

	}

}

```
Özellikleri kontrol edeceğimiz bir method interface :
```java
public interface Predicate<T>{
	boolean control(T t);
}
```
Dönüş değerimizin boolean olmasının nedeni şartı sağlayıp sağlamadığını kontrol etmemiz. Örnek olarak öğrencilerin java, python veya javascript öğrencisi olup olmadığını kontrol etmek istersek :
```java
public static boolean getJavaStudent(Student student) {
return Language.JAVA.equals(student.getLanguage());
}

public static boolean getPythonStudent(Student student) {
return Language.PYTHON.equals(student.getLanguage());
}

public static boolean getJavascriptStudent(Student student) {
return Language.JAVASCRIPT.equals(student.getLanguage());
}
```

Daha sonra filtreleme fonksiyonumuzu hazırlayalım. Bu fonksyon öğrenci tipinde objelerin olduğu bir list ve bizim istediğimiz koşulu parametre olarak almaktadır.
```java

static List<Student> filterStudent(List<Student> inventory,Predicate<Student> p){
	List<Student> students = new ArrayList<Student>();
	for(Student student:inventory) {
	if(p.control(student)) {
		students.add(student);
		}
	}
	return students;
}

```

Sınıfın genel yapısı :
```java
import java.util.ArrayList;
import java.util.List;

import method.Student.Language;

public class StudentUtils {

		public static boolean getJavaStudent(Student student) {
			return Language.JAVA.equals(student.getLanguage());
		}

		public static boolean getPythonStudent(Student student) {
			return Language.PYTHON.equals(student.getLanguage());
		}

		public static boolean getJavascriptStudent(Student student) {
			return Language.JAVASCRIPT.equals(student.getLanguage());
		}

		public static boolean getBiggerThanTwenty(Student student) {
			return student.getAge()>20;
		}

		public interface Predicate<T>{
			boolean control(T t);
		}

		static List<Student> filterStudent(List<Student> inventory,Predicate<Student> p){
			List<Student> students = new ArrayList<Student>();
			for(Student student:inventory) {
				if(p.control(student)) {
					students.add(student);
				}
			}
			return students;
		}

		static List<Student> createStudentList(){
			List<Student> inventory = new ArrayList<Student>();

			Student javaStudent = new Student();
			javaStudent.setAge(23);
			javaStudent.setName("Lorem");
			javaStudent.setLanguage(Language.JAVA);
			inventory.add(javaStudent);

			Student pythonStudent = new Student();
			pythonStudent.setAge(26);
			pythonStudent.setName("Ipsum");
			pythonStudent.setLanguage(Language.PYTHON);
			inventory.add(pythonStudent);

			Student javascriptStudent = new Student();
			javascriptStudent.setAge(30);
			javascriptStudent.setName("Dolor");
			javascriptStudent.setLanguage(Language.JAVASCRIPT);
			inventory.add(javascriptStudent);

			return inventory;
		}
}
```

Test :
```java

	List inventory = StudentUtils.createStudentList();
	
	List<Student> javaStudents = StudentUtils.filterStudent(inventory,StudentUtils::getJavaStudent);
	List<Student> pythonStudents = StudentUtils.filterStudent(inventory,StudentUtils::getPythonStudent);
	List<Student> javascriptStudents = StudentUtils.filterStudent(inventory,StudentUtils::getJavascriptStudent);

	System.out.println(javaStudents.get(0));
	System.out.println(pythonStudents.get(0));
	System.out.println(javascriptStudents.get(0));

```

	Student [age=23, name=Lorem, language=JAVA]

	Student [age=26, name=Ipsum, language=PYTHON]

	Student [age=30, name=Dolor, language=JAVASCRIPT]

