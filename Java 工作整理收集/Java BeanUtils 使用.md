Java BeanUtils 使用
===================

>可以很优雅的实现将父类字段的值copy到子类中

org.springframework.beans.BeanUtils 方法的使用
----------------------------------------------
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

org.apache.commons.beanutils.BeanUtils  方法的使用
--------------------------------------------------
>可复制对象值
```java
import org.apache.commons.beanutils.BeanUtils;
import cn.gaiay.business.groupon.entity.member.MemberProductVO;

public class GrouponMemberServiceImpl {
    
    public static void main(String[] args) {

        MemberProductVO memberProduct = new MemberProductVO();
        memberProduct.setProductPic("a");
        try {
            MemberProductVO temp = (MemberProductVO) BeanUtils.cloneBean(memberProduct);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }

    }
}
```
>MemberProductVO 实体
```java
package cn.gaiay.business.groupon.entity.member;

import cn.gaiay.business.groupon.entity.PriceInfo;

import java.util.List;

/**
 * 成员拼团产品信息 VO 实体
 *
 * @author lzg
 * @Date 2017年10月5日
 * @email linzhongguang@gaiay.cn
 */
public class MemberProductVO {

    String grouponId;           //拼团Id
    String productId;           //产品Id

    public String getGrouponId() {
        return grouponId;
    }

    public String getProductId() {
        return productId;
    }

    public void setGrouponId(String grouponId) {
        this.grouponId = grouponId;
    }

    public void setProductId(String productId) {
        this.productId = productId;
    }
}
```
