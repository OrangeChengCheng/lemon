<template>
  <div class="hello">
    <el-progress :percentage="rate"></el-progress>

    <div v-html="htmlJson"></div>

  </div>
</template>

<script>
    let Base64 = require('js-base64').Base64

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
    data () {
        return {
            currentNum: 50,
            totalNum: 100,
            htmlJson:''
        }
    },
    mounted() {
        // let url = 'https://rosefinch.test1.bestpay.net/groups/getGroupsByType?type=group';
        //
        // var strs = new Array(); //定义一数组
        // strs = url.split("?"); //字符分割
        // window.console.log('==================', strs);


        let encodeStr = 'LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLTkzNDUyMzAwMDAyMzM1MDUyNTkzMzgwMw0KQ29udGVudC1EaXNwb3NpdGlvbjogZm9ybS1kYXRhOyBuYW1lPSJuYW1lIg0KDQp3YW5nDQotLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tOTM0NTIzMDAwMDIzMzUwNTI1OTMzODAzDQpDb250ZW50LURpc3Bvc2l0aW9uOiBmb3JtLWRhdGE7IG5hbWU9ImFnZSINCg0KMjANCi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS05MzQ1MjMwMDAwMjMzNTA1MjU5MzM4MDMNCkNvbnRlbnQtRGlzcG9zaXRpb246IGZvcm0tZGF0YTsgbmFtZT0iZGF0YSINCg0KMjAyMC0wODA4DQotLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tOTM0NTIzMDAwMDIzMzUwNTI1OTMzODAzLS0NCg==';
        let contentType = 'multipart/form-data; boundary=--------------------------934523000023350525933803';
        let decodeDataStr = Base64.decode(encodeStr);
        window.console.log('===== encodeStr =====\n', encodeStr);
        window.console.log('===== contentType =====\n', contentType);
        window.console.log('===== decodeDataStr =====\n', decodeDataStr);

        let boundaryKey = '';///分隔key
        if (contentType.indexOf("boundary") != -1) {
            ///获取分隔key
            let boundaryKeyListTemp = contentType.split(';');
            let boundaryKeyStrTemp = boundaryKeyListTemp[1];
            boundaryKeyListTemp = boundaryKeyStrTemp.split('=');
            boundaryKey = boundaryKeyListTemp[1];
            window.console.log('===== boundaryKey =====\n', boundaryKey);
        }
        if (!boundaryKey.length) {
            window.console.log('boundary不存在，参数异常');
            return;
        }

        ///去除 \r\n
        decodeDataStr = decodeDataStr.replace(/\r\n/g, '');
        window.console.log('===== decodeDataStr =====\n', decodeDataStr);

        ///按照boundaryKey分隔数据
        let paramStrListTemp = decodeDataStr.split(boundaryKey);
        window.console.log('===== paramStrListTemp =====\n', paramStrListTemp);
        let paramStrList = [];
        for (let strTemp of paramStrListTemp) {
            if (strTemp.indexOf('name') != -1) {
                strTemp = strTemp.replace('Content-Disposition: form-data; ', '');
                strTemp = strTemp.replace('--', '');
                strTemp = strTemp.replace('name="', '');
                paramStrList.push(strTemp);
            }
        }
        window.console.log('===== paramStrList =====\n', paramStrList);

        ///参数重组对象
        let paramData = {};
        for (let strTemp of paramStrList) {
            let strListTemp = strTemp.split('"');
            paramData[strListTemp[0]] = strListTemp[1];
        }
        window.console.log('===== paramData =====\n', paramData);




    },
    computed: {
        rate () {
            return this.currentNum / this.totalNum * 100;
        }
    },

    methods: {

    },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .hello {
    display: flex;
    flex-direction: column;
  }
</style>
