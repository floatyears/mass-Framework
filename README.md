mass Framework
==================
<p>这是一个前后通吃的框架。browers部分用于前端，从零开始建起，不依赖于其他框架。servers则构建在node.js，需要node.js的环境支持。<p>
<p>前后端通用的部分是其模块加载机制，虽然内部实现有点不一致，但调用是一致的。另，许多不基于DOM API与node.js API的模块也是前后端共用的。</p>
<p>前端是从模块加载开始，通过选择器构建<em>节点链对象</em>实现jQuery式的各种操作(数据缓存，样式操作，事件绑定，ajax调用，dom操作，属性操作），许多API都与jQuery非常相近。在语言底层，通过对ecma262v5新API的检测与补丁支持确保连IE6也能使用 String.prototype.trim, Array.prototype.forEach, Array.prototype.map,Array.prototype.filter, Array.prototype.reduce, Function.prototype.bind 等高级API， 享受Prototype.js那种编程快感。另，还通过dom.lang(aaa)生成<em>语言链对象</em>对字符串，数字，数组，类数组,对象这几种数据类型提供更多便捷操作, 这在保证不污染原型的同时, 亦能够进行链式操作。</p>
<p>在大规模开发过程，mass Framework提供以下特征确认迅敏开发：</p>
<ol>
<li>多库共存。</li>
<li>多版本共存。</li>
<li>模块系统，将不同业务的代码封存于不同的JS文件中，确保自治，加载时能自行处理依赖关系。</li>
<li>模板机制，提供format，tag, ejs 这三种级别的字符串拼接方法（后两个是用于生成HTML代码片断）。</li>
<li>异步列队，将一组方法延迟到末来某一时间点时执行，并能应对时间上的异常捕获。</li>
<li>操作流，将多个相关的回调组织起来，避免回调套嵌，将串行等待变成并行等待,一处合并，多处触发。</li>
<li>自定义事件，方便设置UI的各种行为。</li>
<li>类工厂。</li>
<li>AS3式的补帧动画系统。</li>
<li>CSS3 transform2D支持。</li>
</ol>
<p>后端部分，核心功能是手脚架，热部署，拦截器群集，MVC，ORM。它正在编写中，前三大功能基本成型。。。。</p>
<h3>mass的合并</h3>
<ol>
<li>将模块加载模块dom.js里面的内容先复制到一个临时文件</li>
<li>在其最后一行"})(this,this.document);" 与倒数第二行" dom.exports("dom"+postfix);"插入标识模块已加域的代码。
其代码如下：<br/>
<pre>
    var module_value = {
        state:2
    };
    var list = "ecma,lang,spec,support,class,data,query,node,css_ie,css,dispatcher,event,attr,fx,ajax".match(dom.rword);
    for(var i=0, module;module = list[i++];){
        map["@"+module] = module_value;
    }
</pre>
list里面的为要合并的模块名
</li>
<li>将其他模块里面的内容直接拷到上面的代码之下。</li>
</ol>
<p>成功后，整个代码结构如下：</p>
<pre>
(function(global , DOC){
//这是最核心的模块加载模块
//XXXXXXXXXXX
 //然后加上这样一段
    var module_value = {
        state:2
    };
    var list = "ecma,lang,spec,support,class,data,query,node,css_ie,css,dispatcher,event,attr,fx,ajax".match(dom.rword);
    for(var i=0, module;module = list[i++];){
        map["@"+module] = module_value;
    }
//然后把要合并的JS文件的内容直接抽取出来放在下面
   dom.define("ecma", function(){
//XXXXXXXXXXXXXX
});
   dom.define("lang", function(){
//XXXXXXXXXXXXXX
})
   dom.define("class", function(){
//XXXXXXXXXXXXXX
})
   dom.define("data", function(){
//XXXXXXXXXXXXXX
})
   dom.define("node", function(){
//XXXXXXXXXXXXXX
})
//....
})(this,this.document)
</pre>
<p>by 司徒正美 （zhongqincheng）</p>
<p>2011.11.15</p>
 <a href="http://www.cnblogs.com/rubylouvre/">http://www.cnblogs.com/rubylouvre/</a>