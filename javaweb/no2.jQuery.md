[toc]

# jQuery

javaScript 和 查询(Query)，它就是辅助javaScript开发的js类库
相比于js：写的更少，做的更多

~~~html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
	<script type="text/javascript" src="../script/jquery-1.7.2.js"> 
        // 使用jQuery 一定要引入jQuery库
    </script>
	<script type="text/javascript">
		// window.onload = function () {
		// 	var btnObj = document.getElementById("btnId");
		// 	alert(btnObj);//[object HTMLButtonElement]   ====>>>  dom对象
		// 	btnObj.onclick = function () {
		// 		alert("js 原生的单击事件");
		// 	}
		// }

		$(function () { 
            // $()是一个函数，表示页面加载完成之后，$(function(){}) 相当 window.onload = function(){}
			var $btnObj = $("#btnId"); // 表示按id查询标签对象
			$btnObj.click(function () { // 绑定单击事件
				alert("jQuery 的单击事件");
			});
		});
	</script>
</head>
<body>
	<button id="btnId">SayHello</button>
</body>
</html>
~~~

## jQuery核心函数

$ 是jQuery的核心函数，能完成jQuery的很多功能。

* $() 传入参数为 （函数）
  表示页面加载完成之后，相当于window.onload=function(){}

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
  	$(function(){
  		alert($);
  	});
  </script>
  </head>
  <body>
      
  </body>
  </html>
  ~~~

  

* $() 传入参数为（HTML 字符串）
  创建这个html标签对象

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
      $(function () {
          alert( $("<h1></h1>") );
      });
  </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

  

* $() 传入参数为（选择器字符串）

  \> $("#id 属性值");	id选择器，根据id查询标签对象
  \> $("标签名");	标签名选择器，根据指定的标签名查询标签对象
  \> $(".class 属性值");	类型选择器，可以根据class属性查询标签对象

  

* $() 传入参数为（DOM对象）
  把这个DOM对象转化成jQuery对象

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
      $(function () {
          var btnObj = document.getElementById("btn01");
          alert(btnObj);
          alert( $(btnObj) );
      });
  </script>
  </head>
  <body>
      <button id="btn01">按钮1</button>
  </body>
  </html>
  ~~~

  

## jQuery 对象 和 dom 对象

jQuery对象是dom对象的数组 + jQuery提供的一系列功能函数

* DOM对象：	[object HTML标签名Element]
  通过诸多方法返回的标签对象都是DOM对象
* jQuery对象： [object object]
  通过jQuery提供的API返回的对象、包装的DOM对象都是jQuery对象

* DOM 和 jQuery 区别
  jQuery对象不能使用DOM对象的属性和方法
  DOM对象不能使用jQuery对象的属性和方法

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
  	$(function(){
  		document.getElementById("testDiv").innerHTML = "这是dom对象的属性InnerHTML";
          //jQuery对象用DOM的属性和方法
  		$("#testDiv").innerHTML = "这是dom对象的属性InnerHTML";
  
  		$("#testDiv").click(function () {
  			alert("click()是jQuery对象的方法");
  		});      
  		//DOM对象用jQuery的属性和方法
  		document.getElementById("testDiv").click(function () {
  			alert("click()是jQuery对象的方法");
  		});
  	});
  </script>
  </head>
  <body>
  	<div id="testDiv">Atguigu is Very Good!</div>
  </body>
  </html>
  ~~~

  

### DOM 和 jQuery 相互转换

![image-20210902154808732](E:\软工\typora笔记\javaweb\jQuery.assets\image-20210902154808732.png)

* dom对象 ===>> jQuery对象	

  > 语法：jQuery对象 ：$(DOM对象) 

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
  	$(function(){
  		alert( $(document.getElementById("testDiv")));
  	});
  </script>
  </head>
  <body>
  	<div id="testDiv">Atguigu is Very Good!</div>
  </body>
  </html>
  ~~~

  

* jQuery对象 ===>> dom对象

  > 语法：DOM对象 ： jQuery对象[下标] 

  ~~~html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
  	$(function(){
  		alert( $(document.getElementById("testDiv"))[0]);
  	});
  </script>
  </head>
  <body>
  	<div id="testDiv">Atguigu is Very Good!</div>
  </body>
  </html>
  ~~~

## jQuery选择器

### 基本选择器

| 基本选择器          | 说明                             |
| ------------------- | -------------------------------- |
| #ID                 | 根据id查找标签对象               |
| .class              | 根据class查找标签对象            |
| element             | 根据标签名查找标签对象           |
| *                   | 表示任意的，所有的元素           |
| selector1,selector2 | 合并选择器1和选择器2的结果并返回 |

> css() 方法 可以设置和获取样式

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			div, span, p {
			    width: 140px;
			    height: 140px;
			    margin: 5px;
			    background: #aaa;
			    border: #000 1px solid;
			    float: left;
			    font-size: 17px;
			    font-family: Verdana;
			}
			
			div.mini {
			    width: 55px;
			    height: 55px;
			    background-color: #aaa;
			    font-size: 12px;
			}
			
			div.hide {
			    display: none;
			}
		</style>
		<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
			
				$(function () {
					//1.选择 id 为 one 的元素 "background-color","#bbffaa"
					$("#btn1").click(function () {
						$("#one").css("background-color","#bbffaa");
					});


					//2.选择 class 为 mini 的所有元素
					$("#btn2").click(function () {
						$(".mini").css("background-color","#bbffaa");
					});

					//3.选择 元素名是 div 的所有元素
					$("#btn3").click(function () {
						$("div").css("background-color","#bbffaa");
					});

					//4.选择所有的元素
					$("#btn4").click(function () {
						$("*").css("background-color","#bbffaa");
					});

					//5.选择所有的 span 元素和id为two的元素
					$("#btn5").click(function () {
						$("span,#two").css("background-color","#bbffaa");
					});

				});

		</script>
	</head>
	<body>
<!-- 	<div>
		<h1>基本选择器</h1>
	</div>	 -->	
		<input type="button" value="选择 id 为 one 的元素" id="btn1" />
		<input type="button" value="选择 class 为 mini 的所有元素" id="btn2" />
		<input type="button" value="选择 元素名是 div 的所有元素" id="btn3" />
		<input type="button" value="选择 所有的元素" id="btn4" />
		<input type="button" value="选择 所有的 span 元素和id为two的元素" id="btn5" />
		
		<br>
		<div class="one" id="one">
			id 为 one,class 为 one 的div
			<div class="mini">class为mini</div>
		</div>
		<div class="one" id="two" title="test">
			id为two,class为one,title为test的div
			<div class="mini" title="other">class为mini,title为other</div>
			<div class="mini" title="test">class为mini,title为test</div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini"></div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini" title="tesst">class为mini,title为tesst</div>
		</div>
		<div style="display:none;" class="none">style的display为"none"的div</div>
		<div class="hide">class为"hide"的div</div>
		<div>
			包含input的type为"hidden"的div<input type="hidden" size="8">
		</div>
		<span class="one" id="span">^^span元素^^</span>
	</body>
