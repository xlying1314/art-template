## 1.template-web是什么东西，有什么作用？

官方参考网址：http://aui.github.io/art-template/zh-cn/docs/

 答：这是一个模板引擎，简单来说就是构建一个模板，让其生成html的js代码。如果不用该js，手动来操作，我们可能需要繁杂的拼接html标签，还要做for循环等等，在没有使用前端框架之前，渲染数据相当之麻烦。

## 2.template-web要如何使用？

总的来说，template-web.js的使用分为3个步骤，

1.引入模板并制作模板 2.将数据插入模板中 3.将模板插入html代码中

   1. 引入模版文件 template-web.js

       <!-- 1.引入模板引擎 --> 

      <script src="lib/template-web.js"></script>

2. 创建引擎模版

   <!-- 2.创建引擎模板 这里type类型必须指定成text/html,id必须指定 -->

     <script type="text/html" id="template">
         <ul>
             <li>姓名{{name}}</li>
             <li>年龄{{age}}</li>
             <li>电话{{phone}}</li>
             <!-- 原始值输出即输出带标签形式的内容 -->
             <li>头像{{@ avator}}</li>
         </ul>
   </script>

3. 模板插入html代码中

   ```
   <script>
     // 获取数据
     const data = {
         name:"Petrel",
         age:"25",
         phone:"1377777777",
         avator:"<img src='https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic2.zhimg.com%2F50%2Fv2-7fe2ecb0e02ce14f92e8d6e10c6a3417_hd.jpg&refer=http%3A%2F%2Fpic2.zhimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1634109811&t=df5463cb1adb851d779b071d72008923'>"
     };
   
     // 将数据放入模板,并显示到页面中
     const res = template("template", data);// 此处template代表模板id,data表示要插入的数据
    
     document.write(res) // 显示结构
   </script>
   ```

    效果预览：

   ## ![image-20210913152756755](https://tva1.sinaimg.cn/large/008i3skNly1guf17jmk4sj60vm0f8mxw02.jpg)3.根据数据的不同，做if判断
   
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
         <meta charset="UTF-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Document</title>
         <!--引入模板引擎:一般用于在网络请求之后，展示相同的多条数据 -->
   <script type="text/javascript" src="lib/template-web.js" ></script>
   </head>
   <body>
     <div id="ifBox"></div>
   <!--
      1.逻辑语句---条件语句的使用
   -->
   <script type="text/html" id="personTemplate">
   <ul>
        {{if sex=="女"}}
     <li>姓名:{{name}} 女士
        <ol>
            <li>最新款的包包</li>
            <li>你真{{skill}}</li>
        </ol>
     </li>
       {{else if sex=="男"}}
     <li>姓名:{{name}} 先生
         <ol>
             <li>最新款的西装</li>
             <li>你真{{skill}}</li>
         </ol>
     </li>
     {{/if}}
   </ul>
   </script>
   <script type="text/javascript">
   
     //定义数据
     const person1 = {
         name:"赵丽颖",
         sex:"女",
         skill:"可爱"
     };
   
     const person2 = {
         name:"胡歌",
         sex:"男",
         skill:"帅气"
     };
   
     /**
      * 利用模板引擎 引用数据填充到模板中
      *
      * 参数一：模板id
      * 参数二：数据
      */
     var result =  template("personTemplate",person1);
     var result2 =  template("personTemplate",person2);
   
     //将返回的模板结果添加到界面中
     var ifBox = document.getElementById("ifBox");
     ifBox.innerHTML = result + result2;
   </script>
   </html>
   ```

   ![image-20210913153020976](https://tva1.sinaimg.cn/large/008i3skNly1guf1a1w56tj60nq0dy3z402.jpg)
   
   ##  4.数据渲染
   
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Document</title>
     <style>
       .current {
         margin-left:10px;
         list-style: none;
         border:1px solid black;
       }
     </style>
     <script src="./lib/jquery-1.12.4.js"></script>
     <script src="./lib/template-web.js"></script>
   </head>
   <body>
     <script type="text/html" id="templateId">
       <ul>
        <!-- 遍历brand数组，brand是请求返回数据中的数组 -->
       {{each brand}}
            <li>
               <a href="">{{$value.title}}</a>
               <ul >
                 {{each $value.text}}
                      <li class="current"> {{$value}}</li>
                 {{/each}}
               </ul>
           </li>
       {{/each}}
     </ul>
     </script>
     <script>
         $(function () { 
            $.ajax({
              url:'http://git.coding-future.com:3010/list/shopsort',
              dataType:'json',
              success (res) {
                const { data } = res
               // const { data: {brand}} = res
                console.log(data);
                const htmlStr = template('templateId',data)
                $("body").html(htmlStr)
              }
            })
         })
     </script>
   </body>
   </html>
   /**
    * 
    * @author Petrel&Hongyan_Li
    * @QQ：158452782
    * @TEL&&WX：137176-----
    * @created 2021/09/13 14:58:31
    */
   
   ```
   
   ![image-20210913153127424](https://tva1.sinaimg.cn/large/008i3skNly1guf1b77r1tj61jw0u0jvw02.jpg)
   
   参考数据：
   
   ```json
   {
     "status": 200,
     "message": "获取成功",
     "data": {
       "brand": [
         {
           "title": "所有品牌",
           "text": [
             "全部",
             "爱肯拿ACANA",
             "爱旺斯ADVANCE",
             "AVODerm",
             "爱诺Eminent",
             "巴西淘淘TOTAL",
             "Farmina",
             "海洋之星FISH4DOGS",
             "纽顿nutram number",
             "耐吉斯SOLUTION",
             "now FRESH",
             "欧帝亿IMPERIAL PAW",
             "primo",
             "生鲜本能Instinct",
             "天衡宝Natural Balance",
             "Tikipets"
           ]
         },
         {
           "title": "A",
           "text": [
             "爱肯拿ACANA",
             "爱旺斯ADVANCE",
             "AVODerm",
             "爱诺Eminent"
           ]
         },
         {
           "title": "B",
           "text": [
             "巴西淘淘TOTAL"
           ]
         },
         {
           "title": "F",
           "text": [
             "Farmina"
           ]
         },
         {
           "title": "H",
           "text": [
             "海洋之星FISH4DOGS"
           ]
         },
         {
           "title": "N",
           "text": [
             "纽顿nutram number",
             "耐吉斯SOLUTION",
             "now FRESH"
           ]
         },
         {
           "title": "O",
           "text": [
             "欧帝亿IMPERIAL PAW"
           ]
         },
         {
           "title": "P",
           "text": [
             "primo"
           ]
         },
         {
           "title": "S",
           "text": [
             "生鲜本能Instinct"
           ]
         },
         {
           "title": "T",
           "text": [
             "天衡宝Natural Balance",
             "Tikipets"
           ]
         }
       ],
       "alt": [
         {
           "name": "主要配方：",
           "title": [
             "鸡肉",
             "鱼肉",
             "禽肉",
             "羊肉",
             "鸭肉",
             "牛肉",
             "鹿肉",
             "其他"
           ]
         },
         {
           "name": "适合体型：",
           "title": [
             "全体型",
             "小型犬",
             "中型犬",
             "大型犬"
           ]
         },
         {
           "name": "适合年龄：",
           "title": [
             "全年龄犬",
             "成犬",
             "幼犬",
             "老犬",
             "怀孕哺乳期犬"
           ]
         },
         {
           "name": "颗粒大小：",
           "title": [
             "标准粒",
             "小颗粒",
             "大颗粒"
           ]
         },
         {
           "name": "重量区间：",
           "title": [
             "小包",
             "大包",
             "中包",
             "超小包",
             "超大包"
           ]
         },
         {
           "name": "单价：",
           "title": [
             "50元以上/斤",
             "40.1-50元/斤",
             "30.1-40元/斤",
             "25.1-30元/斤",
             "20.1-25元/斤",
             "15.1-20元/斤",
             "10.1-15元/斤"
           ]
         }
       ]
     }
   }
   ```
   
   

