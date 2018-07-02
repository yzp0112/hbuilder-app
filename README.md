# 混合开发 

### mui窗口对象

>Webview模块管理应用窗口界面，实现多窗口的逻辑控制管理操作。通过plus.webview可获取应用界面管理对象。

# 方法：
#### all：获取所有Webview窗口
```
 plus.webview.all();
 获取应用中已创建的所有Webview窗口，包括所有未显示的Webview窗口。 返回WebviewObject对象在数组中按创建的先后顺序排
 列，即数组中第一个WebviewObject对象用是加载应用的入口页面。
 
 返回值：
<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 获取所有Webview窗口
		var wvs=plus.webview.all();
		for(var i=0;i<wvs.length;i++){
			console.log('webview'+i+': '+wvs[i].getURL());
		}
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```
			
#### close:关闭Webview窗口
```
plus.webview.close( id_wvobj, aniClose, duration, extras );
关闭已经打开的Webview窗口，需先获取窗口对象或窗口id，并可指定关闭窗口的动画及动画持续时间。
--参数：
id_wvobj: ( String | WebviewObject ) 必选 要关闭Webview窗口id或窗口对象
若操作窗口对象已经关闭，则无任何效果。 使用窗口id时，则查找对应id的窗口，如果有多个相同id的窗口则操作最先打开的窗口，若
没有查找到对应id的WebviewObject对象，则无任何效果。

aniClose: ( AnimationTypeClose ) 可选 关闭Webview窗口的动画效果
如果没有指定关闭窗口动画，则使用默认值“auto”，即使用显示时设置的窗口动画相对应的关闭动画。

duration: ( Number ) 可选 关闭Webview窗口动画的持续时间
单位为ms，如果没有设置则使用显示窗口动画时间。

extras: ( WebviewExtraOptions ) 可选 关闭Webview窗口扩展参数
可用于指定Webview窗口动画是否使用图片加速。
<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 关闭自身窗口
	function closeme(){
		var ws=plus.webview.currentWebview();
		plus.webview.close(ws);
	}
</script>
```

#### create:创建新的Webview窗口

```
 plus.webview.create( url, id, styles, extras )
 创建Webview窗口，用于加载新的HTML页面，可通过styles设置Webview窗口的样式，创建完成后需要调用show方法才能将
 Webview窗口显示出来。
 
 参数：
url: ( String ) 可选 新窗口加载的HTML页面地址,新打开Webview窗口要加载的HTML页面地址，可支持本地地址和网络地址。
id: ( String ) 可选 新窗口的标识
窗口标识可用于在其它页面中通过getWebviewById来查找指定的窗口，为了保持窗口标识的唯一性，应该避免使用相同的标识来创建多个Webview窗口。 如果传入无效的字符串则使用url参数作为WebviewObject窗口的id值。

styles: ( WebviewStyles ) 可选 创建Webview窗口的样式（如窗口宽、高、位置等信息）
extras: ( JSON ) 可选 创建Webview窗口的额外扩展参数
值为JSON类型，设置扩展参数后可以直接通过Webview的点（“.”）操作符获取扩展参数属性值，如： var w=plus.webview.create('url.html','id',{},{preload:'preload webview'}); // 可直接通过以下方法获取preload值 console.log(w.preload); // 输出值为“preload webview”

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 创建并显示新窗口
	function create(){
		var w = plus.webview.create('http://m.weibo.cn/u/3196963860');
		w.show(); // 显示窗口
	}
</script>
```

#### currentWebview:获取当前窗口的WebviewObject对象

```
plus.webview.currentWebview()  获取当前页面所属的Webview窗口对象
返回值：WebviewObject : Webview窗口对象
<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		var ws=plus.webview.currentWebview();
		console.log( "当前Webview窗口："+ws.getURL() );
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### getDisplayWebview:获取屏幕所有可视的Webview窗口

```
plus.webview.getDisplayWebview()
仅在屏幕区域显示的Webview窗口，如果Webview窗口显示了但被其它Webview窗口盖住则认为不可视。

返回值：
Array[ WebviewObject ] : 屏幕中可视的Webview窗口对象数组。

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 获取所有可视的Webview窗口
		var wvs=plus.webview.getDisplayWebview();
		for(var i=0;i<wvs.length;i++){
			console.log('Display webview '+i+': '+wvs[i].getURL());
		}
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### getWebviewById:查找指定标识的WebviewObject窗口