</html>
~~~



### 层级选择器

| 层级选择器          | 说明                                                     |
| ------------------- | -------------------------------------------------------- |
| ancestor descendant | 后代选择器：在给定的祖先元素下匹配所有的后代元素         |
| parent > child      | 子元素选择器：在给定的父元素下匹配所有的子元素           |
| prev + next         | 相邻元素选择器：匹配所有紧接在prev元素后的next元素       |
| prev ~ siblings     | 之后的兄弟元素选择器：匹配prev元素之后的所有siblings元素 |

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			div, span, p {
			    width: 140px;
			    height: 140px;
			    margin: 5px;
			    background: #aaa;
			    border: #000 1px solid;
			    float: left;
			    font-size: 17px;
			    font-family: Verdana;
			}
			
			div.mini {
			    width: 55px;
			    height: 55px;
			    background-color: #aaa;
			    font-size: 12px;
			}
			
			div.hide {
			    display: none;
			}			
		</style>
		<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(document).ready(function(){
				//1.选择 body 内的所有 div 元素
				$("#btn1").click(function(){
					$("body div").css("background", "#bbffaa");
				});

				//2.在 body 内, 选择div子元素  
				$("#btn2").click(function(){
					$("body > div").css("background", "#bbffaa");
				});

				//3.选择 id 为 one 的下一个 div 元素 
				$("#btn3").click(function(){
					$("#one+div").css("background", "#bbffaa");
				});

				//4.选择 id 为 two 的元素后面的所有 div 兄弟元素
				$("#btn4").click(function(){
					$("#two~div").css("background", "#bbffaa");
				});
			});
		</script>
	</head>
	<body>	
	
<!-- 	<div>
		<h1>层级选择器:根据元素的层级关系选择元素</h1>
		ancestor descendant  ：
		parent > child 		   ：
		prev + next 		   ：
		prev ~ siblings 	   ：
	</div>	 -->
		<input type="button" value="选择 body 内的所有 div 元素" id="btn1" />
		<input type="button" value="在 body 内, 选择div子元素" id="btn2" />
		<input type="button" value="选择 id 为 one 的下一个 div 元素" id="btn3" />
		<input type="button" value="选择 id 为 two 的元素后面的所有 div 兄弟元素" id="btn4" />
		<br><br>
		<div class="one" id="one">
			id 为 one,class 为 one 的div
			<div class="mini">class为mini</div>
		</div>
		<div class="one" id="two" title="test">
			id为two,class为one,title为test的div
			<div class="mini" title="other">class为mini,title为other</div>
			<div class="mini" title="test">class为mini,title为test</div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini"></div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini" title="tesst">class为mini,title为tesst</div>
		</div>
		<div style="display:none;" class="none">style的display为"none"的div</div>
		<div class="hide">class为"hide"的div</div>
		<div>
			包含input的type为"hidden"的div<input type="hidden" size="8">
		</div>
		<span id="span">^^span元素^^</span>
	</body>
</html>
~~~



### 过滤选择器

* 基本过滤器	

  | 基本过滤器     | 说明                                    |
  | -------------- | --------------------------------------- |
  | :first         | 获取第一个元素                          |
  | :last          | 获取最后一个元素                        |
  | :not(selector) | 去除所有与给定选择器匹配的元素          |
  | :even          | 匹配索引值为偶数的元素，从0开始计数     |
  | :odd           | 匹配所有索引值为奇数的元素，从0开始计数 |
  | :eq(index)     | 匹配一个给定索引值的元素                |
  | :gt(index)     | 匹配所有大于给定索引值的元素            |
  | :lt(index)     | 匹配所有小于给定索引值的元素            |
  | :header        | 匹配如 h1、h2、h3等标题元素             |
  | :animated      | 匹配所有正在执行动画效果的元素          |

  ~~~html
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  <html>
  	<head>
  		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  		<title>Untitled Document</title>
  		<style type="text/css">
  			div, span, p {
  			    width: 140px;
  			    height: 140px;
  			    margin: 5px;
  			    background: #aaa;
  			    border: #000 1px solid;
  			    float: left;
  			    font-size: 17px;
  			    font-family: Verdana;
  			}
  			
  			div.mini {
  			    width: 55px;
  			    height: 55px;
  			    background-color: #aaa;
  			    font-size: 12px;
  			}
  			
  			div.hide {
  			    display: none;
  			}			
  		</style>
  		<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  		<script type="text/javascript">
  			$(document).ready(function(){
  				function anmateIt(){
  					$("#mover").slideToggle("slow", anmateIt);
  				}
  				anmateIt();
  			});
  			
  			$(document).ready(function(){
  				//1.选择第一个 div 元素  
  				$("#btn1").click(function(){
  					$("div:first").css("background", "#bbffaa");
  				});
  
  				//2.选择最后一个 div 元素
  				$("#btn2").click(function(){
  					$("div:last").css("background", "#bbffaa");
  				});
  
  				//3.选择class不为 one 的所有 div 元素
  				$("#btn3").click(function(){
  					$("div:not(.one)").css("background", "#bbffaa");
  				});
  
  				//4.选择索引值为偶数的 div 元素
  				$("#btn4").click(function(){
  					$("div:even").css("background", "#bbffaa");
  				});
  
  				//5.选择索引值为奇数的 div 元素
  				$("#btn5").click(function(){
  					$("div:odd").css("background", "#bbffaa");
  				});
  
  				//6.选择索引值为大于 3 的 div 元素
  				$("#btn6").click(function(){
  					$("div:gt(3)").css("background", "#bbffaa");
  				});
  
  				//7.选择索引值为等于 3 的 div 元素
  				$("#btn7").click(function(){
  					$("div:eq(3)").css("background", "#bbffaa");
  				});
  
  				//8.选择索引值为小于 3 的 div 元素
  				$("#btn8").click(function(){
  					$("div:lt(3)").css("background", "#bbffaa");
  				});
  
  				//9.选择所有的标题元素
  				$("#btn9").click(function(){
  					$(":header").css("background", "#bbffaa");
  				});
  
  				//10.选择当前正在执行动画的所有元素
  				$("#btn10").click(function(){
  					$(":animated").css("background", "#bbffaa");
  				});
  				//11.选择没有执行动画的最后一个div
  				$("#btn11").click(function(){
  					$("div:not(:animated):last").css("background", "#bbffaa");
  				});
  			});
  		</script>
  	</head>
  	<body>
  		<input type="button" value="选择第一个 div 元素" id="btn1" />
  		<input type="button" value="选择最后一个 div 元素" id="btn2" />
  		<input type="button" value="选择class不为 one 的所有 div 元素" id="btn3" />
  		<input type="button" value="选择索引值为偶数的 div 元素" id="btn4" />
  		<input type="button" value="选择索引值为奇数的 div 元素" id="btn5" />
  		<input type="button" value="选择索引值为大于 3 的 div 元素" id="btn6" />
  		<input type="button" value="选择索引值为等于 3 的 div 元素" id="btn7" />
  		<input type="button" value="选择索引值为小于 3 的 div 元素" id="btn8" />
  		<input type="button" value="选择所有的标题元素" id="btn9" />
  		<input type="button" value="选择当前正在执行动画的所有元素" id="btn10" />		
  		<input type="button" value="选择没有执行动画的最后一个div" id="btn11" />
  
  
  		<h3>基本选择器.</h3>
  		<br><br>
  		<div class="one" id="one">
  			id 为 one,class 为 one 的div
  			<div class="mini">class为mini</div>
  		</div>
  		<div class="one" id="two" title="test">
  			id为two,class为one,title为test的div
  			<div class="mini" title="other">class为mini,title为other</div>
  			<div class="mini" title="test">class为mini,title为test</div>
  		</div>
  		<div class="one">
  			<div class="mini">class为mini</div>
  			<div class="mini">class为mini</div>
  			<div class="mini">class为mini</div>
  			<div class="mini"></div>
  		</div>
  		<div class="one">
  			<div class="mini">class为mini</div>
  			<div class="mini">class为mini</div>
  			<div class="mini">class为mini</div>
  			<div class="mini" title="tesst">class为mini,title为tesst</div>
  		</div>
  		<div style="display:none;" class="none">style的display为"none"的div</div>
  		<div class="hide">class为"hide"的div</div>
  		<div>
  			包含input的type为"hidden"的div<input type="hidden" size="8">
  		</div>
  		<div id="mover">正在执行动画的div元素.</div>
  	</body>
  </html>
  
  
  ~~~

  

