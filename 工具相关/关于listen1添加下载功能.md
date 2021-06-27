# 关于listen1添加下载功能

### 效果图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190517181514691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nob3V6aG91OTcwMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190517182024876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nob3V6aG91OTcwMQ==,size_16,color_FFFFFF,t_70)

#### 首先照样更改 loweb.js文件

选中内容为新增的代码(就是在successCallback函数中添加两行代码)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190714012423827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nob3V6aG91OTcwMQ==,size_16,color_FFFFFF,t_70)
下面这段方便复制

```js
button = document.getElementById('download_music')
button.mp3url = sound.url
```

### 在 html引入自己新建的js和css

更改文件是 **listen1.html** ,在 **html** 合适位置插入即可

```html
<link rel="stylesheet" type="text/css" href="css/iconfont.css">
<link rel="stylesheet" type="text/css" href="css/me.css">
<script type="text/javascript" src="js/download2.js"></script>
```

这里我引入了一个**download2.js**的**js**脚本,这个脚本就是解决跨域下载文件不能正确重命名的方法,这个是别人写好的一个组件 [网站](http://danml.com/download.html)
你也可以直接在js目录下新建这个**download2.js**文件
**download2.js文件内容:**

```js
//download.js v3.0, by dandavis; 2008-2014. [CCBY2] see http://danml.com/download.html for tests/usage
// v1 landed a FF+Chrome compat way of downloading strings to local un-named files, upgraded to use a hidden frame and optional mime
// v2 added named files via a[download], msSaveBlob, IE (10+) support, and window.URL support for larger+faster saves than dataURLs
// v3 added dataURL and Blob Input, bind-toggle arity, and legacy dataURL fallback was improved with force-download mime and base64 support

// data can be a string, Blob, File, or dataURL

		 
						 
						 
function download(data, strFileName, strMimeType) {
	
	var self = window, // this script is only for browsers anyway...
		u = "application/octet-stream", // this default mime also triggers iframe downloads
		m = strMimeType || u, 
		x = data,
		D = document,
		a = D.createElement("a"),
		z = function(a){return String(a);},
		
		
		B = self.Blob || self.MozBlob || self.WebKitBlob || z,
		BB = self.MSBlobBuilder || self.WebKitBlobBuilder || self.BlobBuilder,
		fn = strFileName || "download",
		blob, 
		b,
		ua,
		fr;

	//if(typeof B.bind === 'function' ){ B=B.bind(self); }
	
	if(String(this)==="true"){ //reverse arguments, allowing download.bind(true, "text/xml", "export.xml") to act as a callback
		x=[x, m];
		m=x[0];
		x=x[1]; 
	}
	
	
	
	//go ahead and download dataURLs right away
	if(String(x).match(/^data\:[\w+\-]+\/[\w+\-]+[,;]/)){
		return navigator.msSaveBlob ?  // IE10 can't do a[download], only Blobs:
			navigator.msSaveBlob(d2b(x), fn) : 
			saver(x) ; // everyone else can save dataURLs un-processed
	}//end if dataURL passed?
	
	try{
	
		blob = x instanceof B ? 
			x : 
			new B([x], {type: m}) ;
	}catch(y){
		if(BB){
			b = new BB();
			b.append([x]);
			blob = b.getBlob(m); // the blob
		}
		
	}
	
	
	
	function d2b(u) {
		var p= u.split(/[:;,]/),
		t= p[1],
		dec= p[2] == "base64" ? atob : decodeURIComponent,
		bin= dec(p.pop()),
		mx= bin.length,
		i= 0,
		uia= new Uint8Array(mx);

		for(i;i<mx;++i) uia[i]= bin.charCodeAt(i);

		return new B([uia], {type: t});
	 }
	  
	function saver(url, winMode){
		
		
		if ('download' in a) { //html5 A[download] 			
			a.href = url;
			a.setAttribute("download", fn);
			a.innerHTML = "downloading...";
			D.body.appendChild(a);
			setTimeout(function() {
				a.click();
				D.body.removeChild(a);
				if(winMode===true){setTimeout(function(){ self.URL.revokeObjectURL(a.href);}, 250 );}
			}, 66);
			return true;
		}
		
		//do iframe dataURL download (old ch+FF):
		var f = D.createElement("iframe");
		D.body.appendChild(f);
		if(!winMode){ // force a mime that will download:
			url="data:"+url.replace(/^data:([\w\/\-\+]+)/, u);
		}
		 
	
		f.src = url;
		setTimeout(function(){ D.body.removeChild(f); }, 333);
		
	}//end saver 
		

	if (navigator.msSaveBlob) { // IE10+ : (has Blob, but not a[download] or URL)
		return navigator.msSaveBlob(blob, fn);
	} 	
	
	if(self.URL){ // simple fast and modern way using Blob and URL:
		saver(self.URL.createObjectURL(blob), true);
	}else{
		// handle non-Blob()+non-URL browsers:
		if(typeof blob === "string" || blob.constructor===z ){
			try{
				return saver( "data:" +  m   + ";base64,"  +  self.btoa(blob)  ); 
			}catch(y){
				return saver( "data:" +  m   + "," + encodeURIComponent(blob)  ); 
			}
		}
		
		// Blob but not URL:
		fr=new FileReader();
		fr.onload=function(e){
			saver(this.result); 
		};
		fr.readAsDataURL(blob);
	}	
	return true;
} /* end download() */
```

**iconfont.css**是我从阿里巴巴官网下载的图标文件(显示下载图标)
**iconfont.css**文件内容:

```css
@font-face {font-family: "iconfont";
  src: url('iconfont.eot?t=1558084334586'); /* IE9 */
  src: url('iconfont.eot?t=1558084334586#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAJ8AAsAAAAABjQAAAIwAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCCcAo8WQE2AiQDCAsGAAQgBYRtBzIbgQUR1YtVsi8OMrnE7peizckfRUSW2KbNqW0TymQa73sQ1X75PbvzLpu9ACtElYoEAoWoYhFUYjRQ+VSESnn4yO7ra8BNZmubm1XXqAjFkpzn/dYdaTfhQn51tcof1wDweS6nN4EOZNQHym1PGmvymICBpQHuhVEvkhLKuGHsghd4HwJ4MilHmrauIRyKvUwAWc0LU7hCGEVZziGEGvYqck6AM6cmOIu+L78rxIEhsNgNffPNs9R923MKLJGd0YAgAgI6HAAWKAcUZKgx3YNFGGfxx5VcQFEEvu1FItiFHNWOsL/Oro4BIB0qnknpHUAAMABoMh4PpEx6HxgXta7ixJtEO/ny1htfcT9sxHCUbv8Cpc92qMM/s7tmAQQ+PQxvIr7qvwsrAM/qv8imDPOC24FH4EcoB/apobadRjVlo9cQP2mpATweiGB/Eu+mWlLPmhccqef2RpGRweLIJ5UtR0CYSoRw1MJTRuvhMMmsYRGNBko5BBASuIUhjkdYEngilX2LgDT+IkSCWHhGJPnMMIViZd3IaAQ92D9kR3tw3aLj2ivGdzOcV0f8E/nMMZiHqZy74I68xIbzExeRAIGpwwYew9YIDqaCToYkcqzjGJpeNDjqStPIaAQ92D9kR3sIRouufP6K8d0MdzTVZZ/IZ54c5mEaQF4M+6CmR3nm/MRFJEBg6rCBWdgawdE8q6CTIU1IHuvoDAtDDcP2pv4D5awN22rEmW5xs/TdGxmvFAAA') format('woff2'),
  url('iconfont.woff?t=1558084334586') format('woff'),
  url('iconfont.ttf?t=1558084334586') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('iconfont.svg?t=1558084334586#iconfont') format('svg'); /* iOS 4.1- */
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-download:before {
  content: "\e682";
}
```

**me.css**文件内容:

```css
#download_music{
    cursor: pointer;
    color: #7F7F7F;
    margin-right: 3px;
}
#download_music:hover{
    color: black;
}
```

到此在对应文件引入就已经完成了,接下来继续修改html

### 修改 html(添加下载标签)

在**html**代码添加如下部分
(光标所在位置为插入部分,大约在447行左右)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190714005844399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nob3V6aG91OTcwMQ==,size_16,color_FFFFFF,t_70)
下面这段方便复制

