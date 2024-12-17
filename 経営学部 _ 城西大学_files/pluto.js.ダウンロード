/**
 * pluto.js - アーティスWebサイト制作用Javascriptライブラリ 
 *
 * Copyright (C) Artis Inc. | http://www.artisj.com/
 * Dual licensed under the MIT <http://www.opensource.org/licenses/mit-license.php>
 * and GPL <http://www.opensource.org/licenses/gpl-license.php> licenses.
 * Release: 2009-8-31
 * Last Update: 2010-8-2
 * @version 1.2.5
 * author takuya fujimi, takashi iwata
 */

var pluto = new function(){

    /**
     * プライベートメソッド：外部からはアクセスできません
     */

    // private method用オブジェクト
    var ins = {};

    // イベントストップ
    ins._preventDefault = function(e){
        if(e.preventDefault){
            e.preventDefault();
        }
        else if(window.event){
            window.event.returnValue = false;
        }
    };

    // イベントアタッチ
    ins._addEvent = function(node,evt,func){
        if(node.addEventListener){
            node.addEventListener(evt,func,false);
        } else if(node.attachEvent){
            node.attachEvent("on"+evt,func);
        }
    };

    // イベントデタッチ
    ins._removeEvent = function (node,evt,func){
        if(node.removeEventListener){
            node.removeEventListener(evt,func,false);
        } else if(node.detachEvent){
            node.detachEvent("on"+evt,func);
        }
    };

    // クッキーをセットする
    ins._setCookie = function(key,data,term){
        if(!navigator.cookieEnabled)return;
        var day = new Date();
        day.setTime(day.getTime() + (term * 1000 * 60 * 60 * 24));
        s2day = day.toGMTString();
        document.cookie = key + "=" + escape(data) + ";expires=" + s2day+ ";path=/";
    };

    // クッキーを取得する
    ins._getCookie = function(key){
        if(typeof(key) == "undefined")return "";
        key+="=";
        var scookie = document.cookie + ";";
        var start = scookie.indexOf(key);
        if (start != -1){
            end = scookie.indexOf(";", start);
            var data = unescape(scookie.substring(start + key.length, end));
        }else{
            var data='';
        }
        return data;
    };

    // スコープバインド
    ins._bind = function(){
        var args=[];
        if(arguments){
            for(var i=0,n=arguments.length;i<n;i++){
                args.push(arguments[i]);
            }
        }
        var object=args.shift();
        var func=args.shift();
        return function(event) {
            return func.apply(object,[event||window.event].concat(args));
        }
    };
    
    // クラス名を判別する
    ins._checkclass = function(node,classname){
        var reg = new RegExp('(?:^|\\s)'+classname+'(?:$|\\s)');
        if(reg.test(node.className)){ return true; }
        else { return false; };
    };

    // ポップアップ
    ins._openWin = function (theURL,winName,features) { 
        //Google Chrome用
        if (navigator.userAgent.indexOf('Chrome/') > 0) {
            if (window.win) {
                window.win.close();
                window.win = null;
            }
        }
        window.win = window.open(theURL,winName,features);
        window.win.focus();
    };

    // URL改行
    ins._urlBreak = function (node,classname){
        var elm = document.getElementById(node);
        var arr = [];

        (function(node,classname){
         var cnodes = node.childNodes;
         for(var i=0,n=cnodes.length;i<n;i++){
            if(cnodes[i].nodeType == 1){
                if(ins._checkclass(cnodes[i],classname)){ arr.push(cnodes[i]); }
                else { arguments.callee(cnodes[i],classname); };
            }
         }
        })(elm,classname);

        for(var j=0,n=arr.length;j<n;j++){
            var tmp = '';
            try{
                for(var i=0,m=arr[j].innerHTML.length;i<m;i++){
                    tmp += String(arr[j].innerHTML).charAt([i])+'<wbr />';
                }
                arr[j].innerHTML = tmp;
            } catch(e){};
        }
    };

    // 画像ロールオーバー
    ins._rollOver = function(classname){
        var list = ['img','input'];
        for(var j=0,m=list.length;j<m;j++){
            var img = document.getElementsByTagName(list[j]);
            for(var i=0,n=img.length;i<n;i++){
                if(ins._checkclass(img[i],classname)){
                    var obj = new Image();
                    obj.src = img[i].src.replace(/\.(jpg|gif|png)/, "on.$1");
                    img[i].onmouseover = function(){ 
                        this.setAttribute("src", this.getAttribute("src").replace(/\.(jpg|gif|png)/, "on.$1"));
                    };
                    img[i].onmouseout = function(){ 
                        this.setAttribute("src", this.getAttribute("src").replace(/on\.(jpg|gif|png)/, ".$1"));
                    };
                }
            }
        }
    };

    // スクロールアップ
    ins._scrollUp = {
        _x: function(){
            return document.documentElement.scrollLeft || document.body.scrollLeft;
        },
        _y: function(){
            return document.documentElement.scrollTop || document.body.scrollTop;
        },
        bottom_y: function(){
              var allh = document.documentElement.scrollHeight || document.body.scrollHeight;
              var displayh = document.documentElement.clientHeight || document.body.clientHeight;
              return (allh - displayh);
          },
        _gotox: function(elm){
            var element = document.getElementById(elm);
            var px = 0;
            while(element){
                px += element.offsetLeft;
                element = element.offsetParent;
            }
            return px;
        },
        _gotoy: function(elm){
            var element = document.getElementById(elm);
            var px = 0;
            while(element){
                px += element.offsetTop;
                element = element.offsetParent;
            }
            var v = ins._scrollUp.bottom_y();
            if(v-px < 0){ px=v; return px; }
            return px;
        },
        setScroll: function(tag){
               var i;
               var alist = document.getElementsByTagName(tag);
               for(i=0,n=alist.length;i<n;i++){
                   var att = alist[i].href;
                   var hrefname = att.split("#");
                   var host = location.href.replace("#","");
                   if(hrefname[0] == host){
                       if(hrefname[1] == ""){
                           pluto.base.addEvent(alist[i],"click",function(e){ pluto.html.scrollmove(e,0,0); });
                       }
                       else {
                           var x = ins._scrollUp._gotox(hrefname[1]);
                           var y = ins._scrollUp._gotoy(hrefname[1]);
                           (function(x,y){
                            pluto.base.addEvent(alist[i],"click",function(e){ pluto.html.scrollmove(e,x,y); });
                            })(x,y);
                       }
                   }
               }
           },
            movescroll: function(e,to_x,to_y) {
                var x_value = (to_x - this._x())/this.easing;
                var y_value = (to_y - this._y())/this.easing;
                window.scrollBy(x_value,y_value);
                if(Math.abs(y_value) >= 3){
                    var tid = setTimeout("pluto.html.scrollmove('"+e+"',"+to_x+","+to_y+")",this.speed);
                }else {
                    clearTimeout(tid);
                }
                pluto.base.preventDefault(e);
            }
    };

    // Flash設置
    ins._flashVersion = function(){
        this.f_var = 0;
        this.plugin = ( navigator.mimeTypes && navigator.mimeTypes["application/x-shockwave-flash"] ?navigator.mimeTypes["application/x-shockwave-flash"].enabledPlugin : 0 );
    };

    ins._flashVersion.prototype = {
        setFlash: function(){
            var args = arguments[0]
            var node = document.getElementById(args['id']);
            var version = this.getVersion();
            var transparam, transparam2;
            if( args["transparent"]){
                transparam = '<param name="wmode" value="transparent" />';
                transparam2 = ' wmode="transparent"';
            } else {
                transparam = "" ;
                transparam2 = "" ;
            }

            if(version >= args['version']){
                var m = '';
                m += '<object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000"width="'+ args['width'] +'" height="'+ args['height'] +'">';
                m += '<param name="movie" value="'+ args['filepath'] +'" />';
                m += transparam;
                m += '<param name="FlashVars" value="value" />';
                m += '<embed src="'+ args['filepath'] +'" quality="high" pluginspage="http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash" type="application/x-shockwave-flash" width="'+ args['width'] +'" height="'+ args['height'] +'"' + transparam2 + '></embed>';
                m += '</object>';
                node.innerHTML = m;
            } else {
                node.innerHTML = args['source'];
            }
        },
        getVersion: function(){
            var version;
            if( this.plugin ){
                //for modern browser
                version = parseInt(this.plugin.description.match(/\d+\.\d+/));  
            } else {
                //for ie
                try {
                    var flash = new ActiveXObject("ShockwaveFlash.ShockwaveFlash").GetVariable("$version").match(/([0-9]+)/);
                    version = parseInt(flash[0]);
                } catch (e) {
                    version = 0;
                }
            }
            return version;
        }
    };

    // フォントサイズ変更機能
    ins._fontController = function(){
        var args = arguments[0];
        this.outputID = args['outputID'];
        this.source = args['source'];
        document.getElementById(this.outputID).innerHTML = this.source;

        this.sbtn = document.getElementById(args['smallID']);
        this.rbtn = document.getElementById(args['resetID']);
        this.lbtn = document.getElementById(args['largeID']);
        this.rate = args['rate'];
        this.cookieName = args['cookiename'];

        this.body = document.getElementsByTagName('body')[0];
        this.currentValue = parseInt(ins._getCookie(this.cookieName));
        if(!this.currentValue){
            this.currentValue = args['current'];
        }
        this.originalValue = args['current'];
    };


    ins._fontController.prototype = {
        decrease: function (){
              if(this.rate < (this.currentValue - this.rate)){
                  this.currentValue -= this.rate;
                  this.setStyle();
              }
          },
        reset: function(){
           this.currentValue = this.originalValue;
           this.setStyle();
       },
        increase: function(){
              this.currentValue += this.rate;
              this.setStyle();
          },
        setStyle: function(){
              if(this.currentValue == this.originalValue){
                  this.rbtn.className = 'reset-btn-on'; 
                  this.lbtn.className = 'large-btn'; 
                  this.sbtn.className = 'small-btn'; 
              } else if(this.currentValue > this.originalValue){
                  this.rbtn.className = 'reset-btn'; 
                  this.lbtn.className = 'large-btn-on'; 
                  this.sbtn.className = 'small-btn';
              } else {
                  this.rbtn.className = 'reset-btn'; 
                  this.lbtn.className = 'large-btn'; 
                  this.sbtn.className = 'small-btn-on';  
              }
              this.body.style.fontSize = this.currentValue+'%';
              ins._setCookie(this.cookieName,this.currentValue,2);
          },
        setController: function(){
                   this.setStyle();
                   var name = ['decrease','reset','increase'];
                   var id = [this.sbtn,this.rbtn,this.lbtn];
                   for(var i=0,n=id.length;i<n;i++){
                       ins._addEvent(id[i], 'click', ins._bind(this,this[name[i]]));
                   }
               }
    };
    // 背景色
    ins._bgcolorController = function(){
        var args = arguments[0];

        document.getElementById(args['outputID']).innerHTML = args['source'];

        this.path = document.getElementById('master').href.replace(/master\.css/,'') + args['directory'];
        this.presetbtn = document.getElementById(args['preset']);
        this.btn = new Array();
        for(var i=0, n=args['color'].length; i<n; i++ ){
            var elm = document.getElementById(args['color'][i]);
            var name = args['color'][i];
            this.btn.push({'elm': elm, 'name': name});
        }

        this.cookieName = args['cookiename'];
        this.presetValue = args['preset'];

        this.currentValue = ins._getCookie(this.cookieName);
        if(!this.currentValue){
            this.currentValue = this.presetValue;
        }else if(this.currentValue != this.presetValue){
            this.change(this.currentValue);
        }
        this.setController();
    };

    ins._bgcolorController.prototype = {
        reset: function(){
            this.currentValue = this.presetValue;
            var link = document.getElementById('change');
            if(link) link.parentNode.removeChild(link);

            for(var i=0,n=this.btn.length; i<n; i++){
                if(this.btn[i].elm.className.indexOf('-on') != -1){
                    this.btn[i].elm.className =
                        this.btn[i].elm.className.replace(/-on/, '');
                }
            }
            ins._setCookie(this.cookieName,this.presetValue,2);
        },

        change: function(name){
            this.currentValue = name;
            var link = document.getElementById('change');
            if(!link){
                this.create();
            }else{
                link.href = this.path + this.currentValue + '.css';
            }
            this.setStyle();
        },

        create: function(){
            link = document.createElement('link');
            var head = document.getElementsByTagName('head')[0];
            link.id = 'change';
            link.rel = 'stylesheet';
            link.type = 'text/css';
            link.href = this.path + this.currentValue + '.css';
            head.appendChild(link);
        },

        setStyle: function(){
		    if(this.currentValue != this.presetValue){
                for(var i=0,n=this.btn.length; i<n; i++){
                    if((this.btn[i]['name'] == this.currentValue) && (this.btn[i].elm.className.indexOf('-on') == -1)){
                        this.btn[i].elm.className = this.btn[i].elm.className + '-on';
                    }else if(this.btn[i]['name'] != this.currentValue){
                        if(this.btn[i].elm.className.indexOf('-on') != -1){
                            this.btn[i].elm.className =
                                this.btn[i].elm.className.replace(/-on/, '');
                        }
                    }
                }
            }
            ins._setCookie(this.cookieName,this.currentValue,2);
        },

        setController: function(){
               this.setStyle();

               ins._addEvent(this.presetbtn, 'click', ins._bind(this, this.reset));
               for(var i=0,n=this.btn.length; i<n; i++){
                   ins._addEvent(this.btn[i]['elm'], 'click', (function(_this,_i){
                           return function(){
                               _this.change.apply(_this, [_this.btn[_i]['name']]);
                           }
                       })(this, i)
                   );
               }
        }
    };

    ins._headerhack = function(cookiename,current){
        var head = document.getElementsByTagName('head')[0];
        var currentValue = parseInt(ins._getCookie(cookiename));
        if(!currentValue){
            currentValue = current;
        };
        var hack = document.createElement('div');           
        hack.innerHTML='div<style type="text/css"><!--body { font-size: '+currentValue+'%; }--></style>';
        head.appendChild(hack.getElementsByTagName('style')[0]);
    };
	
	//ページャー設定
    ins._pagenation = function(){
        var args = arguments[0];

        this.current = 1;
        this.currentClass = args['currentClass'];
        this.targetAreaId = args['targetId'];
        this.targetArea;
        //this.newsListRange = args['list'];
        this.newsListRange = args['newsList'];
        this.blogListRange = args['blogList'];
        this.pagenationArea;
        this.pagenationId = args['pagenationId'];
        this.pagenationClass = args['pagenationClass'];
        this.pagenationRange = args['range'];
        this.targetArea;
        this.newsTag = 'dl';
        this.newsList;
        this.newsLength;
        this.maxPagenationNumber;
        this.startTag = '<ul>';
        this.innerTag;
        this.endTag = '</ul>';

        this.initialize();
    };


    ins._pagenation.prototype = {
    initialize : function(){
             this.targetArea = document.getElementById(this.targetAreaId);

             if (this.targetArea.className === 'work-list-area') {
                this.newsList = this.getElementsByClassName(this.targetArea, 'work-list-area-in clearfix');

                this.newsListRange = this.blogListRange;
             } else {
                this.newsList = this.targetArea.getElementsByTagName(this.newsTag);
                this.newsListRange = this.newsListRange;
             }

             this.newsLength = this.newsList.length;
             this.maxPagenationNumber = Math.ceil(this.newsLength / this.newsListRange);

             if(this.newsLength < this.newsListRange) return;
         
             for(var i = 0; i < this.newsLength; i++){
                 if(i >= this.newsListRange){
                    this.newsList[i].style.display = 'none';
                    this.newsList[i].style.overflow = 'hidden';
                 }
             }
             this.createPagenation();
         },
    createPagenation : function(){
              this.innerTag = '';
              if(document.getElementById(this.pagenationId)){
                  document.getElementById(this.pagenationId).parentNode.removeChild(document.getElementById(this.pagenationId));
              }
         
              if(this.current != 1){
                  this.innerTag += '<li class="prev"><a href="javascript:void(0);" id="pagenation-' + (this.current - 1) + '">&laquo; 前へ</a></li>';
              }
         
              for(var i = 1; i <= this.maxPagenationNumber; i++){
                  if(i == this.current){
                      this.innerTag += '<li class="' + this.currentClass +'"><a href="javascript:void(0);" class="current-page">' + i + '</a></li>\n';
                  }else if(i > (this.current - this.pagenationRange) && i < ((this.current-0) + (this.pagenationRange-0))){
                      this.innerTag += '<li><a href="javascript:void(0);" id="pagenation-' + i + '">' + i + '</a></li>\n';
                  }
              }
         
              if(this.current != this.maxPagenationNumber && this.maxPagenationNumber != 1){
                  this.innerTag += '<li class="next"><a href="javascript:void(0);" id="pagenation-' + ((this.current-0) + 1) + '">次へ &raquo;</a></li>';
              }
         
              this.pagenationArea = document.createElement('ul');
              this.pagenationArea.id = this.pagenationId;
              this.pagenationArea.className = this.pagenationClass;
              //this.pagenationArea.innerHTML = this.startTag + this.innerTag + this.endTag;
              this.pagenationArea.innerHTML = this.innerTag;
              this.targetArea.parentNode.insertBefore(this.pagenationArea, this.targetArea.nextSibling);
              this.setPagenationEvent();
        },
    setPagenationEvent : function(){
              var targetLinks = this.pagenationArea.getElementsByTagName('a');
              var length = targetLinks.length;
              for(var i = 0; i < length; i++){
                  if (targetLinks[i].className === 'current-page') {
                      continue;
                  }

                  targetLinks[i].onclick = (function(_this, func, offset){
                      return function(event){
                          func.apply(_this, [event||window.event, offset]);
                      };
                  })(this, this.resetNewsList, targetLinks[i].id.split('-').pop());
              }
        },
    resetNewsList : function(e, offset){
              this.current = offset;
              var start = offset * this.newsListRange - this.newsListRange;
              for(var i = 0; i < this.newsLength; i++){
                  if(i >= start && i < start + this.newsListRange){
                      this.newsList[i].style.display = 'block';
                      this.newsList[i].style.overflow = 'visible';
                  }else{
                      this.newsList[i].style.display = 'none';
                      this.newsList[i].style.overflow = 'hidden';
                  }
              }
              this.createPagenation();
        },
    getElementsByClassName : function(elm, className) {
        var allElm = elm.getElementsByTagName('*');
        var elmLen = allElm.length;
        var pattern = new RegExp("(^|\\s)"+className+"(\\s|$)");
        var nodeList = new Array();

        for(var i = 0, j = 0; i < elmLen; i++){
            var elem = allElm[i];
            if(! elem.className) continue;
            if(! pattern.test(elem.className)) continue;

            nodeList[j++] = elem;
        }
        return nodeList;
    }
   };

    ins._headerhack = function(cookiename,current){
        var head = document.getElementsByTagName('head')[0];
        var currentValue = parseInt(ins._getCookie(cookiename));
        if(!currentValue){
            currentValue = current;
        };
        var hack = document.createElement('div');           
        hack.innerHTML='div<style type="text/css"><!--body { font-size: '+currentValue+'%; }--></style>';
        head.appendChild(hack.getElementsByTagName('style')[0]);
    };
	
    // 印刷設定機能
    ins._printBtn = function(node,type){
        var elm = document.getElementById(node);
        var html;
        var allhtml = '<ul><li class="print001"><a href="#" id="print-all-btn">全画面プリント</a></li><li class="print002"><a href="#" id="print-part-btn">本文プリント</a></li></ul>';
        var parthtml = '<ul><li class="print002"><a href="#" id="print-part-btn">本文プリント</a></li></ul>';
        (type == 'part') ? html=parthtml : html=allhtml;
        elm.innerHTML = html;
    
        
        var elmpart = document.getElementById('print-part-btn');
        var elmall = document.getElementById('print-all-btn');
        ins._addEvent(elmpart,'click',function(e){ ins._setCss('part');ins._preventDefault(e); });
        if(elmall){
            ins._addEvent(elmall,'click',function(e){ ins._setCss('all');ins._preventDefault(e); });
        }       
    };

    ins._setCss = function(type){
        var head=document.getElementById('printcss');
        var defpath = head.getAttribute('href');
        
        if(type == 'part'){
            var path = defpath.replace('print.css','print-part.css');
        } else {
            var path = defpath.replace('print-part.css','print.css');
        }
        head.setAttribute('href',path);
    	setTimeout( function() {
			window.print();
		}, 1000);
        
    };
    
    
    
    // オートアイコン機能
    ins._autoIcon = function(){
    var args = arguments[0];
        var anodes = document.getElementsByTagName('a');
    for(var i=0,n=anodes.length;i<n;i++){
        var links = anodes[i].href;
        var ext = links.split('.');
            
        // 特定のclassには適用させない
        if(ins._checkclass(anodes[i],args['cancel'])){ continue; }
            
        for(var j=0,m=args['data'].length;j<m;j++){
            if(ext[ext.length-1] == args['data'][j]){
            if(anodes[i].className != ''){
                var classes = anodes[i].className;
            var arr = classes.split(' ');
            arr.push(args['data'][j]+'-icon');
            anodes[i].className = arr.join(' ');
            } else {
                anodes[i].className = args['data'][j]+'-icon';
            }
        }
        }
    }
    };

    // フォーム色変更
    ins._bgChanger = function(){
        var args = arguments[0];
        this.name = args['formname'];
        this.beforecolor = args['normalbg'];
        this.beforeborder = args['normalborder'];
        this.aftercolor = args['currentbg'];
        this.afterborder = args['currentborder'];
        this.flag = true;
    };

    ins._bgChanger.prototype={
        setEvent: function(){
              var inputnode = document[this.name].getElementsByTagName('input'); 
              for(i=0,n=inputnode.length;i<n;i++){
                  if(inputnode[i].type=='text'){
                      this.typeINPUT(inputnode[i]);
                  }
              };

              inputnode = document[this.name].getElementsByTagName('select');
              for(i=0,n=inputnode.length;i<n;i++){
                  this.typeSELECT(inputnode[i]);
              };

              inputnode = document[this.name].getElementsByTagName('textarea');
              for(i=0,n=inputnode.length;i<n;i++){
                  this.typeTEXTAREA(inputnode[i]);
              };
          },
        typeINPUT: function(obj){
               obj.style.backgroundColor = this.beforecolor;
               obj.style.border = this.beforeborder;
               ins._addEvent(obj,'focus',ins._bind(this,this.change,obj));
               ins._addEvent(obj,'blur',ins._bind(this,this.restore,obj));
           },
        typeSELECT: function(obj){
                ins._addEvent(obj,'mouseover',ins._bind(this,this.change,obj));
                ins._addEvent(obj,'mouseout',ins._bind(this,this.restore,obj));
                ins._addEvent(obj,'mousedown',ins._bind(this,this.eventclear));
                ins._addEvent(obj,'focus',ins._bind(this,this.change,obj));
                ins._addEvent(obj,'blur',ins._bind(this,this.restore,obj));
                ins._addEvent(obj,'change',ins._bind(this,this.eventset,obj));
            },
        typeTEXTAREA: function(obj){
                  ins._addEvent(obj,'focus',ins._bind(this,this.change,obj));
                  ins._addEvent(obj,'blur',ins._bind(this,this.restore,obj));
              },
        change: function(e,obj){
            obj.style.backgroundColor = this.aftercolor;
            obj.style.border = this.afterborder;
        },
        restore: function(e,obj){
             if(this.flag){
                 obj.style.backgroundColor = this.beforecolor;
                 obj.style.border = this.beforeborder;
             }
         },
         eventclear: function(){ 
                this.flag = false;
            },
         eventset: function(e,obj){ 
              this.flag = true;
              obj.blur();
              window.focus();
          }
    };

    // 入力エラー時の背景変更
    ins._error = function(){
        var args = arguments[0];
        var errorList = document[args['formname']][args['errorname']].value.split(',');
        if(errorList != ''){
            for(var i=0,n=errorList.length;i<n;i++){
                try{
                    document.getElementById(errorList[i]).style.backgroundColor = args['errorbg'];
                    document.getElementById(errorList[i]).style.border = args['errorborder'];
                } catch(e){};
            }
        }
    };
    
    // テキストフィールドのデフォルト文字表示
    ins._defaultValue = function(){
        var args = arguments[0];
        var tnodes = [];
        
        var nodes = document.getElementsByTagName('input');
        for(var i=0,n=nodes.length;i<n;i++){
            if(ins._checkclass(nodes[i],args["classname"])){ 
            var obj = {};
        obj['node'] = nodes[i];
        obj['value'] = nodes[i].value;
        tnodes.push(obj);
        }
        }
        
    for(var i=0,n=tnodes.length;i<n;i++){
        (function(){ 
            var num = i;
        tnodes[num]['node'].onfocus = function(){
            if(this.value == tnodes[num]['value']){ this.value = ''; };
            this.style.color = args['inputcolor'];
        };
        tnodes[num]['node'].onblur = function(){
            if(this.value == ''){
                        this.value = tnodes[num]['value'];
                this.style.color = args['defcolor'];
            }
        };
        }());
    }
    };

    // google co-op設定
    ins._coop = function() {
        var args = arguments[0];  
        var target_form = document.getElementById(args['formid']);
        var target_id = document.getElementById("cms-google-coop");

        if(target_form){
            target_form.q.style.border = args['searchborder']; 
            target_form.q.style.width = args['searchwidth'];
            target_form.q.style.height = args['searchheight'];
            target_form.q.style.padding = args['searchpadding'];
        } else if(target_id) {
            var tnode = target_id.getElementsByTagName("INPUT");
            for(var i=0;i<tnode.length;i++){
                if(tnode[i].name=="q"){
                    tnode[i].style.border = args['searchborder'];
                    tnode[i].style.width = args['searchwidth'];
                    tnode[i].style.height = args['searchheight'];
                    tnode[i].style.padding = args['searchpadding'];
                }
            }
        }
    };


    // トップページ新着エリアタブ機能
    ins._tabs = function(){
        var args = arguments[0];
        this.node = document.getElementById(args['node']);
        this.tabarr = args['tabarr'];
        this.tab = document.getElementById(args['currenttab']);
        this.tabcon = document.getElementById(args['currenttab']+"-content");
        this.bool = args['setattribute'];
    };

    ins._tabs.prototype = {
        initialize: function(){
            this.changeContent('',this.tabcon);
            if(this.bool){
                this.setAttribute('',this.tab);
            }
            this.changeTab();
        },
        changeTab: function(){


            for(var i=0,n=this.tabarr.length;i<n;i++){
                var node = document.getElementById(this.tabarr[i]);
                var nodeCon = document.getElementById(this.tabarr[i]+"-content");
                
                ins._addEvent(node,'click',ins._bind(this,this.changeContent,nodeCon));
                if(this.bool){
                    ins._addEvent(node,'click',ins._bind(this,this.setAttribute,node));
                }
            }
        },
        changeContent: function(e,nodeCon){
            for(var i=0,n=this.tabarr.length;i<n;i++){
                document.getElementById(this.tabarr[i]+"-content").style.display = 'none';
            }           
            nodeCon.style.display = 'block';
            ins._preventDefault(e);
        },
        setAttribute: function(e,node){
            for(var i=0,n=this.tabarr.length;i<n;i++){
                var nodes = document.getElementById(this.tabarr[i]).getElementsByTagName('a');
                nodes[0].className = '';
            }
            
            var anodes = node.getElementsByTagName('a');
            anodes[0].className = 'current';
            ins._preventDefault(e);
        }
    };

    // smartphone誘導
    ins._smartphoneguide = function(args) {
        this.path = document.getElementById('master').href
                    .replace(/common\/css\/master\.css/,'')
                    .replace(/\/+$/, '') + '/';

        this.spDirName = args.spDirName;
        this.btnClass = args.btnClass;
        this.image = args.image;
        this.alt = args.alt;

        var bodyNode = document.getElementById(args.bodyNode),
            parentNode = bodyNode.parentNode;

        if (args.range === 'top' && !this.isToppage(bodyNode)) {
            return;
        }

        if (this.isSmartphone()) {
            if (args.type === 'btn') {
                parentNode.insertBefore(this.createBtnNode(), bodyNode);
            } else if (args.type === 'auto') {
                var currentPath = 'http://' + location.hostname + location.pathname,
                dirName = currentPath.replace(this.path, '');
                location.href = this.path + this.spDirName + dirName;
            }
        }
    };
    ins._smartphoneguide.prototype.createBtnNode = function() {
        var node = document.createElement('p');
        node.className = this.btnClass;
        var currentPath = 'http://' + location.hostname + location.pathname,
        dirName = currentPath.replace(this.path, '');
        node.innerHTML = '<a href="' + this.path + this.spDirName + dirName + '"><img src="'
                    + this.path
                    + this.image +'" width="100%" alt="'
                    + this.alt + '" /></a>';
        return node;
    };
    ins._smartphoneguide.prototype.isSmartphone = function() {
        var agent = navigator.userAgent;
        if (agent.search(/iPhone/) != -1
            || agent.search(/iPod/) != -1
            || agent.search(/Android/) != -1) {
                return true;
            }
        return false;
    };
    ins._smartphoneguide.prototype.isToppage = function(bodyNode) {
        return bodyNode.className === 'home';
    };
	
	// 動画要素設定機能
	ins._movieElement = function(classname) {
		
		this.divTag = document.getElementsByTagName("div");
		
		for(var i=0,n=this.divTag.length;i<n;i++){
			
			
			if(this.divTag[i].className == classname){//ページにclassnameがある場合実行

				var agent = navigator.userAgent;

				for(var i=0,n=this.divTag.length;i<n;i++){
					if(this.divTag[i].className == "flash-box-pc"){
						var pcArea = this.divTag[i]
						if(agent.search(/iPhone/) != -1 
							|| agent.search(/Android/) != -1 
							|| agent.search(/iPad/) != -1 
							|| agent.search(/iPod/) != -1){
								pcArea.style.display = 'none';
						}else{
							pcArea.style.display = 'block';
						}
					} else if (this.divTag[i].className == "flash-box-sp"){
						var spArea = this.divTag[i]
						if(agent.search(/iPhone/) != -1 
							|| agent.search(/Android/) != -1 
							|| agent.search(/iPad/) != -1 
							|| agent.search(/iPod/) != -1){
							spArea.style.display = 'block';
						}else{
							spArea.style.display = 'none';
						}
					}

				}
			}
		}


	};

    /**
     * アクセス用メソッド
     */

    // HTML関連
    this.html = {};

    // ポップアップ
    this.html.openWin = function (theURL,winName,features) { 
        ins._openWin(theURL,winName,features);
    };

    // URL改行
    this.html.urlBreak = function (node,classname) { 
        if(document.getElementById(node)){
            ins._urlBreak(node,classname);
        }
    };

    // 画像ロールオーバー
    this.html.rollOver = function (classname) {
        ins._rollOver(classname);
    };

    // スクロールアップ
    this.html.scrollUp = function(){
        var args = arguments[0];
        ins._scrollUp.speed = args['speed'];
        ins._scrollUp.easing = args['easing'];
        ins._scrollUp.setScroll(args['tag']);
    };
    this.html.scrollmove = function(e,to_x,to_y){
        ins._scrollUp.movescroll(e,to_x,to_y);
    };

    // Flash設置
    this.html.flashVersion = function(){
        var allargs = arguments;
        for(var i=0,n=allargs.length;i<n;i++){
            var args = allargs[i];
            if(document.getElementById(args['id'])){
                var s = new ins._flashVersion();
                s.setFlash(args);
            }
        }
    };

    // フォントサイズ変更
    this.html.fontController = function(){
        var args = arguments[0];
        if(document.getElementById(args['outputID'])){
            var s = new ins._fontController(args);
            s.setController();
        }
    };
    this.html.headerhack = function(cookiename){
        if (navigator.userAgent.indexOf("MSIE 6.0") == -1) {
            ins._headerhack(cookiename);
        }
    };
    // 背景色変更
    this.html.bgcolorController = function(){
        var args = arguments[0];
        if(document.getElementById(args['outputID'])){
            new ins._bgcolorController(args);
        }
    };
    
    // 新着カテゴリページ送り機能
    this.html.pagenation = function(){
        var args = arguments[0];
        if(document.getElementById(args['targetId'])){
            new ins._pagenation(args);
        }
    }

    
    // 印刷設定機能
    this.html.printBtn = function(){
        var args = arguments[0];
        if(document.getElementById(args['node'])){
            ins._printBtn(args['node'],args['type']);
        }
    };

    // オートアイコン
    this.html.autoIcon = function(){
        var args = arguments[0];
        ins._autoIcon(args);
    };

    // トップページ新着エリアタブ機能
    this.html.tabs = function(){
        var args = arguments[0];
        if(document.getElementById(args['node'])
          && document.getElementById(args['homeID']).className===args['homeClass']){
            ins._tabs(args);
            var s = new ins._tabs(args);
            s.initialize();
        }
    };
	
	// smartphone誘導
    this.html.smartphoneguide = function() {
        var args = arguments[0];
        new ins._smartphoneguide(args);
    };
	
	// 動画要素設定機能
    this.html.movie = function(classname) {
		ins._movieElement(classname);
    };



    // form関連
    this.form = {};

    // フォーム色変更
    this.form.bgChanger = function(){
        var args = arguments[0];
        if(document[args['formname']]){
            var s = new ins._bgChanger(args);
            s.setEvent();
        }
    };

    // 入力エラー時の背景変更
    this.form.error = function(){
        var args = arguments[0];
        try{
            var d = document[args['formname']][args['errorname']];
            ins._error(args);
        }catch(e){};
    };
    
    // テキストフィールドのデフォルト文字表示
    this.form.defaultValue = function(){
        var args = arguments[0];
        ins._defaultValue(args);
    };

    // google関連
    this.google = {};

    // google coop
    this.google.coop = function(args) {
        ins._coop(args); 
    };


    

    // 共通 
    this.base = {};

    // イベントストップ
    this.base.preventDefault = function(e){
        ins._preventDefault(e);
    };

    // イベントアタッチ
    this.base.addEvent = function(node,evt,func){
        ins._addEvent(node,evt,func);
    };

    // イベントデタッチ
    this.base.removeEvent = function (node,evt,func){
        ins._removeEvent(node,evt,func);
    };

    // クッキーをセットする
    this.base.setCookie = function(key,data,term){
        ins._setCookie(key,data,term);
    };

    // クッキーを取得する
    this.base.getCookie = function(key){
        ins._getCookie(key);
    };

    // スコープバインド
    this.base.bind = function(){
        ins._bind();
    };
};

