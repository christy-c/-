html代码

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>滚动透明轮播</title>
<link href="css/common.css" rel="stylesheet" type="text/css">
<link href="css/carrousel.css" rel="stylesheet" type="text/css">
</head>
<script src="js/jquery-1.7.2.min.js"></script>
<script src="js/carrousel.js"></script>
<body>
	<div class="slider">
    	<ul class="slider-main">
        	<li class="slider-panel">
            	<a href="#" target="_blank"><img  alt="关注脚本之家" title="关注脚本之家"src="img/banner1.jpg"></a>
            </li>
            <li class="slider-panel">
            	<a href="#" target="_blank"><img  alt="关注脚本之家" title="关注脚本之家"src="img/banner2.jpg"></a>
            </li>
            <li class="slider-panel">
            	<a href="#" target="_blank"><img  alt="关注脚本之家" title="关注脚本之家"src="img/banner3.jpg"></a>
            </li>
            <li class="slider-panel">
            	<a href="#" target="_blank"><img  alt="关注脚本之家" title="关注脚本之家"src="img/banner4.jpg"></a>
            </li>
        </ul>
        <div class="slider-extra">
        	<ul class="slider-nav">
            	<li class="slider-item">1</li>
                <li class="slider-item">2</li>
                <li class="slider-item">3</li>
                <li class="slider-item">4</li>
            </ul>
        </div>
        <div class="slider-page">
        	<a class="slider-pre" href="javascript:;;"> < </a> 
            <a class="slider-next" href="javascript:;;"> > </a> 
        </div>
    </div>
</body>
</html>