* 内容过滤器

  | 内容过滤器      | 说明                                                     |
  | --------------- | -------------------------------------------------------- |
  | :contains(text) | 匹配包含给定文本的元素                                   |
  | :empty          | 匹配所有不包含子元素或者文本的空元素                     |
  | :parent         | 匹配含有子元素或者文本的元素                             |
  | :has(selector)  | 匹配含有选择器所匹配的元素的元素【包含某子元素的父元素】 |

  ```html
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  <html>
     <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Untitled Document</title>
        <style type="text/css">
           div, span, p {
               width: 140px;
               height: 140px;
               margin: 5px;
               background: #aaa;
               border: #000 1px solid;
               float: left;
               font-size: 17px;
               font-family: Verdana;
           }
           
           div.mini {
               width: 55px;
               height: 55px;
               background-color: #aaa;
               font-size: 12px;
           }
           
           div.hide {
               display: none;
           }        
        </style>
        <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
        <script type="text/javascript">
           $(document).ready(function(){
              function anmateIt(){
                 $("#mover").slideToggle("slow", anmateIt);
              }
     
              anmateIt();             
           });
  
           $(document).ready(function(){
              //1.选择 含有文本 'di' 的 div 元素
              $("#btn1").click(function(){
                 $("div:contains('di')").css("background", "#bbffaa");
              });
  
              //2.选择不包含子元素(或者文本元素) 的 div 空元素
              $("#btn2").click(function(){
                 $("div:empty").css("background", "#bbffaa");
              });
  
              //3.选择含有 class 为 mini 元素的 div 元素
              $("#btn3").click(function(){
                 $("div:has(.mini)").css("background", "#bbffaa");
              });
  
              //4.选择含有子元素(或者文本元素)的div元素
              $("#btn4").click(function(){
                 $("div:parent").css("background", "#bbffaa");
              });
           });
        </script>
     </head>
     <body>    
        <input type="button" value="选择 含有文本 'di' 的 div 元素" id="btn1" />
        <input type="button" value="选择不包含子元素(或者文本元素) 的 div 空元素" id="btn2" />
        <input type="button" value="选择含有 class 为 mini 元素的 div 元素" id="btn3" />
        <input type="button" value="选择含有子元素(或者文本元素)的div元素" id="btn4" />
        
        <br><br>
        <div class="one" id="one">
           id 为 one,class 为 one 的div
           <div class="mini">class为mini</div>
        </div>
        <div class="one" id="two" title="test">
           id为two,class为one,title为test的div
           <div class="mini" title="other">class为mini,title为other</div>
           <div class="mini" title="test">class为mini,title为test</div>
        </div>
        <div class="one">
           <div class="mini">class为mini</div>
           <div class="mini">class为mini</div>
           <div class="mini">class为mini</div>
           <div class="mini"></div>
        </div>
        <div class="one">
           <div class="mini">class为mini</div>
           <div class="mini">class为mini</div>
           <div class="mini">class为mini</div>
           <div class="mini" title="tesst">class为mini,title为tesst</div>
        </div>
        <div style="display:none;" class="none">style的display为"none"的div</div>
        <div class="hide">class为"hide"的div</div>
        <div>
           包含input的type为"hidden"的div<input type="hidden" size="8">
        </div>
        <div id="mover">正在执行动画的div元素.</div>
     </body>
  </html>
  ```

