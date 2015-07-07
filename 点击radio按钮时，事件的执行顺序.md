##点击radio按钮时，事件的执行顺序问题

`<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title></title>
	<link rel="stylesheet" href="">
</head>
<body>
	<input id="a" type="radio" name="a" value="a">aaaaaaa
	<input id="b" type="radio" name="a" value="b">bbbbbbb
	<script>
	var btn = document.getElementById('a');
	btn.addEventListener('click', function() {
    	console.log('click---click');
	}, false);
	btn.addEventListener('change', function() {
    	console.log('change---change');
	}, false);
	btn.addEventListener('mouseup', function() {
    	console.log('mounseup---mounseup');
	}, false);
	btn.addEventListener('mousedown', function() {
    	console.log('mounsedown---mounsedown');
	}, false);
	</script>
</body>
</html>`

1. chrome下：
mounsedown---mounsedown
mounseup---mounseup
change---change
click---click

2. firefox下：
mounsedown---mounsedown
mounseup---mounseup
click---click
change---change


**解释：**
根据当前规范（<https://html.spec.whatwg.org/multipage/forms.html#radio-button-state-(type=radio)>），顺序应该是 click -> input -> change。如果click中prevent default，不会触发input和change事件。

Chrome的实现是有bug的（尤其是在click事件prevent default时，change事件不应该发生，但是在Chrome里由于顺序在前而必然发生）。Firefox的实现比较准确（但是也没有触发input）。IE的话，手头暂时没有，未测试。