```html
<a title="下载此歌曲" class="iconfont icon-download" id="download_music" mp3url=""  ng-click="download_music(currentPlaying.artist,currentPlaying.title)"></a> 
```

### 在 app.js 里面添加 ng-click 监听事件

在 `app.js` 文件的 272 行左右添加
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190714012013272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nob3V6aG91OTcwMQ==,size_16,color_FFFFFF,t_70)

```js
$scope.download_music = (artist_name,song_name) => {
        var mp3url = document.getElementById('download_music').mp3url
        var strs=mp3url.split('.'); //字符分割 
        var houzhui = strs[strs.length-1].substring(0, 3);
        var filename = song_name +" - "+artist_name;
        var x=new XMLHttpRequest();
        x.open("GET", mp3url, true);
        x.responseType = 'blob';
        x.onload=function(e){download(x.response, filename+'.'+houzhui);};
        x.send();
      };
```

### 到此大功告成!





-----

# 给 Listen 1 Chrome 版添加下载功能

简略版

具体要修改的地方有两处 **第一处**在 listen1.html 433行

修改

```html
<div class="title">{{ currentPlaying.title }}</div>
```

为

```html
<div class="title">{{ currentPlaying.title }} <a id="download_music" download="" class="" href="" target="_blank">下载</a></div>
```

这里我增加了 target 新窗口打开，并且把原作者的 download 设置为空，因为安全的原因所以跨域这个问题无解。这样的设置的不足就是无法自动保存歌曲名称，需要手动修改保存的歌曲名称。

**第二处**在 js/loweb.js 173行 修改

```js
        function successCallback() {
          playerSuccessCallback();
          success();
        }
```

为

```js
        function successCallback() {
          button = document.getElementById('download_music')
          button.href = sound.url
          playerSuccessCallback();
          success();
        }

```

这两处修改好就完成了。

安装方法：Chrome 因为安全原因禁止非Chrome商店的链接安装，你需要把.crx 文件下载到本地，然