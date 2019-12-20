# 一些个小trick

[TOC]

## ++i 的效率比 i++高

<div id="main">
	<div id="mainContent">
	<div class="forFlow">
<div id="topics">
	<div class="post">
		<h1 class="postTitle">
			<a id="cb_post_title_url" class="postTitle2" href="https://www.cnblogs.com/AndyJee/p/4550391.html">（C++）i++和++i，哪个效率高一些</a>
		</h1>
		<div class="clear"></div>
		<div class="postBody">
			<div id="cnblogs_post_body" class="blogpost-body"><p>在看《程序员面试笔试宝典》时，发现了这样一个问题，书中只给出了++i的效率高一些，但并没有给出具体的解释和说明。</p>
<p>在网上找到下面的答案：</p>
<p><strong>1、从高级层面上解释</strong></p>
<p>++i 是i=i+1,表达式的值就是i本身</p>
<p>i++ 也是i=i+1,但表达式的值是加1前的副本，由于要先保存副本，因此效率低一些。</p>
<p>对于C++内置类型而言，大部分编译器会做优化，因此效率没什么区别。但在自定义类型上，就未必有优化，++i 效率会高一些。</p>
<p><strong>2、从底层汇编来看内置类型</strong></p>
<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_70464" class="syntaxhighlighter  csharp"><div class="toolbar"><span><a href="#" class="toolbar_item command_help help">?</a></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div><div class="line number9 index8 alt2">9</div><div class="line number10 index9 alt1">10</div><div class="line number11 index10 alt2">11</div><div class="line number12 index11 alt1">12</div><div class="line number13 index12 alt2">13</div><div class="line number14 index13 alt1">14</div><div class="line number15 index14 alt2">15</div><div class="line number16 index15 alt1">16</div><div class="line number17 index16 alt2">17</div><div class="line number18 index17 alt1">18</div><div class="line number19 index18 alt2">19</div><div class="line number20 index19 alt1">20</div><div class="line number21 index20 alt2">21</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="csharp keyword">int</code> <code class="csharp plain">a,i=0; a=++i;汇编代码如下：</code></div><div class="line number2 index1 alt1">&nbsp;</div><div class="line number3 index2 alt2"><code class="csharp keyword">int</code> <code class="csharp plain">a,i=0;</code></div><div class="line number4 index3 alt1"><code class="csharp plain">01221A4E mov dword ptr [i],0</code></div><div class="line number5 index4 alt2"><code class="csharp plain">a=++i;</code></div><div class="line number6 index5 alt1"><code class="csharp plain">01221A55 mov eax,dword ptr [i]</code></div><div class="line number7 index6 alt2"><code class="csharp plain">01221A58 add eax,1</code></div><div class="line number8 index7 alt1"><code class="csharp plain">01221A5B mov dword ptr [i],eax</code></div><div class="line number9 index8 alt2"><code class="csharp plain">01221A5E mov ecx,dword ptr [i]</code></div><div class="line number10 index9 alt1"><code class="csharp plain">01221A61 mov dword ptr [a],ecx</code></div><div class="line number11 index10 alt2">&nbsp;</div><div class="line number12 index11 alt1"><code class="csharp keyword">int</code> <code class="csharp plain">a,i=0; a=i++;汇编代码如下：</code></div><div class="line number13 index12 alt2">&nbsp;</div><div class="line number14 index13 alt1"><code class="csharp keyword">int</code> <code class="csharp plain">a,i=0;</code></div><div class="line number15 index14 alt2"><code class="csharp plain">009E1A4E mov dword ptr [i],0</code></div><div class="line number16 index15 alt1"><code class="csharp plain">a=i++;</code></div><div class="line number17 index16 alt2"><code class="csharp plain">009E1A55 mov eax,dword ptr [i]</code></div><div class="line number18 index17 alt1"><code class="csharp plain">009E1A58 mov dword ptr [a],eax</code></div><div class="line number19 index18 alt2"><code class="csharp plain">009E1A5B mov ecx,dword ptr [i]</code></div><div class="line number20 index19 alt1"><code class="csharp plain">009E1A5E add ecx,1</code></div><div class="line number21 index20 alt2"><code class="csharp plain">009E1A61 mov dword ptr [i],ecx</code></div></div></td></tr></tbody></table></div></div>
</div>
<p>从上述汇编代码可以看到，对于内置类型，它们的执行数目是一样的，效率没有差别。</p>
<p><strong>3、从重载运算符来看自定义类型</strong></p>
<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_753604" class="syntaxhighlighter  csharp"><div class="toolbar"><span><a href="#" class="toolbar_item command_help help">?</a></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div><div class="line number9 index8 alt2">9</div><div class="line number10 index9 alt1">10</div><div class="line number11 index10 alt2">11</div><div class="line number12 index11 alt1">12</div><div class="line number13 index12 alt2">13</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="csharp plain">Operator Operator::</code><code class="csharp keyword">operator</code><code class="csharp plain">++()</code></div><div class="line number2 index1 alt1"><code class="csharp plain">{</code></div><div class="line number3 index2 alt2"><code class="csharp plain">++value; </code><code class="csharp comments">//内部成员变量</code></div><div class="line number4 index3 alt1"><code class="csharp keyword">return</code> <code class="csharp plain">*</code><code class="csharp keyword">this</code><code class="csharp plain">;</code></div><div class="line number5 index4 alt2"><code class="csharp plain">}</code></div><div class="line number6 index5 alt1">&nbsp;</div><div class="line number7 index6 alt2"><code class="csharp plain">Operator Operator::</code><code class="csharp keyword">operator</code><code class="csharp plain">++(</code><code class="csharp keyword">int</code><code class="csharp plain">)</code></div><div class="line number8 index7 alt1"><code class="csharp plain">{</code></div><div class="line number9 index8 alt2"><code class="csharp plain">Operator temp;</code></div><div class="line number10 index9 alt1"><code class="csharp plain">temp.value=value;</code></div><div class="line number11 index10 alt2"><code class="csharp plain">value++;</code></div><div class="line number12 index11 alt1"><code class="csharp keyword">return</code> <code class="csharp plain">temp;</code></div><div class="line number13 index12 alt2"><code class="csharp plain">}</code></div></div></td></tr></tbody></table></div></div>
</div>
<p>从上面代码可以看出，后置++多了一个保存临时对象的操作，因此效率自然低一些。</p>
<p><strong>总结：</strong></p>
<p>对于C++内置类型，两者的效率差别不大；</p>
<p>对于自定义的类而言，++i 的效率更高一些。</p>
<p>&nbsp;</p>