css代码

 body{margin:0;padding:0; font-size:14px; font-family:"SourceHanSansSC-Light","Microsoft Yahei",微软雅黑,"Helvetica Neue",Arial,sans-serif; width:100%; background:#f6f6f6; color:#333;}
html,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,form,fieldset,input,textarea,blockquote,p,img{padding:0; margin:0;}
img{vertical-align:top; border:none;}
button,select,textarea{outline:none}
figure,form,fieldset{border:0;margin:0;padding:0}
textarea{resize:none}
ul,li{list-style-type:none; cursor:pointer;}
textarea:focus,input:focus{outline:none;}
table{border-collapse:collapse; border-spacing:0; empty-cells:show; }
a{color:#55585c;text-decoration:none;}
a:hover{color:#005aa8;text-decoration:none; cursor:pointer;}
b,strong{font-weight:normal;}
cite,em,i{font-style:normal;}
.c:after {content:"\200B"; display:block; height:0; clear:both;}
.c{*zoom:1;}
.fl{float:left;}
.fr{float:right;}

.bold{font-weight:bold;}
.w1000{margin:0 auto; width:1000px;}

.index-title{font-size:18px; font-weight:normal;}


.slider, .slider-panel img, .slider-extra{width:650px; height:413px;}
.slider{ text-align:center; margin:30px auto; position:relative;}
.slider-panel, .slider-nav, .slider-pre, .slider-next{ position:absolute; z-index:8;}
.slider-panel{ position:absolute;}
.slider-panel img{border:none;}
.slider-extra{position:relative;}

.slider-nav{margin-left:-51px; position:absolute; left:50%; bottom:4px; }
.slider-nav li{
	background:#3e3e3e;
	border-radius:50%; 
	display:inline-block; 
	margin:0 2px; 
	cursor:pointer;
	height:18px; 
	width:18px; 
	line-height:18px; 
	text-align:center; 
	overflow:hidden; 
	color:#FFF;
}
.slider-nav .slider-item-selected{ background:blue;}

.slider-page a{
	background: rgba(0, 0, 0, 0.2); 
	filter: progid:DXImageTransform.Microsoft.gradient(startColorstr=#33000000,endColorstr=#33000000); 
	color:#FFF;
	text-align:center;
	display:block;
	font-family: "simsun";
	font-size:22px;
	width:28px;
	height:62px;
	line-height:62px;
	margin-top:-31px;
	position:absolute;
	top:50%;
}
.slider-page a:hover{
	background: rgba(0, 0, 0, 0.4); 
	filter: progid:DXImageTransform.Microsoft.gradient(startColorstr=#66000000,endColorstr=#66000000); 	
}
.slider-next{
	left: 100%; 
	margin-left:-28px; 	
}


js代码

// JavaScript Document
function css(obj,attr,value){
	if(arguments.length == 2){
		if(attr == 'opacity'){
			if(obj.currentStyle){
				return parseInt(obj.currentStyle[attr]*100);
			}else{
				return parseInt(getComputedStyle(obj,false)[attr]*100);
			}
		}else{
			if(obj.currentStyle){
				return parseInt(obj.currentStyle[attr]);
			}else{
				return parseInt(getComputedStyle(obj,false)[attr]);
			}
		}
	}else{
		if(attr == 'opacity'){
			obj.style.opacity = value/100;
			obj.style.filter = "alpha(opacity:"+value+")"; 
		}else{
			obj.style[attr]=value;
		}
		
	}	
}

function roun(obj,attr,iTarget,model,endFunction){//对象，参数，目标，缓冲，结束后执行下一个
	var speed=5;
	function runOne(){
		if(model==0){//匀速运动
			if(iTarget>css(obj,attr)){
				speed=2;	
			}else if(iTarget<css(obj,attr)){
				speed=-2;	
			}else{
				speed=0;	
			}	
		}else{//缓冲运动
			speed=(iTarget - css(obj,attr))/10;
			if(speed > 0){
				speed=Math.ceil(speed);	//向上取整
			}else if(speed < 0){
				speed=Math.floor(speed);//向下取整
			}
		}
		
		//obj.style[attr]=css(obj,attr)+speed;
		css(obj,attr,css(obj,attr)+speed);
		if(iTarget == css || Math.abs(iTarget - css(obj,attr))<Math.abs(speed)){
			//obj.style[attr]=iTarget;
			css(obj,attr,iTarget);
			clearInterval(obj.timer);
			if(endFunction){
				endFunction();
			}
		}	
	}
	
	clearInterval(obj.timer);
	obj.timer=setInterval(runOne,30);
}


$(document).ready(function() {
 var length,
	currentIndex=0,
	interval,//定时器
	hasStarted=false,//是否已经开始轮
	t=2000;//轮播时间间隔
	
	length = $(".slider-panel").length;//长度
	//将除了第一张图片隐藏
	$(".slider-panel:not(:first)").hide();
	//将第一个slider-item设为激活状态
	$(".slider-item:first").addClass('slider-item-selected'); 
	//隐藏向前、向后翻按钮
	$(".slider-page").hide();
	
	//指向显示向前、向后翻按钮
	$(".slider-panel, .slider-pre, .slider-next").hover(function(){
		stop();
		$('.slider-page').show();
	},function(){
		$(".slider-page").hide();
		start();
	});
	
	//指向小圆点
	$(".slider-item").hover(function(e){
		 stop();
		 var preIndex = $(".slider-item").filter(".slider-item-selected").index();
		 currentIndex = $(this).index();
		 play(preIndex,currentIndex);
	},function(){
		start();	
	});
	
	//点击左翻
	$(".slider-pre").unbind('click');
	$(".slider-pre").click(function(){//左
		pre();
	});
	
	//点击右翻
	$(".slider-next").unbind('click');
	$(".slider-next").click(function(){//右
		next();
	});
	
	/*----向前翻页-----*/
	function pre(){
		var preIndex 	 = currentIndex;
			currentIndex = (--currentIndex+length)%length;
			play(preIndex,currentIndex);
	}
	
	
	/*----向后翻页-----*/
	function next(){
		var preIndex 	 = currentIndex;
			currentIndex = ++currentIndex%length;
			play(preIndex,currentIndex);
	}
	
	/*-----从preIndex页翻到currentIndex页 
			preIndex 整数，翻页的起始页 
			currentIndex 整数，翻到的那页---*/ 
	function play(preIndex,currentIndex){
		$(".slider-panel").eq(preIndex).fadeOut(1000).parent().children().eq(currentIndex).fadeIn(2000);
		$(".slider-item").removeClass('slider-item-selected');
		$(".slider-item").eq(currentIndex).addClass('slider-item-selected');
	}
	
	
	/*-----开始轮播---------*/
	function start(){
		if(!hasStarted){
			hasStarted = true;
			interval = setInterval(next,t);	
		}	
	}
	
	/*-----停止轮播---------*/
	function stop(){
		clearInterval(interval);
		hasStarted = false;	
	}
	start();
});
