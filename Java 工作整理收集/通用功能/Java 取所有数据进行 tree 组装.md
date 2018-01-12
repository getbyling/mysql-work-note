Java 取所有数据进行 tree 组装
=============================

>组装 tree 的实现算法
```java
package cn.gaiay.business.zm.live.category.biz.impl;

import cn.gaiay.business.zm.live.category.dao.LivePlateformTypeMapper;
import cn.gaiay.business.zm.live.category.model.dto.HomePlateTypeDTO;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.google.gson.Gson;
import org.apache.commons.beanutils.BeanUtils;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import java.lang.reflect.InvocationTargetException;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ActiveProfiles("dev")
@WebAppConfiguration
@ContextConfiguration(locations = {"/application.xml"})
public class LivePlateformTypeServiceImplTest {

    private static final Logger logger = LoggerFactory.getLogger(LivePlateformTypeServiceImplTest.class);

    @Autowired
    private LivePlateformTypeMapper livePlateformTypeMapper;

    @Test
    public void assembleTree() throws Exception {

        Page pageInfo = PageHelper.offsetPage(0, Integer.MAX_VALUE, false);
        List<HomePlateTypeDTO> typeList = livePlateformTypeMapper.getAllPlatformTypeList();

        List<HomePlateTypeDTO> assembleTree = new ArrayList<>();
        recursiveAssembleTree(typeList, "sdf15", assembleTree);

        Gson gson = new Gson();
        System.out.println("assembleTree = " + gson.toJson(assembleTree));
    }

    /**
     * 递归组装 tree
     */
    private void recursiveAssembleTree(List<HomePlateTypeDTO> typeList, String parentCode, List<HomePlateTypeDTO> assembleTree)
            throws InvocationTargetException, NoSuchMethodException, InstantiationException, IllegalAccessException {
        if (typeList != null && typeList.size() > 0) {
            List<HomePlateTypeDTO> filterTypeList = typeList.stream().filter(t -> parentCode.equals(t.getParentCode())).collect(Collectors.toList());
            if (filterTypeList != null && filterTypeList.size() > 0) {
                for (HomePlateTypeDTO item : filterTypeList) {
                    HomePlateTypeDTO temp = (HomePlateTypeDTO) BeanUtils.cloneBean(item);

                    int iFlag = assembleTree.stream().filter(a -> temp.getCode().equals(a.getCode())).collect(Collectors.toList()).size();
                    if (iFlag <= 0) {
                        assembleTree.add(temp);
                        if (temp.getChildren() == null) {
                            temp.setChildren(new ArrayList<>());
                        }
                        //递归调用
                        recursiveAssembleTree(typeList, item.getCode(), temp.getChildren());
                    }
                }
            }
        }
    }
}
```