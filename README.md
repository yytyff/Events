## 事件兼容
```
<!DOCTYPE html>
<html>
<head>
	<title>事件兼容</title>
	<meta charset="utf-8">
</head>
<body>
	<button id="btn">点我</button>
<script>
//IE 浏览器的兼容
	var btn = document.getElementById('btn');
	//alert(btn);
	//btn.addEventListener('click',function(e){
	//	console.log(this.innerText);
	//	console.log(e.target);
	//	alert(2);
	//})

	//IE上绑定事件
	//btn.onclick = function(){
	//	alert(2);
	//}

	var a =1;
	btn.attachEvent('onclick',function(e){
	//	//alert(e.srcElement.innerText);
		var target = getTarget(e);
		alert(this.innerText);  //在IE中this代表Window
		alert(this.a);
		alert(3); //IE上可用，chrome上不可用
	})

	bindEvent(btn,'click', function(e){
		var target = getTarget(e);
		preventDefault(e);
		stopPropagation(e);
		alert(target.innerText);
		alert(this.innerText);
		alert(3);
	})

	function bindEvent(node,type,handler){
		if (node.addEventListener) {
			node.addEventListener(type,handler);
		} else {
			node['e' + type + handler] = handler;
        	node[type + handler] = function() {
            	ode['e' + type + handler](window.event);
        	};
        	node.attachEvent('on' + type, node[type + handler]);
		}
	}

	function removeEvent(node,type,handler){
		if(node.removeEventListener){
			node.removeEventListener(type,handler);
		}else{
			node.detachEvent('on' + type, node[type + handler]);
        	node[type + handler] = null;
		}
	}

	function getTarget(e){
		return e.target || e.srcElement;
	}

	function stopPropagation(e){
		if(e.stopPropagation){
			e.stopPropagation()
		}else{
			e.cancelBubble =true;
		}
	}
	function preventDefault(e){
		if(e.preventDefault){
			e.preventDefault()
		}else{
			e.returnValue = false;
		}
	}

	var node = {
		'eclickfunction(){alert(1)}':function(){alert(this);alert(1)}
		'clickfunction(){alert(1)}':function(){
			node['eclickfunction(){alert(1)}'](window.event);
		}
	}
</script>
</body>
</html>
```
