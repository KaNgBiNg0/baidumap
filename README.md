# baidumap
画圈找企业项目


/* 点聚合 */
        /* stylesInMarker:[
          {
          url, //图片的url 地址必填
          size, // size 图片大小 必填项
          opt_anchor,
          textColor, 
          opt_textSize
           }
], */


/* 显示浮层的信息 目前好像没啥用 */
      showAddressDetail(data){
        let adds=[];
        for (let index = 0; index < data.length; index++) {
          let elementpt = data[index];
          adds.push(elementpt)
        }
        this.adds = adds;
        this.addindex = 0;
        this.bdGEO(0);
      },
      bdGEO() {
        var pt = this.adds[this.addindex];
        //geocodeSearch(pt);
        this.addindex++;
      },
      geocodeSearch(pt) {
        if (index < adds.length - 1) {
          setTimeout(window.bdGEO, 400);
        }
		    myGeo.getLocation(pt, function (rs) {
          var addComp = rs.addressComponents;
          var item =  "商圈:" + rs.business +
            " </br> 地址：" + addComp.province  + addComp.city  + addComp.district  + addComp.street +
            addComp.streetNumber + "";
          console.log(item)
          if(adds.length === 1){
            console.log('title')
            var comAddress = document.getElementById('comAddress')
            comAddress.innerHTML = item;
              /* 通过其查找 企业 */
          const dataAll = [
                {
                  lng:116.313082,lat:40.047674
                },
                {
                  lng:116.328749,lat:40.026922
                },
                {
                  lng:116.347571,lat:39.988698
                },
                {
                  lng:116.316163,lat:39.998333
                },
                {
                  lng:116.313076,lat:40.059011
                },
              ]
              var mockBottom = document.getElementById('mockBottom')
              mockBottom.innerHTML ='';
                showAddressDetail(dataAll);

          }else{
            createEle(item)
          }
        });
      },