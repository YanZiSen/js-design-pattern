### ����һ���������˹���,���հ������ڹ����а���������;��������������̵ļ���;���Զ���ʵ�ֵĹ���Ҳ�����ñհ���ʵ��
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
  		extend1.call();//ע��return��һ��д����,��count<strong>����</strong>���е�,�ٴδ���ʱ�ִ�����һ���հ��ֱ��۸���;
  		extend.call();
  </pre>
### ʵ���첽��̵����ַ�ʽ
   * setTimeout f2��f1��ִ�� f1��һ���ܺ�ʱ������
   <pre>
     f1(callback){
      setTimeout(function(){
       //f1��ִ����
       callback();
      },1000);
     }
   </pre>
   * �¼���
   * ��������ģʽ/�۲���ģʽ
   * promise

     * �۲���ģʽʵ��
     <pre>
      var saleOffice={};
      slaeOffice.list=[];
      saleOffice.listen=function(fn){
        this.list.push(fn);
      }
      saleOffice.trigger=function(){
        for(var i=0;i<this.list.length;i++){
          this.list[i].apply(this,arguments);//û����this.list[i]()��ԭ����û�д������
        }
      }
      saleOffice.listen(div,function(){alert(this.id)});
      saleOffice.trigger();
     </pre>
   * ����ģʽ
   <pre>
   <button id='excute'>�򿪵��ӻ�</button>
   <button id='undo'>�رյ��ӻ�</button>
   <script>
    var tv={
      open:function(){console.log('�򿪵��ӻ�')},
      close:function(){console.log('�رյ��ӻ�')}
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
   + ����ģʽ
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
### �հ�����;��forѭ������һ���÷�
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
   * �հ�����;
   1����ȫ�ֱ�����Ϊ�ֲ�����ʹ��
   2���ӳ��������������� img�����ϱ�
   3������ҵ��ɱ���� ��Ϊ��������
   <pre>
   var appendDiv=function(){
   			for (var i=0;i<100;i++){
   				var div=document.createElement('div');
   				div.innerHTML=i;
   				document.body.appendChild(div);
   				div.style.dispaly='none';
   			}
   		}
   		// �ı��
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
   		/*��ͬArray.prototype.sort����һ��,���ڲ���װ������Ĺ���,Ȼ���Ժ��ַ�ʽ�����Ǳ仯��,���������ô���ص����������*/
   </pre>
### ԭ�ͱ�̵�����,�������޷���Ӧһ������ʱ,��Ѹ�����ί�и��Լ���ԭ��
   * ���е����ݶ��Ƕ���
   * Ҫ�õ�һ������,����ͨ��ʵ������,�����ҵ�һ��������Ϊԭ�Ͳ���¡����
   * ������ס����ԭ�͡�
   * ��������޷�����,������������ί�и����Լ���ԭ��
### this������ָ��,����ʱ�� �ϸ�ģʽ����Ϊ��������ʱָ��undefined
   * һ������ķ��� �������� ��new apply,call����
   * apply��call������ call��Ҫ����һ������������ apply�Զ���ɢ
   <pre>
    document.getElementById('div1').onclick=function(){
      alert(this.id);
      var func=function(){
        alert(this.id);
      }
      func();//�˴����������       �������ַ�ʽ����
    }
    <br/>
    var obj={name='name',sayName=function(){alert(this.name)}};
    func=obj.sayName;
    func();           //this�Ķ�ʧ
    Function.prototype.bind=function(){
      var self=this,
          context=[].shift.call(arguments),
          args=[].slice.call(arguments);
      return function(){
        return self.apply(context,[].concat.call(args,[].slice.call(arguments)));
      }
    }
   </pre>


