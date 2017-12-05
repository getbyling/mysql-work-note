Java BeanUtils 使用
===================
>可以很优雅的实现将父类字段的值copy到子类中

```java
import org.springframework.beans.BeanUtils;

public class Test {

    public static void main(String[] args) {
        Person person = new Person("people");
        Male male = new Male();
        BeanUtils.copyProperties(person, male);
        
        System.out.println(Male.class.isInstance(male));
        System.out.println(male);
    }
}

class Person {
    private String name;

    public Person() {
    }

    public Person(String name) {
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
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Male extends Person {
    private String necklace;

    public Male() {
    }

    public Male(String name) {
        super(name);
    }

    public String getNecklace() {
        return necklace;
    }

    public void setNecklace(String necklace) {
        this.necklace = necklace;
    }
}
```