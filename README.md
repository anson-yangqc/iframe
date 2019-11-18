## iframe
* iframe项目的问题：
* (同域)父子页面的通信应该使用回调来通信；
* (同域)父子页面的dom元素和方法可以相互操作/调用；
* （不同域）子页无法找到父页的元素和方法（需要映射url使用同域；做到子页面使用父页面的域名也能访问到子页面）；这样就可以做到父子页面相互调用元素和方法
* （不同域）possmessage(H5);只能传递字符串；父子页面通信（告诉 ---> 执行的方式）；
* （不同域）websocket通信
* 同个项目里嵌套同个项目的iframe页面，iframe子页面会重新加载整个项目的资源（包括公共js/CSS资源），
* iframe子页面显示过程会出现空白（就是打开页面看到文字前的一段空白）
* 如果打开的iframe子页面在弹窗里，那么弹窗的关闭按钮就有些卡钝，无法随时点击随时关闭
```javascript
<div id="color">mainHTML</div>
	<button id='cd1'>传递1</button>
	<button id='cd2'>传递2</button>
	<iframe id="child" src='http://www.gzgaocheng.com/b.html' width="500" height='500'></iframe>
	<script>
		window.onload=function(){
			document.querySelector('#cd1').onclick = function(){
	            window.frames[0].postMessage({'color':'red','changeTxt':'changeTxt'},'http://www.gzgaocheng.com');
			}
			document.querySelector('#cd2').onclick = function(){
	            window.frames[0].postMessage({'tall':190},'http://www.gzgaocheng.com');
			}
        }
	</script>
```

```javascript
<a href="b1.html">b1.html</a>
	<div id='b'>bbb<div>
	<script type="text/javascript">
		window.addEventListener('message',function(MessageEvent){
			$('#b').html(MessageEvent.data.color)		//接收主页的消息
			eval(MessageEvent.data.changeTxt)();
			console.log(MessageEvent.data.color.tall)
            //if(e.source!=window.parent) return;
            //var color=container.style.backgroundColor;
            //window.parent.postMessage(color,'*');
        },false);
		const changeTxt = ()=>{
			alert('changeTxt')
		}
	</script>
```


```javascript
<div>b1.html<div>
	<script type="text/javascript">
		window.addEventListener('message',function(MessageEvent){
			console.log(MessageEvent.data.tall)
        },false);
		
	</script>
```
