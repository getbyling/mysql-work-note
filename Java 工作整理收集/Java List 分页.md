Java List 分页
==============
>List 分页，初步版本

```java
package cn.gaiay.business.groupon.util;

import java.util.ArrayList;
import java.util.List;

public class ListPageUtil<E> {

    int pageNum;
    int pageSize;
    List<E> list;

    int totalCount;

    public ListPageUtil(List<E> list) {
        this(list, 15);
    }

    public ListPageUtil(List<E> list, int pageSize) {
        this(list, 1, pageSize);
    }

    public ListPageUtil(List<E> list, int start, int pageSize) {
        this.list = list;
        this.totalCount = list.size();
        this.pageNum = start;
        this.pageSize = pageSize;
    }

    public int getPageNum() {
        return pageNum;
    }

    public void setPageNum(int pageNum) {
        this.pageNum = pageNum;
    }

    public int getPageSize() {
        return pageSize;
    }

    public void setPageSize(int pageSize) {
        this.pageSize = pageSize;
    }

    public List<E> getList() {
        return list;
    }

    public void setList(List<E> list) {
        this.list = list;
    }

    public List<E> calcResult() {
        int pageCount = 0;
        int m = this.totalCount / this.pageSize;
        if (m > 0) {
            pageCount = m + 1;
        }

        List<E> result = new ArrayList<>();
        if (pageCount == 0) {
            if (this.pageNum <= 1) {
                result = this.list.subList(0, this.totalCount);
            }
        } else {
            if (pageCount > 0 && this.pageNum < 0) {
                this.pageNum = 1;
            }
            if (this.pageNum == pageCount) {
                result = this.list.subList((pageCount - 1) * this.pageSize, this.totalCount);
            } else if (this.pageNum < pageCount) {
                result = this.list.subList((this.pageNum - 1) * this.pageSize, this.pageSize * (this.pageNum));
            }
        }

        return result;
    }

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        for (int i = 1; i <= 20; i++) {
            list.add(i + "");
        }

        int page = 10;
        int pageSize = 15;
        ListPageUtil<String> listPage = new ListPageUtil(list, page, pageSize);
        System.out.println(listPage.calcResult());
    }
}

```