* 属性过滤器

  | 属性过滤器                      | 说明                                               |
  | ------------------------------- | -------------------------------------------------- |
  | [attribute]                     | 匹配包含给定属性的元素                             |
  | [attribute=value]               | 匹配给定的属性是某个特定值的元素                   |
  | [attribute!=value]              | 匹配所有不含指定的属性，或者属性不等于特定值的元素 |
  | [attribute^=value]              | 匹配给定的属性是以某些值开始的元素                 |
  | [attribute$=value]              | 匹配给定的属性是以某些值结尾的元素                 |
  | [attribute*=value]              | 匹配给定的属性是以包含某些值的元素                 |
  | [attrSel1]\[attrSel2][attrSelN] | 复合属性选择器，需要同时满足多个条件时使用         |

  ```html
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <style type="text/css">
  div,span,p {
     width: 140px;
     height: 140px;
     margin: 5px;
     background: #aaa;
     border: #000 1px solid;
     float: left;
     font-size: 17px;
     font-family: Verdana;
  }
  
  div.mini {
     width: 55px;
     height: 55px;
     background-color: #aaa;
     font-size: 12px;
  }
  
  div.hide {
     display: none;
  }
  </style>
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
     $(function() {
        //1.选取含有 属性title 的div元素
        $("#btn1").click(function() {
           $("div[title]").css("background", "#bbffaa");
        });
        //2.选取 属性title值等于'test'的div元素
        $("#btn2").click(function() {
           $("div[title='test']").css("background", "#bbffaa");
        });
        //3.选取 属性title值不等于'test'的div元素(*没有属性title的也将被选中)
        $("#btn3").click(function() {
           $("div[title!='test']").css("background", "#bbffaa");
        });
        //4.选取 属性title值 以'te'开始 的div元素
        $("#btn4").click(function() {
           $("div[title^='te']").css("background", "#bbffaa");
        });
        //5.选取 属性title值 以'est'结束 的div元素
        $("#btn5").click(function() {
           $("div[title$='est']").css("background", "#bbffaa");
        });
        //6.选取 属性title值 含有'es'的div元素
        $("#btn6").click(function() {
           $("div[title*='es']").css("background", "#bbffaa");
        });
        
        //7.首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素
        $("#btn7").click(function() {
           $("div[id][title*='es']").css("background", "#bbffaa");
        });
        //8.选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素
        $("#btn8").click(function() {
           $("div[title][title!='test']").css("background", "#bbffaa");
        });
     });
  </script>
  </head>
  <body>
     <input type="button" value="选取含有 属性title 的div元素." id="btn1" style="display: none;"/>
     <input type="button" value="选取 属性title值等于'test'的div元素." id="btn2" />
     <input type="button"
        value="选取 属性title值不等于'test'的div元素(没有属性title的也将被选中)." id="btn3" />
     <input type="button" value="选取 属性title值 以'te'开始 的div元素." id="btn4" />
     <input type="button" value="选取 属性title值 以'est'结束 的div元素." id="btn5" />
     <input type="button" value="选取 属性title值 含有'es'的div元素." id="btn6" />
     <input type="button"
        value="组合属性选择器,首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素."
        id="btn7" />
     <input type="button"
        value="选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素." id="btn8" />
  
     <br>
     <br>
     <div class="one" id="one">
        id 为 one,class 为 one 的div
        <div class="mini">class为mini</div>
     </div>
     <div class="one" id="two" title="test">
        id为two,class为one,title为test的div
        <div class="mini" title="other">class为mini,title为other</div>
        <div class="mini" title="test">class为mini,title为test</div>
     </div>
     <div class="one">
        <div class="mini">class为mini</div>
        <div class="mini">class为mini</div>
        <div class="mini">class为mini</div>
        <div class="mini"></div>
     </div>
     <div class="one">
        <div class="mini">class为mini</div>
        <div class="mini">class为mini</div>
        <div class="mini">class为mini</div>
        <div class="mini" title="tesst">class为mini,title为tesst</div>
     </div>
     <div style="display: none;" class="none">style的display为"none"的div</div>
     <div class="hide">class为"hide"的div</div>
     <div>
        包含input的type为"hidden"的div<input type="hidden" value="123456789"
           size="8">
     </div>
     <div id="mover">正在执行动画的div元素.</div>
  </body>
  </html> 
  ```

* 表单过滤器

  | 表单过滤器 | 说明                                                  |
  | ---------- | ----------------------------------------------------- |
  | :input     | 匹配所有input\textarea\select\button元素              |
  | :text      | 匹配所有 文本输入框                                   |
  | :password  | 匹配所有 密码输入框                                   |
  | :radio     | 匹配所有 单选框                                       |
  | :checkbox  | 匹配所有 复选框                                       |
  | :submit    | 匹配所有 提交按钮                                     |
  | :image     | 匹配所有 img标签                                      |
  | :reset     | 匹配所有 重置按钮                                     |
  | :button    | 匹配所有 input type=button 、<button\> 按钮           |
  | :file      | 匹配所有 input type=file 文件上传                     |
  | :hidden    | 匹配所有 不可见元素 display:none 或 input type=hidden |

  表单对象属性过滤器

  | 表单对象属性过滤器 | 说明                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | :enabled           | 匹配所有可用元素                                             |
  | :disabled          | 匹配所有不可用元素                                           |
  | :checked           | 匹配所有选中的单选，复选，和下拉列表中选中的 option 标签对象 |
  | :selected          | 匹配所有选中的option                                         |

  > val() 方法
  > 可以操作表单项的value属性值；它可以设置和获取
  >
  > each() 方法
  > 是jQuery对象提供用来遍历元素的方法；在遍历的function函数中，有一个this对象，这个this对象，就是当前遍历到的dom对象

  ```html
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  <html>
     <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Untitled Document</title>
        <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
        <script type="text/javascript">
           $(function(){
              //1.对表单内 可用input 赋值操作
              $("#btn1").click(function(){
                 $(":text:enabled").val("我是万能的程序员");
              });
              //2.对表单内 不可用input 赋值操作
              $("#btn2").click(function(){
                 $(":text:disabled").val("管你可用不可用，反正我是万能的程序员");
              });
              //3.获取多选框选中的个数  使用size()方法获取选取到的元素集合的元素个数
              $("#btn3").click(function(){
                 alert( $(":checkbox:checked").length );
              });
              //4.获取多选框，每个选中的value值
              $("#btn4").click(function(){
                 // 获取全部选中的复选框标签对象
                 var $checkboies = $(":checkbox:checked");
                 // 老式遍历
                 // for (var i = 0; i < $checkboies.length; i++){
                 //     alert( $checkboies[i].value );
                 // }
                 $checkboies.each(function () {
                    alert( this.value );
                 });
              });
              //5.获取下拉框选中的内容  
              $("#btn5").click(function(){
                 // 获取选中的option标签对象
                 var $options = $("select option:selected");
                 // 遍历，获取option标签中的文本内容
                 $options.each(function () {
                    alert(this.innerHTML);
                 });
              });
           }) 
        </script>
     </head>
     <body>
        <h3>表单对象属性过滤选择器</h3>
         <button id="btn1">对表单内 可用input 赋值操作.</button>
         <button id="btn2">对表单内 不可用input 赋值操作.</button><br /><br />
         <button id="btn3">获取多选框选中的个数.</button>
         <button id="btn4">获取多选框选中的内容.</button><br /><br />
           <button id="btn5">获取下拉框选中的内容.</button><br /><br />
         
        <form id="form1" action="#">         
           可用元素: <input name="add" value="可用文本框1"/><br>
           不可用元素: <input name="email" disabled="disabled" value="不可用文本框"/><br>
           可用元素: <input name="che" value="可用文本框2"/><br>
           不可用元素: <input name="name" disabled="disabled" value="不可用文本框"/><br>
           <br>
           
           多选框: <br>
           <input type="checkbox" name="newsletter" checked="checked" value="test1" />test1
           <input type="checkbox" name="newsletter" value="test2" />test2
           <input type="checkbox" name="newsletter" value="test3" />test3
           <input type="checkbox" name="newsletter" checked="checked" value="test4" />test4
           <input type="checkbox" name="newsletter" value="test5" />test5
           
           <br><br>
           下拉列表1: <br>
           <select name="test" multiple="multiple" style="height: 100px" id="sele1">
              <option>浙江</option>
              <option selected="selected">辽宁</option>
              <option>北京</option>
              <option selected="selected">天津</option>
              <option>广州</option>
              <option>湖北</option>
           </select>
           
           <br><br>
           下拉列表2: <br>
           <select name="test2">
              <option>浙江</option>
              <option>辽宁</option>
              <option selected="selected">北京</option>
              <option>天津</option>
              <option>广州</option>
              <option>湖北</option>
           </select>
        </form>       
     </body>
  </html>
  ```

