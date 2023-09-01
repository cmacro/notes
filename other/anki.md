# anki


## org

frontground

```html
{{单词}}
```

background

```html

<div class="section" style="display:block">
{{例句}} 
 </div>


<!-- back section -->
   
{{释义}}                       



<!--————button section————-->


<div id="sentence" style="display:none;">{{例句}}</div>
<div id="sentence2" style="display:none;">{{释义}}</div>


{{#发音}}
<div style="display:none;">
<div class='fayin' style='position:fixed;right:0%; top:90%;'>♪</div>
<div class='fayinw' style='position:fixed;right:0%; top:90%;'>{{发音}}</div>
</div>
{{/发音}}


<div class="zxfayin">
[sound:http://dict.youdao.com/dictvoice?audio={{单词}}&type=2]

<div style="position:fixed; top:45%;">
[sound:http://dict.youdao.com/dictvoice?audio={{text:例句}}&type=2] </div>


<div style="position:fixed; top:30%;">{{发音}}</div>
</div>


```

```css
/*——————
在划词作者炸毛的antimoon模板上修改了css样式，增加了一些功能，按钮功能来自cici和mmjang
Homepage:https://github.com/mmjang
由Ann整合而成
——————*/


/*global card style*/
.card {
  font-family          :"Helvetica";
  font-size              : 1.32em;
  text-align             : left;                        
  color                     :#369;
  background-color:#e9ebee;
  font-weight            :;bold; 
  text-align               : justify;
  overflow-x             :;hidden;
  word-spacing        :2px;
  letter-spacing          :0px;
}


/* section global style */
.section{
  line-height        :170%;
  border               : 1px solid;   
  border-color     : #e9ebee;                  
  border-radius    : 8px;  
  background       :#fff;
  opacity               :;0.5;
  position              : relative;      
  padding             :8px 12px 8px 12px;
}

br{
display:none;
}


/*————释义，例句，方框设置———*/
.njzj,.lwzj,.div_dictionary,.div_collins,.div_ode,.dictionary_vocab,.oxts,.solr_sentence,.eudic_sentence,.rrcd_sentence{
background             :#fff;
border-radius          :8px;
border                       :1px solid #fff;
margin                      :8px 0px 0px 0px;
padding                    :12px 12px 12px 12px;
line-height                :150%;
}

sy{
font-size:0.88em;
}


i{display:none;}

.fayin,.fayinw,.huaci a,huacia a,.rrcd_en a,.eudic_sentence a{
color:#365A7D;
}

/*回链格式定义*/
hl{display:block;color:#369;font-size:0.5em;text-align:right; padding-left:6px;line-height:100%;text-decoration:none;
}
hl a{
display:block;
color:#e9ebee;
}

/*例句字段中带 a标签的字符串格式设置，主要是 jump to source的格式设置*/
 .section a{display:block;text-decoration:none;color:#e9ebee;opacity:0;}

/*————————tag———————*/
tag{display:block;color:#369;font-size:0.5em;text-align:right; padding-right:-2px;line-height:100%;
border-top:0px solid rgb(245,245,245,0);}

#time,#info { 
font-size:10px; 
line-height:;28px; 
font-weight:;bold;
line-height:140%;
}


b{
font-weight:normal;
}

/*——例句和朗文中的单词加粗部分——*/
#sentence b{
  font-weight:normal;
  border-radius:3px;
  color:rgba(255,255,255,0.9);
  background-color:#ec6c4f;
  padding:0 3px;
}

danci
{font-size:1.28em;
font-weight:800;
letter-spacing:;0.5px;
color:#ec6c4f;
display:;none;
}
.vocab_hwd{display:none;}
syn{
font-size:0.82em;
}


/*————加粗————*/
b{
  font-weight:normal;
  border-radius:;3px;
  color:;#ec6c4f;
  background-color:;#369;
  padding:0 3px;  
  border-bottom:;2px solid #e68080;

}
/*——英文释义，例句，  加粗——*/
.endef b,.ys b,.dpss b,.psv b,.vocab_def b,.eudic_sentence b,.solr_en b,.solr_cn b,.dub91_en b,.eudic_en b,.rrcd_en b{
color:#ec6c4f;
background-color:;
border-bottom        :;2px solid #369;
font-weight             :normal;
}
/*————例句中的中文——————*/
.text_blue,.dpz,.zs{
font-weight:normal;
color:#ec6c4f;
display:initial;
font-weight:bold;
border-top:;100px solid #fff;
line-height:;200%;
font-size:0.72em;
}


/*button  style*/
.fayin,.fayinw,.huaci a,huacia a,.rrcd_en a,.eudic_sentence a{
display:block;
opacity                          :0.8;
text-decoration            :none;
background                   :rgb(255,255,255,0);
border-radius               :8px;
border                            :1px solid;
font-size                        :1.18em;
margin                            :5px;
padding                          :2px 20px 2px 20px;
font-weiht                      :normal;
z-index                            :9999;
}
.fayinw {opacity: 0;}
.fayin.zxfayin:active { opacity: 0.5; }

.zxfayin{
position:fixed;
right:-10%; 
top:60%;
font-size:100px;
opacity:0;
z-index:999999;
}


}
/*——人人,欧陆例句中的音频图标设置—*/
.rrcd_en a{
position:fixed;
right:20%; 
top:80%;
font-size:0.3em;
opacity:0;
display:;none;
}

.rrcd_sentence a,.solr_sentence a,.eudic_sentence a{
text-decoration:none;
position:fixed;
left:-15%; 
top:60%;
color:;#369;
font-size:100px;
display:block;
opacity:0;
z-index:999999;
}

/*—————例句格式设置——————*/
.solr_en,.dub91_en,.eudic_en,.rrcd_en{
line-height   :130%;
}

.solr_en b,.solr_cn b,.dub91_en b,.eudic_en b,.rrcd_en b{
color:#ec6c4f;
font-weight:normal;
}

.exp,.rrcd_cn,.solr_cn,.dub91_cn,.eudic_cn{
line-height   :125%;
font-size      :14px;
border-top   :;10px solid #fff;
}

.solr_cn,.mFoot,.dub91_cn{
border-top   :;10px solid #fff;
padding:10px 0;
}


.eudic_title,.rrcd_title,.solr_title,.dub91_title{
display             :;block;
color                 :#4E7CA0;
font-size           :0.5em;
text-align          :right;
padding-left      :6px;
text-decoration :none;
border-bottom  :5px solid #fff;
line-height         :150%;
}

.rrcd_img,.dub91_img{
text-align:center;
margin:-100px;
margin-right:-8px;
margin-left:-12px;
margin-top:0px;
min-width:339px;
height:200px;
}
.ankihelper_image{
min-width:337px;
height:200px;
margin:-8px -12px 0px -12px;

}


y{
font-size:0.72em;
color:#369;
opacity:0.8;
font-weight:800;
}



c,.nm,.zl,.dps,.xxl,.dp,.yl,y{display:none;}

/*——————中英切换功能
Author  : laohuang
Homepage: https://about.me/laohuang
——————*/
.hidden{
   display:none;
}
</style>
<script>
//hide or display trans
function toggleTrans(){
    var customized = []; //add your own css class here

    
    var ldoce4 = ['.L_DEC', '.L_CEX', '.L_DCH'];
    var collins = ['.text_blue'];
    var odh = ['.chn_tran', '.chn_sent'];
    var lw = ['.zs', '.zl','.dpz','.zsl','zs','danci'];
   var lj =['.mFoot','.exp','.solr_cn','.dub91_cn'];
    
    var transClass = customized.concat(ldoce4,collins,odh,lw,lj).join();

[].forEach.call(document.querySelectorAll(transClass), function(div) {
        div.classList.toggle('hidden');
    });
}

</script>
```