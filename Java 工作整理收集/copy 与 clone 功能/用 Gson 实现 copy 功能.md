用 Gson 实现 copy 功能
======================

copy 数据功能
-------------

>用 Gson 将属性相近的两个对象，相互 copy 并赋值
```java
package cn.gaiay.business.util;

import com.google.gson.Gson;

public class GsonCopyTest {

    public static void main(String[] args) {

        EntityOne entityOne = new EntityOne();
        entityOne.setGrouponId("a1");
        entityOne.setProductId("aa1");
        entityOne.setProductName("aaaaa");
        
        Gson gson = new Gson();
        String entityOneJson = gson.toJson(entityOne);
        EntityTwo entityTwo = gson.fromJson(entityOneJson, EntityTwo.class);
        
    }
}
```

```java
package cn.gaiay.business.util;

/**
 * 测试实体 1
 *
 * @author lzg
 * @Date 2017年10月5日
 * @email linzhongguang@gaiay.cn
 */
public class EntityOne {

    String grouponId;           //拼团Id
    String productId;           //产品Id
    String productName;         //产品名称

    public String getGrouponId() {
        return grouponId;
    }

    public void setGrouponId(String grouponId) {
        this.grouponId = grouponId;
    }

    public String getProductId() {
        return productId;
    }

    public void setProductId(String productId) {
        this.productId = productId;
    }

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }
}
```

```java
package cn.gaiay.business.util;

/**
 * 测试实体 1
 *
 * @author lzg
 * @Date 2017年10月5日
 * @email linzhongguang@gaiay.cn
 */
public class EntityTwo {

    String grouponId;           //拼团Id
    String productId;           //产品Id

    public String getGrouponId() {
        return grouponId;
    }

    public void setGrouponId(String grouponId) {
        this.grouponId = grouponId;
    }

    public String getProductId() {
        return productId;
    }

    public void setProductId(String productId) {
        this.productId = productId;
    }
}
```