* 可见性过滤器

  | 可见性过滤器 | 说明                                       |
  | ------------ | ------------------------------------------ |
  | :hidden      | 匹配所有不可见元素，或者type为hidden的元素 |
  | :visible     | 匹配所有可见元素                           |

  

### jQuery元素筛选

| 筛选          | 说明                                                     |
| ------------- | -------------------------------------------------------- |
| eq()          | 和 :eq() 一样                                            |
| first()       | 和 :first一样                                            |
| last()        | 和 :last一样                                             |
| filter(exp)   | 留下匹配的元素                                           |
| is(exp)       | 判断是否匹配给定的选择器，只要一个匹配就返回true         |
| has(exp)      | 和 :has一样                                              |
| not(exp)      | 和 :not一样                                              |
| children(exp) | 和 parent > child一样                                    |
| find(exp)     | 和 ancestor descendant一样                               |
| next()        | 和 prev + next 一样【同级元素】                          |
| nextAll()     | 和 prev ~ siblings一样【同级元素】                       |
| nextUntil()   | 返回当前元素到指定匹配的元素为止的后面元素【同级元素】   |
| parent()      | 返回父元素                                               |
| prev(exp)     | 返回当前元素的上一个兄弟元素【同级元素】                 |
| prevAll()     | 返回当前元素前面所有的兄弟元素【同级元素】               |
| prevUnit(exp) | 返回当前元素到指定匹配的元素为止的前面的元素【同级元素】 |
| siblings(exp) | 返回所有兄弟元素                                         |
| add()         | 把add匹配的选择器的元素添加到当前jQuery对象中            |

> p.myclass
> 表示标签名必须是 p 标签，而且 class 类型还要是 myClass

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>DOM查询</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }        
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(document).ready(function(){
            function anmateIt(){
               $("#mover").slideToggle("slow", anmateIt);
            }
            anmateIt();
             
            //(1)eq()  选择索引值为等于 3 的 div 元素
            $("#btn1").click(function(){
               $("div").eq(3).css("background-color","#bfa");
            });
            //(2)first()选择第一个 div 元素
             $("#btn2").click(function(){
                //first()   选取第一个元素
               $("div").first().css("background-color","#bfa");
            });
            //(3)last()选择最后一个 div 元素
            $("#btn3").click(function(){
               //last()  选取最后一个元素
               $("div").last().css("background-color","#bfa");
            });
            //(4)filter()在div中选择索引为偶数的
            $("#btn4").click(function(){
               $("div").filter(":even").css("background-color","#bfa");
            });
             //(5)is()判断#one是否为:empty或:parent
            $("#btn5").click(function(){
               alert( $("#one").is(":empty") );
            });
            //(6)has()选择div中包含.mini的
            $("#btn6").click(function(){
               $("div").has(".mini").css("background-color","#bfa");
            });
            //(7)not()选择div中class不为one的
            $("#btn7").click(function(){
               $("div").not('.one').css("background-color","#bfa");
            });
            //(8)children()在body中选择所有class为one的div子元素
            $("#btn8").click(function(){
               $("body").children("div.one").css("background-color","#bfa");
            });
            //(9)find()在body中选择所有class为mini的div元素
            $("#btn9").click(function(){
               $("body").find("div.mini").css("background-color","#bfa");
            });
            //(10)next() #one的下一个div
            $("#btn10").click(function(){
               $("#one").next("div").css("background-color","#bfa");
            });
            //(11)nextAll() #one后面所有的span元素
            $("#btn11").click(function(){
               $("#one").nextAll("span").css("background-color","#bfa");
            });
            //(12)nextUntil() #one和span之间的元素
            $("#btn12").click(function(){
               //
               $("#one").nextUntil("span").css("background-color","#bfa")
            });
            //(13)parent() .mini的父元素
            $("#btn13").click(function(){
               $(".mini").parent().css("background-color","#bfa");
            });
            //(14)prev() #two的上一个div
            $("#btn14").click(function(){
               $("#two").prev("div").css("background-color","#bfa")
            });
            //(15)prevAll() span前面所有的div
            $("#btn15").click(function(){
               $("span").prevAll("div").css("background-color","#bfa")
            });
            //(16)prevUntil() span向前直到#one的元素
            $("#btn16").click(function(){
               $("span").prevUntil("#one").css("background-color","#bfa")
            });
            //(17)siblings() #two的所有兄弟元素
            $("#btn17").click(function(){
               $("#two").siblings().css("background-color","#bfa")
            });
            //(18)add()选择所有的 span 元素和id为two的元素
            $("#btn18").click(function(){
               //$("span,#two,.mini,#one")
               $("span").add("#two").add("#one").css("background-color","#bfa");
            });
         });
      </script>
   </head>
   <body>    
      <input type="button" value="eq()选择索引值为等于 3 的 div 元素" id="btn1" />
      <input type="button" value="first()选择第一个 div 元素" id="btn2" />
      <input type="button" value="last()选择最后一个 div 元素" id="btn3" />
      <input type="button" value="filter()在div中选择索引为偶数的" id="btn4" />
      <input type="button" value="is()判断#one是否为:empty或:parent" id="btn5" />
      <input type="button" value="has()选择div中包含.mini的" id="btn6" />
      <input type="button" value="not()选择div中class不为one的" id="btn7" />
      <input type="button" value="children()在body中选择所有class为one的div子元素" id="btn8" />
      <input type="button" value="find()在body中选择所有class为mini的div后代元素" id="btn9" />
      <input type="button" value="next()#one的下一个div" id="btn10" />
      <input type="button" value="nextAll()#one后面所有的span元素" id="btn11" />
      <input type="button" value="nextUntil()#one和span之间的元素" id="btn12" />
      <input type="button" value="parent().mini的父元素" id="btn13" />
      <input type="button" value="prev()#two的上一个div" id="btn14" />
      <input type="button" value="prevAll()span前面所有的div" id="btn15" />
      <input type="button" value="prevUntil()span向前直到#one的元素" id="btn16" />
      <input type="button" value="siblings()#two的所有兄弟元素" id="btn17" />
      <input type="button" value="add()选择所有的 span 元素和id为two的元素" id="btn18" />

      
      <h3>基本选择器.</h3>
      <br /><br />
      文本框<input type="text" name="account" disabled="disabled" />
      <br><br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other"><b>class为mini,title为other</b></div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <span id="span1">^^span元素 111^^</span>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <span id="span2">^^span元素 222^^</span>
      <div id="mover">正在执行动画的div元素.</div>
   </body>
