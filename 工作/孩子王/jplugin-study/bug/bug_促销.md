##查询促销详情
```
// productQueryBySkuId商品
                     if  ( prplist .get(0).getProductIncludeList().size() > 0) {
                         for  (uint64_t  product  :  prplist .get(0).getProductIncludeList()) {
                             includeSku .add( "("  + Long. toString ( product .longValue()) +  ")"
                                    + GetSkuById( product .longValue()).getOrdinaryInfo().getSkuTitle());
                        }                     }  
  GetSkuById( product .longValue()):NullPointException

```
##创建奇偶促销
```
折扣设置了参数，但是查询不出来。
```


0314：
1.换购和赠品，在线下的时候要支持erp编码
2.商品范围 css优化


skuQuery.do
var entityType = $("input[name='entityRadio']:checked").val();//参与实体方式
var chnnels = $("input[name='saleWay']");
var channelInclude = [];
for(ch in chnnels){
    if(chnnels[ch].checked){
        channelInclude.push(chnnels[ch].value);
    }
}
channelInclude = JSON.stringify(channelInclude);

,"entityType":entityType,"channelInclude":channelInclude



if(data.tag != 1){
                        if(siteprice!=undefined){
                            if(typeof(skuPic) == "undefined" || skuPic.length == 0){
                                alert("该商品没有图片信息!请维护图片信息!");
                                return false;
                            }
                            for(var j=0;j<siteprice.length;j++){
                                if(skuPic[j]["picUrl"] == ""){
                                    alert("图片Url为空!请检查图片信息!");
                                    return false;
                                }
                                dlgHtml.append("<tr><td>" + skuId + "</td><td>" + skuname + "</td>"
                                        +"<td>"+ getYuanByFen(siteprice[j]["areaPrice"]) +"</td>"
                                        +"<td><input name='itemNum' onblur='checkItemNum(this)' type='text' value='1'/><input type='hidden' value='"+skuPic[j]["picUrl"]+"'/>"
                                        +"<input type='hidden' value='"+sku_erpcode+"'/></td>"
                                        +"<td><input type='button' onclick='delGift(this, 0)' value='删除' /></td></tr>");
                            }
                            
                            $("#addGiftRule").removeAttr("disabled");
                        }else{
                            alert("没有查询到商品");
                            return false;
                        }
                    }else{
                        dlgHtml.append("<tr><td>" + skuId + "</td><td>" + skuname + "</td>"
                                +"<td>"+ getYuanByFen(skuinfo["skuReferPrice"]) +"</td>"
                                +"<td><input name='itemNum' onblur='checkItemNum(this)' type='text' value='1'/></td>"
                                +"<td><input type='button' onclick='delGift(this, 0)' value='删除' /></td></tr>");
                    }
