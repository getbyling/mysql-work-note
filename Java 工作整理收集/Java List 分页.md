Java List 分页
==============
>List 分页，初步版本

```java
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
        int m = this.totalCount % this.pageSize;
        if (m > 0) {
            pageCount = this.totalCount / this.pageSize + 1;
        } else {
            pageCount = this.totalCount / this.pageSize;
        }

        List<E> result = new ArrayList<>();
        if (m == 0) {
            result = this.list.subList((this.pageNum - 1) * this.pageSize, this.pageSize * (this.pageNum));
        } else {
            if (this.pageNum == pageCount) {
                result = this.list.subList((this.pageNum - 1) * this.pageSize, this.totalCount);
            } else {
                result = this.list.subList((this.pageNum - 1) * this.pageSize, this.pageSize * (this.pageNum));
            }
        }

        return result;
    }
}
```