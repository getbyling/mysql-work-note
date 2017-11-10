RestTemplate post 请求传参
==========================
>RestTemplate 发送Post 多个参数请求
```java
/**
 * 改拼团订单地址，用于与订单服务交互
 *
 * @param orderId   String 	是	拼团订单
 * @param addressId String	是	用户地址Id
 * @description 参与拼团创建；迭代32功能
 */
public boolean updateOrderAddress(String orderId, long addressId) {
    if (StringUtil.isBlank(orderId)) {
        logger.warn("updateOrderAddress: 参数错误");
        return false;
    }
    logger.info("groupon update order address orderId:{}, addressId", new Object[]{orderId, addressId});

    String url = host + GROUPON_UP_ORDER_ADDRESS;
    Map<String, String> param = new HashMap<>();
    param.put("orderId", orderId);
    param.put("addressId", String.valueOf(addressId));

    String sign = SignUtil.getSig(param);
    param.put("sign", sign);

    boolean bResult = false;
    try {
        MultiValueMap<String, String> requestEntity = new LinkedMultiValueMap<>();
        requestEntity.add("orderId", orderId);
        requestEntity.add("addressId", String.valueOf(addressId));
        requestEntity.add("sign", sign);

        SuccessResult<String> result = restTemplate.postForObject(url, requestEntity, SuccessResult.class);
        if (result != null && result.getCode() == 0) {
            logger.info("groupon update order address,orderId:{},status:{}", orderId, result.getResult());

            Gson gson = new Gson();
            String json = gson.toJson(result.getResult());
            Map map = gson.fromJson(json, HashMap.class);
            if (map != null) {
                if ("0".equals(map.get("status").toString()) == true) {
                    bResult = true;
                }
            }
        }
    } catch (Exception e) {
        logger.error("updateOrderAddress error", e);
    }
    return bResult;
}
```
