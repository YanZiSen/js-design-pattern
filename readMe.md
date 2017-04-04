### 对象一方法包含了过程,而闭包则是在过程中包含了数据;对象是数据与过程的集合;所以对象实现的功能也可以用闭包来实现
  <pre>
  var Extend=function(name){
  			var count=0;
  			return {
  				name:name,
  				call:function(){
  					count++;
  					console.log(count);
  					console.log(name);
  				}
  			}
  		};
  		var extend=Extend('bob');
  		extend.call();
  		extend.call();
  		extend.call();
  		var extend1=Extend('Jerry');
  		extend1.call();//注意return了一个写对象,但count<strong>不是</strong>共有的,再次创建时又创建了一个闭包又被篡改了;
  		extend.call();
  </pre>
### 实现异步编程的四种方式
   * setTimeout f2在f1后执行 f1是一个很耗时的任务
   <pre>
     f1(callback){
      setTimeout(function(){
       //f1的执行码
       callback();
      },1000);
     }
   </pre>
   * 事件绑定
   * 发布订阅模式/观察者模式
   * promise

     * 观察者模式实现
     <pre>
      var saleOffice={};
      slaeOffice.list=[];
      saleOffice.listen=function(fn){
        this.list.push(fn);
      }
      saleOffice.trigger=function(){
        for(var i=0;i<this.list.length;i++){
          this.list[i].apply(this,arguments);//没有用this.list[i]()的原因是没有传入参数
        }
      }
      saleOffice.listen(div,function(){alert(this.id)});
      saleOffice.trigger();
     </pre>
   * 命令模式
   <pre>
   <button id='excute'>打开电视机</button>
   <button id='undo'>关闭电视机</button>
   <script>
    var tv={
      open:function(){console.log('打开电视机')},
      close:function(){console.log('关闭电视机')}
    }
    var openTvCommand=function(recieve){
      this.recieve=recieve;
    }
    openTvCommand.prototype.open=function(){
      this.recieve.open();
    }
    openTvCommand.prototype.undo=function(){
      this.recieve.close();
    }
    var setCommand=function(command){
      document.getElementById('excute').onclick=function(){
        command.excute();
      }
      document.getElementById('undo').onclick=function(){
        command.undo();
      }
    }
    setCommand(new openTvCommand(tv));
   </script>
   </pre>
   + 单例模式
   <pre>
   var getSingle=function(fn){
   			var ret;
   			return ret||ret=fn.apply(this,arguments);
   		}
   		var getScript=getSingle(function(){
   			return document.createElement('script');
   		});
   		var script1=getScript();
   		var script2=getScript();
   		alert(script1=script2);//true;
   </pre>
### 闭包的用途与for循环的另一种用法
   <pre>
   var Type={};
   for(var i=0,type;type=['string','array','object'][i++];){
   	(function(type){
   		Type['is'+type]=function(obj){
   			return Object.prototype.toString.call(obj);
   		}
   	})(type)
   }
   </pre>
   * 闭包的用途
   1、将全局变量变为局部变量使用
   2、延长变量的生存周期 img数据上报
   3、抽离业务可变代码 作为参数传入
   <pre>
   var appendDiv=function(){
   			for (var i=0;i<100;i++){
   				var div=document.createElement('div');
   				div.innerHTML=i;
   				document.body.appendChild(div);
   				div.style.dispaly='none';
   			}
   		}
   		// 改变后
   		var appendDiv=function(callback){
   			for(var i=0;i<100;i++){
   				var div=document.createElement('div');
   				div.innerHTML=i;
   				document.body.appendChild(div);
   				if(typeof callback==='function'){
   					callback(div);
   				}
   			}
   		}
   		appendDiv(function(node){node.style.display='none';});
   		/*如同Array.prototype.sort函数一样,它内部封装了排序的功能,然而以何种方式排序是变化的,所以其利用传入回调函数来解决*/
   </pre>
### 原型编程的特性,当对象无法响应一个请求时,会把该请求委托给自己的原型
   * 所有的数据都是对象
   * 要得到一个对象,不是通过实例化类,而是找到一个对象作为原型并克隆它。
   * 对象会记住它的原型。
   * 如果对象无法请求,他会把这个请求委托给他自己的原型
### this的四种指向,运行时绑定 严格模式下作为函数调用时指向undefined
   * 一个对象的方法 函数调用 用new apply,call调用
   * apply与call的区别 call需要参数一个个单个传入 apply自动打散
   <pre>
    document.getElementById('div1').onclick=function(){
      alert(this.id);
      var func=function(){
        alert(this.id);
      }
      func();//此处输出？？？       看是哪种方式调用
    }
    <br/>
    var obj={name='name',sayName=function(){alert(this.name)}};
    func=obj.sayName;
    func();           //this的丢失
    Function.prototype.bind=function(){
      var self=this,
          context=[].shift.call(arguments),
          args=[].slice.call(arguments);
      return function(){
        return self.apply(context,[].concat.call(args,[].slice.call(arguments)));
      }
    }
   </pre>


