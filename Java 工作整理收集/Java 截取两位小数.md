Java 截取两位小数
=================
>用字符转换两位小数，实用 -_- ！。。。
```java
public class subDouble {

    public static void main(String[] args) {

        double realPrice = 0.1 * (9 / 10.0);   //产品真实价

        System.out.println(String.format("1. output real price = %s", realPrice));

        //取产品真实价，两位小数
        String temp = String.valueOf(realPrice);
        if (temp.length() - temp.indexOf(".") >= 3) {
            temp = temp.substring(0, temp.indexOf(".") + 3);
        } else {
            temp = temp.substring(0, temp.indexOf(".") + 2);
        }
        realPrice = Double.valueOf(temp);

        System.out.println(String.format("2. output real price = %s", realPrice));
    }
}

```