</html>
```



## 正则表达式

| 字符         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| \            | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个向后引用、或一个八进制转义符。例如，“`n`”匹配字符“`n`”。“`\n`”匹配一个换行符。串行“`\\`”匹配“`\`”而“`\(`”则匹配“`(`”。 |
| ^            | 匹配输入字符串的开始位置。如果设置了RegExp对象的Multiline属性，^也匹配“`\n`”或“`\r`”之后的位置。 |
| $            | 匹配输入字符串的结束位置。如果设置了RegExp对象的Multiline属性，$也匹配“`\n`”或“`\r`”之前的位置。 |
| *            | 匹配前面的子表达式零次或多次。例如，zo*能匹配“`z`”以及“`zoo`”。*等价于{0,}。 |
| +            | 匹配前面的子表达式一次或多次。例如，“`zo+`”能匹配“`zo`”以及“`zoo`”，但不能匹配“`z`”。+等价于{1,}。 |
| ?            | 匹配前面的子表达式零次或一次。例如，“`do(es)?`”可以匹配“`does`”或“`does`”中的“`do`”。?等价于{0,1}。 |
| {*n*}        | *n*是一个非负整数。匹配确定的*n*次。例如，“`o{2}`”不能匹配“`Bob`”中的“`o`”，但是能匹配“`food`”中的两个o。 |
| {*n*,}       | *n*是一个非负整数。至少匹配*n*次。例如，“`o{2,}`”不能匹配“`Bob`”中的“`o`”，但能匹配“`foooood`”中的所有o。“`o{1,}`”等价于“`o+`”。“`o{0,}`”则等价于“`o*`”。 |
| {*n*,*m*}    | *m*和*n*均为非负整数，其中*n*<=*m*。最少匹配*n*次且最多匹配*m*次。例如，“`o{1,3}`”将匹配“`fooooood`”中的前三个o。“`o{0,1}`”等价于“`o?`”。请注意在逗号和两个数之间不能有空格。 |
| ?            | 当该字符紧跟在任何一个其他限制符（*,+,?，{*n*}，{*n*,}，{*n*,*m*}）后面时，匹配模式是非贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串“`oooo`”，“`o+?`”将匹配单个“`o`”，而“`o+`”将匹配所有“`o`”。 |
| .            | 匹配除“`\`*`n`*”之外的任何单个字符。要匹配包括“`\`*`n`*”在内的任何字符，请使用像“`(.|\n)`”的模式。 |
| (pattern)    | 匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“`\(`”或“`\)`”。 |
| (?:pattern)  | 匹配pattern但不获取匹配结果，也就是说这是一个非获取匹配，不进行存储供以后使用。这在使用或字符“`(|)`”来组合一个模式的各个部分是很有用。例如“`industr(?:y|ies)`”就是一个比“`industry|industries`”更简略的表达式。 |
| (?=pattern)  | 正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如，“`Windows(?=95|98|NT|2000)`”能匹配“`Windows2000`”中的“`Windows`”，但不能匹配“`Windows3.1`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?!pattern)  | 正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如“`Windows(?!95|98|NT|2000)`”能匹配“`Windows3.1`”中的“`Windows`”，但不能匹配“`Windows2000`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始 |
| (?<=pattern) | 反向肯定预查，与正向肯定预查类拟，只是方向相反。例如，“`(?<=95|98|NT|2000)Windows`”能匹配“`2000Windows`”中的“`Windows`”，但不能匹配“`3.1Windows`”中的“`Windows`”。 |
| (?<!pattern) | 反向否定预查，与正向否定预查类拟，只是方向相反。例如“`(?<!95|98|NT|2000)Windows`”能匹配“`3.1Windows`”中的“`Windows`”，但不能匹配“`2000Windows`”中的“`Windows`”。 |
| x\|y         | 匹配x或y。例如，“`z|food`”能匹配“`z`”或“`food`”。“`(z|f)ood`”则匹配“`zood`”或“`food`”。 |
| [xyz]        | 字符集合。匹配所包含的任意一个字符。例如，“`[abc]`”可以匹配“`plain`”中的“`a`”。 |
| [^xyz]       | 负值字符集合。匹配未包含的任意字符。例如，“`[^abc]`”可以匹配“`plain`”中的“`p`”。 |
| [a-z]        | 字符范围。匹配指定范围内的任意字符。例如，“`[a-z]`”可以匹配“`a`”到“`z`”范围内的任意小写字母字符。 |
| [^a-z]       | 负值字符范围。匹配任何不在指定范围内的任意字符。例如，“`[^a-z]`”可以匹配任何不在“`a`”到“`z`”范围内的任意字符。 |
| \b           | 匹配一个单词边界，也就是指单词和空格间的位置。例如，“`er\b`”可以匹配“`never`”中的“`er`”，但不能匹配“`verb`”中的“`er`”。 |
| \B           | 匹配非单词边界。“`er\B`”能匹配“`verb`”中的“`er`”，但不能匹配“`never`”中的“`er`”。 |
| \cx          | 匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“`c`”字符。 |
| \d           | 匹配一个数字字符。等价于[0-9]。                              |
| \D           | 匹配一个非数字字符。等价于[^0-9]。                           |
| \f           | 匹配一个换页符。等价于\x0c和\cL。                            |
| \n           | 匹配一个换行符。等价于\x0a和\cJ。                            |
| \r           | 匹配一个回车符。等价于\x0d和\cM。                            |
| \s           | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。 |
| \S           | 匹配任何非空白字符。等价于[^ \f\n\r\t\v]。                   |
| \t           | 匹配一个制表符。等价于\x09和\cI。                            |
| \v           | 匹配一个垂直制表符。等价于\x0b和\cK。                        |
| \w           | 匹配包括下划线的任何单词字符。等价于“`[A-Za-z0-9_]`”。       |
| \W           | 匹配任何非单词字符。等价于“`[^A-Za-z0-9_]`”。                |




## jQuery的属性操作

| 方法   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| html() | 可以设置和获取起始标签和结束标签中的内容。跟dom属性innerHTML一样 |
| text() | 可以设置和获取起始标签和结束标签中的文本。跟dom属性innerText一样 |
| val()  | 可以设置和获取表单项的value属性值。跟dom属性value一样        |
| attr() | 可以设置和获取属性的值，不推荐操作checked、readOnly、selected、disabled等等。还可以操作非标准的属性，比如自定义属性。 |
| prop() | 可以设置和获取属性的值，只建议操作checked、readOnly、selected、disabled等等。 |

> html([val|fn])	      a.html()取出a的html值    			 a.html(val)  让a的html值变为val
> text([val|fn]) 		  a.text()取出a的text值    				a.text(val)  让a的文本值变为val
> val([val|fn|arr]) 	 a.val()  取出a的val值（input）    a.val(v)  设置a的value值为v 
>
> attr(name|pro|key,val|fn)  
>  1、a.attr('name')取出a的name值   
>  2、a.attr("name","username")把a的name值设置为username
>
> removeAttr(name) a.removeAttr('class')    移除a的class属性
>
> prop(name|pro|key,val|fn)1.6+ 
>  1、a.prop('id')  取出a的id值   
>  2、a.prop('id',"bj")  设置a的id值为bj
>
> removeProp(name) a.removeProp('class') 移除a的class属性





## DOM的增删改

* 内部插入

  | 方法        | 说明                                                        |
  | ----------- | ----------------------------------------------------------- |
  | appendTo()  | a.appendTo(b) 把a插入到b子元素末尾，成为最后一个子元素。    |
  | prependTo() | a.prependTo(b) 把a插入到b所有子元素前面，成为第一个子元素。 |

* 外部插入

  | 方法           | 说明                     |
  | -------------- | ------------------------ |
  | insertAfter()  | a.insertAfter(b) 得到ba  |
  | insertBefore() | a.insertBefore(b) 得到ab |

* 替换

  | 方法          | 说明                           |
  | ------------- | ------------------------------ |
  | replaceWith() | a.replaceWith(b) 用b替换掉a    |
  | replaceAll()  | a.replaceAll(b) 用a替换掉所有b |

* 删除

  | 方法     | 说明                        |
  | -------- | --------------------------- |
  | remove() | a.remove() 删除a标签        |
  | empty()  | a.empty() 清空a标签里的内容 |

## CSS样式操作

| 方法          | 说明                       |
| ------------- | -------------------------- |
| addClass()    | 添加样式                   |
| removeClass() | 删除样式                   |
| toggleClass() | 有就删除，没有就添加       |
| offset()      | 获取和设置元素的坐标       |
| css()         | 获取和设置a元素的 样式属性 |

> css(name|pro|[,val|fn])       读写匹配元素的样式属性。 
>                         a.css('color')取出a元素的color
>                         a.css('color',"red")设置a元素的color为red
>
> addClass(class|fn)           	  为元素添加一个class值;<div class="mini big">
> removeClass([class|fn])    	删除元素的class值；传递一个具体的class值，就会删除具体的某个class
>                    	 a.removeClass()：移除所有的class值

~~~html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">	
	div{
		width:100px;
		height:260px;
	}
	div.border{
		border: 2px white solid;
	}
	div.redDiv{
		background-color: red;
	}
	div.blackDiv{
		border: 5px blue solid;
	}	
</style>
<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	$(function(){
		var $divEle = $('div:first');
		
		$('#btn01').click(function(){
			//addClass() - 向被选元素添加一个或多个类
			$divEle.addClass("redDiv blackDiv");
		});
		
		$('#btn02').click(function(){
			//removeClass() - 从被选元素删除一个或多个类 
			$divEle.removeClass()
		});

		
		$('#btn03').click(function(){
			//toggleClass() - 对被选元素进行添加/删除类的切换操作 
			//切换就是如果具有该类那么删除，如果没有那么添加上
			$divEle.toggleClass("redDiv");
		});
		
		$('#btn04').click(function(){
			//offset() - 返回第一个匹配元素相对于文档的位置。
			var os = $divEle.offset();
			//注意通过offset获取到的是一个对象，这个对象有两个属性top表示顶边距，left表示左边距
			alert("顶边距："+os.top+" 左边距："+os.left);
			
			//调用offset设置元素位置时，也需要传递一个js对象，对象有两个属性top和left
			//offset({ top: 10, left: 30 });
			 $divEle.offset({
				 top:50,
				 left:60
			 }); 
		});
	})
</script>
</head>
<body>
	<table align="center">
		<tr>
			<td>
				<div class="border">
				</div>
			</td>
			
			<td>
				<div class="btn">
					<input type="button" value="addClass()" id="btn01"/>
					<input type="button" value="removeClass()" id="btn02"/>
					<input type="button" value="toggleClass()" id="btn03"/>
					<input type="button" value="offset()" id="btn04"/>
				</div>
			</td>
		</tr>
	</table>	
	<br /> <br />
	<br /> <br />	
</body>
</html>
~~~



## jQuery动画

* 基本动画

  | 方法     | 说明                     |
  | -------- | ------------------------ |
  | show()   | 将隐藏的元素显示         |
  | hide()   | 将可见的元素隐藏         |
  | toggle() | 可见就隐藏，不可见就显示 |

  > 添加参数：
  >
  > 1. 第一个参数是动画 执行的时长，以毫秒为单位
  > 2. 第二个参数是动画的回调函数（动画完成后自动调用的函数）

* 淡入淡出动画

  | 方法         | 说明                                                         |
  | ------------ | ------------------------------------------------------------ |
  | fadeln()     | 淡入（慢慢可见）                                             |
  | fadeOut()    | 淡出（慢慢消失）                                             |
  | fadeTo()     | 在指定时长内慢慢的将透明度修改到指定的值。0 透明，0.5 半透明，1 不透明 |
  | fadeToggle() | 淡入/淡出 切换                                               |

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<link href="css/style.css" type="text/css" rel="stylesheet" />
		<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
	
<script type="text/javascript">
		$(function(){
			//显示   show()
			$("#btn1").click(function(){
				$("#div1").show(1000);
			});		
			//隐藏  hide()
			$("#btn2").click(function(){
				$("#div1").hide(1000);
			});	
			//切换   toggle()
			$("#btn3").click(function(){
				$("#div1").toggle(1000);
			});	
			
			//淡入   fadeIn()
			$("#btn4").click(function(){
				$("#div1").fadeIn(500);
			});	
			//淡出  fadeOut()
			$("#btn5").click(function(){
				$("#div1").fadeOut(500);
			});	
			
			//淡化到  fadeTo()
			$("#btn6").click(function(){
				$("#div1").fadeTo("slow",Math.random());
			});	
			//淡化切换  fadeToggle()
			$("#btn7").click(function(){
				$("#div1").fadeToggle("slow","linear");
			});	
		})
</script>	
	</head>
	<body>
		<table style="float: left;">
			<tr>
				<td><button id="btn1">显示show()</button></td>
			</tr>
			<tr>
				<td><button id="btn2">隐藏hide()</button></td>
			</tr>
			<tr>
				<td><button id="btn3">显示/隐藏切换 toggle()</button></td>
			</tr>
			<tr>
				<td><button id="btn4">淡入fadeIn()</button></td>
			</tr>
			<tr>
				<td><button id="btn5">淡出fadeOut()</button></td>
			</tr>
			<tr>
				<td><button id="btn6">淡化到fadeTo()</button></td>
			</tr>
			<tr>
				<td><button id="btn7">淡化切换fadeToggle()</button></td>
			</tr>
		</table>		
		<div id="div1" style="float:left;border: 1px solid;background-color: blue;width: 300px;height: 200px;">
			jquery动画定义了很多种动画效果，可以很方便的使用这些动画效果
		</div>
	</body>
</html>
~~~



## jQuery事件操作

### $(function(){}); 和 window.onload = function(){} 区别

* 触发时间区别
  \> jQuery的页面加载完成之后是浏览器的内核解析完页面的标签创建好DOM对象之后就会马上执行。
  \> 原生js的页面加载完成之后，除了要等浏览器内核解析完标签创建好DOM对象，还要等标签显示时需要的内容加载完成。
* 触发顺序区别
  \> jQuery 页面加载完成之后先执行
  \> 原生js 的页面加载完成之后
* 执行次数
  \> 原生js的页面加载完成之后，只会执行最后一次的赋值函数
  \> jQuery 的页面加载完成之后是全部把注册的function函数，依次顺序全部执行

~~~html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript"></script>
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
 	window.onload = function(){
		//会在整个页面加载完毕之后调用
		alert("abc")
	}

	$(function(){
		alert("abc")
        //会在当前文档加载完毕之后调用
	})
	
	$(function(){
		alert("edf")//会在当前文档加载完毕之后调用
	})
	
 	$(function(){
		alert("我是jQuery核心函数");
	});
	window.onload = function(){
		alert("window.onload出来了");
	}; 
</script>
</head>
<body>
	<button>我是按钮</button>
	
	<iframe src="http://www.baidu.com"></iframe>
	
</body>
</html>
~~~



### jQuery事件处理方法

| 方法        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| click()     | 它可以绑定单击事件，以及触发单击事件                         |
| mouseover() | 鼠标移入事件                                                 |
| mouseout()  | 鼠标移出事件                                                 |
| bind()      | 可以给元素一次性绑定一个或多个事件                           |
| one()       | 使用上跟bind()一样，但是one方法绑定的事件只会响应一次        |
| unbind()    | 跟bind方法相反的操作，解除事件的绑定                         |
| live()      | 也是用来绑定事件，它可以用来绑定选择器匹配的所有元素的事件。这个元素是后面动态创建出来的也有效 |

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<link href="css/style.css" type="text/css" rel="stylesheet" />
		<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
		
			$(function(){
				//*1.通常绑定事件的方式
				//给元素绑定事件  
				//jquery对象.事件方法(回调函数(){ 触发事件执行的代码 }).事件方法(回调函数(){ 触发事件执行的代码 }).事件方法(回调函数(){ 触发事件执行的代码 })
				//绑定事件可以链式操作
				$(".head").click(function(){
					$(".content").toggle();
				}).mouseover(function(){
					$(".content").toggle();
				}); 
				
				//*2.jQuery提供的绑定方式：bind(type,[data],fn)函数把元素和事件绑定起来
				//type表示要绑定的事件   [data]表示传入的数据   fn表示事件的处理方法
				//bind(事件字符串,回调函数),后来添加的元素不会绑定事件
				//使用bind()绑定多个事件   type可以接受多个事件类型，使用空格分割多个事件
				 $(".head").bind("click mouseover",function(){
					$(".content").toggle();
				}); 
			
				
				//3.one()只绑定一次,绑定的事件只会发生一次one(type,[data],fn)函数把元素和事件绑定起来

			 	$(".head").one("click mouseover",function(){
					$(".content").toggle();
				}); 

				//4.live方法会为现在及以后添加的元素都绑定上相应的事件
				$(".head").live("click",function(){
					$(".content").toggle();
				});
				$("#panel").before("<h5 class='head'>什么是jQuery?</h5>");
			});
		</script>
	</head>
	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>
</html>
~~~

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<link rel="stylesheet" type="text/css" href="style/css.css" />
		<script type="text/javascript" src="jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(function(){
				//给li绑定两种事件：单击和鼠标移入
				$("li").bind("click mouseenter" , function(){
					alert(this.innerHTML);
				});
				//点击第一个button，将#bj上的mouseenter事件移除
				//unbind()可以移除指定的事件，只需要传一个事件名作为参数
				//unbind(type,[data|fn]])
				//type事件类型  当传入type的时候会解除type事件
				//如果没有传入type值，会移除所有事件
				$("button:eq(0)").click(function(){
					$("li").unbind("click mouseenter");
				});
			});
		</script>
	</head>
	<body>
		<div id="total">
			<div class="inner">
				<p>
					你喜欢哪个城市?
				</p>
				<ul id="city">
					<li id="bj">北京</li>
					<li>上海</li>
					<li>东京</li>
					<li>首尔</li>
				</ul>
				<br>
				<br>
				<p>
					你喜欢哪款单机游戏?
				</p>
				<ul id="game">
					<li id="rl">红警</li>
					<li>实况</li>
					<li>极品飞车</li>
					<li>魔兽</li>
				</ul>
			</div>
			<button>移除#bj的mouseenter事件</button>
			<button>移除#rl所有事件</button>
		</div>
	</body>
</html>
~~~



### 事件的冒泡

* 定义：父子元素同时监听同一个事件。当触发子元素的事件的时候，同一个事件也被传递到了父元素的事件里去响应
* 阻止事件冒泡：在子元素事件函数体内，return false；可以阻止事件的冒泡传递。

~~~html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			body{
				font-size: 13px;
				line-height: 130%;
				padding: 60px;
			}
			#content{
				width: 220px;
				border: 1px solid #0050D0;
				background: #96E555;
			}
			span{
				width: 200px;
				margin: 10px;
				background: #666666;
				cursor: pointer;
				color: white;
				display: block;
			}
			p{
				width: 200px;
				background: #888;
				color: white;
				height: 16px;
			}
		</style>
		<script type="text/javascript" src="jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(function(){
				//给span绑定一个单击响应函数
				$("span").click(function(){
					alert("我是span的单击响应函数");
					return false;
				});
				
				//给id为content的div绑定一个单击响应函数
				$("#content").click(function(){
					alert("我是div的单击响应函数");
					return false;
				});
				
				//取消默认行为
				$("a").click(function(){
					return false;
				}) 
				
			})
		</script>
	</head>
	<body>
		<div id="content">
			外层div元素
			<span>内层span元素</span>
			外层div元素
		</div>
		<div id="msg"></div>	
		<br><br>
		<a href="http://www.hao123.com" onclick="return false;">WWW.HAO123.COM</a>	
	</body>
</html>
~~~



### JavaScript事件对象 **event**

> 获取JavaScript事件对象
>
> 在给元素绑定事件的时候，在事件的 function(event) 参数列表中添加一个参数，这个参数名习惯取名为event，这个event就是javascript 传递参事件处理函数的事件对象。

~~~html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">

	#areaDiv {
		border: 1px solid black;
		width: 300px;
		height: 50px;
		margin-bottom: 10px;
	}
	
	#showMsg {
		border: 1px solid black;
		width: 300px;
		height: 20px;
	}

</style>
<script type="text/javascript" src="jquery-1.7.2.js"></script>
<script type="text/javascript">
	$(function(){
		$("#areaDiv").mousemove(function(event){
			$("#showMsg2").text("x="+event.screenX+" y="+event.screenY)	
		})
	})

	window.onload = function(){
		//使用js获取事件对象
		var areaDiv = document.getElementById("areaDiv");
		var showMsg = document.getElementById("showMsg");
		//同样是将event传入回调函数
		areaDiv.onmousemove = function(event){
			var x = event.pageX;//当前页面的坐标
			var y = event.pageY;//Y
			showMsg.innerHTML = "x="+x+" y="+y;
		};
	}
</script>
</head>
<body>
	<div id="areaDiv"></div>
	<div id="showMsg"></div>
	<div id="showMsg2"></div>
</body>
</html>
~~~

