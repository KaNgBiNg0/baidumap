1. 根据不同比例获得当前地图区域坐标点信息的接口
```
    // req.params
    {
    // 可是区域 起始纬度，起始经度，结束纬度，结束经度
     area:[lat1,long1,lat2,long2],   
     // 层级 国家country=0，省province=1，市city=2, 区县District=3，街道street=4，点坐标point=5
     level: 0/1/2/3/4/5
    }
    
    // res level为 0-4 的
    {
        "text": "显示用的文本",
        "lat": "纬度",
        "long": "经度",
        "count": "数量"
    }
    
    // res level为 5 的 
    {
        "text": "显示用的文本",
        "lat": "纬度",
        "long": "经度",
        "count": "数量",
        
        // 新增的
        "data":["组织机构代码1组织结构代码2"...]
    }
```

2. 点击点坐标和画圈以后显示选中单位所需要的的单位详细信息接口
```
    
    // req.params
    {
        // data的长度不超过单页显示长度, 每页数据页面缓存
        data: ["组织机构代码1组织结构代码2"...]
    }
    
    // req
    
    {
        data:[{
            // 列表展示使用
          "name":"长安福特",
          "businessType":"新能源",
          "staffs": "员工人数",
          "companyType":"国企",
          "address":"地址",
          "phone":"电话",
    
          // 数据查询使用
          "code": "组织机构代码"
        }...]
    }

```

1. 获得右侧筛选条件（行业，规模，指标）的接口？？？

---

# 组件

## 查询框组件 searchCompanyInput

prop => formInline searchByStationName

v-model => formInline.address // 待查询输入文本

@click => searchByStationName // 调用输入接口

## 条件过滤组件 searchKeyCon

prop => addtags removetags drawing isInDrawing nodrawering

@click => addtags // 增加筛选条件

@click => removetags // 删除筛选条件

@click => drawing // 开启画图找企业功能

@click => nodrawing // 关闭画图找企业功能

v-show => isInDrawing // 切换文本

## 候选单位基本信息组件 conpanyListPanel 

prop => list(polyPointArray) querData

list // 展示数据使用，判断是否展示该组件

@click => querData // 查询数据（拼接iframe）

## 单位数据信息组件 conpanyDataPanel 

prop => list(polyPointArray) isShowCompanyData closeCompanyDataPanel

list // 展示数据使用

v-show => isShowCompanyData // 是否展示该组件

@click => closeCompanyDataPanel // 通知vue已经关闭该组件