```
plus.webview.getWebviewById( id )
在已创建的窗口列表中查找指定标识的Webview窗口并返回。 若没有查找到指定标识的窗口则返回null，若存在多个相同标识的Webview窗口，则返回第一个创建的Webview窗口。 如果要获取应用入口页面所属的Webview窗口，其标识为应用的%APPID%，可通过plus.runtime.appid获取。

参数：
id: ( String ) 必选 要查找的Webview窗口标识

返回值：
WebviewObject : WebviewObject窗口对象

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 查找应用首页窗口对象
		var h=plus.webview.getWebviewById( plus.runtime.appid );
		console.log( "应用首页Webview窗口："+h.getURL() );
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### getLaunchWebview:获取应用首页WebviewObject窗口对象

```
plus.webview.getLaunchWebview()

返回值：
WebviewObject : WebviewObject窗口对象

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 获取应用首页窗口对象
		var h=plus.webview.getLaunchWebview();
		console.log('应用首页Webview窗口：'+h.getURL());
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### getSecondWebview:获取应用第二个首页WebviewObject窗口对象

```
plus.webview.getSecondWebview()
在双首页模式下（在manifest.json的plus->secondwebview节点下配置），应用会自动创建两个首页Webview，通过getLaunchWebview()可获取第一个首页窗口对象，通过getSecondWebview()可获取第二个首页窗口对象。

返回值：
WebviewObject : WebviewObject窗口对象，在非双首页模式下则返回undefined。

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 获取应用第二个首页窗口对象
		var h=plus.webview.getSecondWebview();
		if(h){
			console.log('应用第二个首页Webview窗口：'+h.getURL());
		}else{
			console.log('应用不存在第二个首页Webview窗口');
		}
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### getTopWebview:获取应用显示栈顶的WebviewObject窗口对象

```
plus.webview.getTopWebview()
返回值：
WebviewObject : WebviewObject窗口对象

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){
		// 获取应用首页窗口对象
		var h=plus.webview.getTopWebview();
		console.log('应用显示栈顶的Webview窗口：'+h.getURL());
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
</script>
```

#### hide:隐藏Webview窗口

```
plus.webview.hide( id_wvobj, aniHide, duration, extras )
根据指定的WebviewObject对象或id隐藏Webview窗口，使得窗口不可见。

参数：
id_wvobj: ( String | WebviewObject ) 必选 要隐藏的Webview窗口id或窗口对象
使用窗口对象时，若窗口对象已经隐藏，则无任何效果。 使用窗口id时，则查找对应id的窗口，如果有多个相同id的窗口则操作最先打开的，若没有查找到对应id的WebviewObject对象，则无任何效果。

aniHide: ( AnimationTypeClose ) 可选 隐藏Webview窗口的动画效果
如果没有指定窗口动画，则使用默认动画效果“none”。

duration: ( Number ) 可选 隐藏Webview窗口动画的持续时间
单位为ms，如果没有设置则使用默认窗口动画时间。

extras: ( WebviewExtraOptions ) 可选 隐藏Webview窗口扩展参数
可用于指定Webview窗口动画是否使用图片加速。

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 隐藏自身窗口
	function hideeme(){
		plus.webview.hide(plus.webview.currentWebview());
	}
</script>
```

#### open：创建并打开Webview窗口

```
plus.webview.open( url, id, styles, aniShow, duration, showedCB )
创建并显示Webview窗口，用于加载新的HTML页面，可通过styles设置Webview窗口的样式，创建完成后自动将Webview窗口显示出来。

参数：
url: ( String ) 可选 打开窗口加载的HTML页面地址,新打开Webview窗口要加载的HTML页面地址，可支持本地地址和网络地址。
id: ( String ) 可选 打开窗口的标识
窗口标识可用于在其它页面中通过getWebviewById来查找指定的窗口，为了保持窗口标识的唯一性，应该避免使用相同的标识来创建多个Webview窗口。 如果传入无效的字符串则使用url参数作为WebviewObject窗口的id值。

styles: ( WebviewStyles ) 可选 创建Webview窗口的样式（如窗口宽、高、位置等信息）
aniShow: ( AnimationTypeShow ) 可选 显示Webview窗口的动画效果
如果没有指定窗口动画，则使用默认无动画效果“none”。

duration: ( Number ) 可选 显示Webview窗口动画的持续时间,单位为ms，默认值为200ms（毫秒）。
showedCB: ( SuccessCallback ) 可选 Webview窗口显示完成的回调函数
当指定Webview窗口显示动画执行完毕时触发回调函数，窗口无动画效果（如"none"动画效果）时也会触发此回调。

返回值：
WebviewObject : WebviewObject窗口对象

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 创建并显示新窗口
	function openWebview(){
		var w = plus.webview.open('http://m.weibo.cn/u/3196963860');
	}
</script>
```

#### prefetchURL:预载网络页面

```
plus.webview.prefetchURL(url)
预载网络页面会向服务器发起http/https请求获取html页面内容， 待Webview窗口加载此url页面时会则根据缓存机制优先使用预载
的页面内容(加快页面显示速度)。 注意：预载网络页面仅在运行期生效，为了节省内存仅保留最后5个预载页面数据。

参数：
url: ( String ) 必选 需要预载的页面地址,必须是网络地址（http/https）,本地页面地址无需预载。

<script type="text/javascript">
	var url = 'http://m.weibo.cn/u/3196963860';
	var ws=null,wn=null;
	// H5 plus事件处理
	function plusReady(){
		ws=plus.webview.currentWebview();
		// 预载网络页面
		plus.webview.prefetchURL(url);
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 打开窗口
	function showWebview(){
		// 预创建新窗口（显示在可视区域外）
		wn=plus.webview.create(url, 'test',{render:'always'});
		wn.show('none');
	}
</script>
```

#### prefetchURLs:预载网络页面（多个地址）

```
plus.webview.prefetchURLs(urls)
预载网络页面会向服务器发起http/https请求获取html页面内容， 待Webview窗口加载此url页面时会则根据缓存机制优先使用预载
的页面内容(加快页面显示速度)。 注意：预载网络页面仅在运行期生效，为了节省内存仅保留最后5个预载页面数据。

参数：
urls: ( Array[ String ] ) 必选 需要预载的页面地址数组,数组项必须是网络地址（http/https）,本地页面地址无需预载。

<script type="text/javascript">
	var urls = ['http://m.weibo.cn/u/3196963860',
	'http://m3w.cn/'];
	var ws=null,wn=null;
	// H5 plus事件处理
	function plusReady(){
		ws=plus.webview.currentWebview();
		// 预载网络页面
		plus.webview.prefetchURLs(urls);
	}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 打开窗口
	function showWebview(){
		// 创建并显示新窗口
		wn=plus.webview.create(urls[0], 'test', {render:'always'});
		wn.show('none');
	}
	function showWebview1(){
		// 创建并显示新窗口
		wn=plus.webview.create(urls[1], 'test1', {render:'always'});
		wn.show('none');
	}
</script>
```

#### show:显示Webview窗口

```
plus.webview.show( id_wvobj, aniShow, duration, showedCB, extras )
显示已创建或隐藏的Webview窗口，需先获取窗口对象或窗口id，并可指定显示窗口的动画及动画持续时间。

参数：
id_wvobj: ( String | WebviewObject ) 必选 要显示Webview窗口id或窗口对象
若操作Webview窗口对象显示，则无任何效果。 使用窗口id时，则查找对应id的窗口，如果有多个相同id的窗口则操作最先创建的窗口，若没有查找到对应id的WebviewObject对象，则无任何效果。

aniShow: ( AnimationTypeShow ) 可选 显示Webview窗口的动画效果
如果没有指定窗口动画类型，则使用默认值“auto”，即自动选择上一次显示窗口的动画效果，如果之前没有显示过，则使用“none”动画效果。

duration: ( Number ) 可选 显示Webview窗口动画的持续时间,单位为ms，如果没有设置则使用默认窗口动画时间600ms。
showedCB: ( SuccessCallback ) 可选 Webview窗口显示完成的回调函数
当指定Webview窗口显示动画执行完毕时触发回调函数，窗口无动画效果（如"none"动画效果）时也会触发此回调。

extras: ( WebviewExtraOptions ) 可选 显示Webview窗口扩展参数,可用于指定Webview窗口动画是否使用图片加速。

返回值：
WebviewObject : Webview窗口对象

<script type="text/javascript">
	// H5 plus事件处理
	function plusReady(){}
	if(window.plus){
		plusReady();
	}else{
		document.addEventListener('plusready', plusReady, false);
	}
	// 创建并显示新窗口
	function create(){
		var w = plus.webview.create('http://m.weibo.cn/u/3196963860');
		plus.webview.show(w); // 显示窗口
	}
</script>
```