# sky
<!DOCTYPE html>
<html>
	<head>
		<meta content="text/html; charest=utf-8">
		<style>
			#canvas{
				margin:25px auto;
				display:block;
				border:1px solid #ccc;
			}
			#mySky{
				width:130px;
				height:80px;
				background:rgba(51,225,255,0.7);
				
				-moz-border-radius: 15px;
				-webkit-border-radius: 15px;
				border-radius:15px;
				
				position:absolute;
				left:263px;
				top:25px;
			}
			button{
				background:#ff3;
				border:none;
				
				-moz-border-radius: 5px;
				-webkit-border-radius: 5px;
				border-radius:5px;
				
				position:relative;
				left:19px;
				top:5px;
				cursor:pointer;
			}
			p{
				width:68px;
				margin:5px auto;
				margin-top:10px;
				color:#fff;
				font-weight:bold;
			}
			span{
				color:#fff;
				font-size:0.5em;
				padding:0 20px;
			}
		</style>
		<script>
			window.onload=function(){
				var blink=document.getElementById("blink");
				var stop=document.getElementById("stop");
				var start=false;
				
				var canvas=document.getElementById("canvas");
				var context=canvas.getContext("2d");
				
				canvas.width=800;
				canvas.height=400;
				
				//天空
				drawSky(canvas.width,canvas.height);
				//绿地
				drawLand("green");
				//月亮
				drawMoon(650,70,50,30);
				//星星
				drawStars();
				//文字
				write();
				
				blink.onclick=function(){
					timer=setInterval(function(){
						context.clearRect(0,0,800,250);
						drawSky(canvas.width,250);
						drawMoon(650,70,50,30);
						drawStars();
					},500);
				};
				
				stop.onclick=function(){
					clearInterval(timer);
				}
				
				document.onkeyup=function(e){
					if(e.keyCode==32&&!start){
						blink.onclick();
						start=true;
					}else if(e.keyCode==32&&start){
						stop.onclick();
						start=false;
					}
				}
				
				function write(){
					context.font="bold 20px Verdana";
					context.textAlign="right";
					context.textBaseline="bottom";
					context.fillStyle="rgba(255,255,255,0.5)";
					context.fillText("DYY",800,400);
				}
				
				function drawStars(){
					for(var i=1;i<200;i++){
						var x=parseInt(Math.random()*800);
						var y=parseInt(Math.random()*200);
						var scale=parseInt(Math.random()*5);
						var rot=parseInt(Math.random()*180);

						drawStar(x,y,scale,rot);
					}
				}
				
				function drawSky(width,height){
					context.beginPath();
					var grd=context.createLinearGradient(0,0,0,400);
					grd.addColorStop(0,"black");
					grd.addColorStop(1,"blue");
					context.fillStyle=grd;
					context.fillRect(0,0,width,height);
					context.closePath();
				}
				
				function drawMoon(x,y,r,a,color){
					color=color||"yellow";
					context.beginPath();
					context.save();
					
					context.translate(x,y);
					context.scale(r,r);
					context.rotate(a*Math.PI/180);
					
					context.arc(0,0,1,1.5*Math.PI,0.5*Math.PI);
					context.quadraticCurveTo(1.1,0,0,-1);
					
					context.fillStyle=color;
					context.fill();
					
					context.restore();
					context.closePath();
				}
				
				function drawLand(color){	
					context.beginPath();
					context.moveTo(0,350);
					context.bezierCurveTo(100,300,700,400,800,350);
					context.lineTo(800,400);
					context.lineTo(0,400);					

					context.fillStyle=color;
					context.fill();
					context.closePath();
				}
				
				function drawStar(x,y,r,a,color){
					color=color||"yellow";
					context.beginPath();
					context.save();
					
					context.translate(x,y);
					context.scale(r,r);
					context.rotate(a*Math.PI/180);
					
					for(var i=0;i<5;i++){
						context.lineTo(Math.sin((36+72*i)*Math.PI/180),-Math.cos((36+72*i)*Math.PI/180));
						context.lineTo(2*Math.sin((72+72*i)*Math.PI/180),-2*Math.cos((72+72*i)*Math.PI/180));
					}
					 
					context.fillStyle=color;
					context.fill();
					context.restore();
					context.closePath();
				}
			};
		</script>
	</head>
	<body>
		<canvas id="canvas">此浏览器不支持canvas，请更换浏览器！</canvas>
		<div id="mySky">
			<p>穹顶之下</p>
			<span>点击空格试一试~</span>
			<button id="blink">blink</button>
			<button id="stop">stop</button>
		</div>

	</body>
</html>
