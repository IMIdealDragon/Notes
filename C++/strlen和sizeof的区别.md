<div id="content_views" class="markdown_views prism-atom-one-dark">
                    <!-- flowchart 箭头图标 勿删 -->
                    <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
                        <path stroke-linecap="round" d="M5,0 0,2.5 5,5z" id="raphael-marker-block" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);"></path>
                    </svg>
                                            <h4><a id="strlensizeof__0"></a>strlen和sizeof 区别</h4>
<h6><a id="sizeof__1"></a>一、sizeof 运算符：计算所占的字节大小</h6>
<p>sizeof()是运算符，其值在<strong>编译时</strong> 就已经计算好了，参数可以是数组、指针、类型、对象、函数等。</p>
<p><mark>它的功能是：获得保证能容纳实现所建立的最大对象的字节大小。</mark></p>
<p>由于在编译时计算，因此sizeof不能用来返回动态分配的内存空间的大小。实际上，用sizeof来返回类型以及静态分配的对象、结构或数组所占的空间，返回值跟对象、结构、数组所存储的内容没有关系。</p>
<p>具体而言，当参数分别如下时，sizeof返回的值表示的含义如下：</p>
<ol>
<li>数组——编译时分配的数组空间大小；</li>
<li>指针——存储该指针所用的空间大小（在32位机器上是4，64位机器上是8）；</li>
<li>类型——该类型所占的空间大小；</li>
<li>对象——对象的实际占用空间大小；</li>
<li>函数——函数的返回类型所占的空间大小。函数的返回类型不能是void。</li>
</ol>
<h6><a id="strlen__15"></a>二、strlen函数： 字符串的具体长度即字符个数</h6>
<p>通过查看 <a href="http://www.cplusplus.com/reference/cstring/strlen/?kw=strlen" rel="nofollow">strlen文档</a><br>
我们知道strlen(…)是函数，要在<strong>运行时</strong> 才能计算。参数必须是字符型指针（char*）。当数组名作为参数传入时，实际上数组就退化成指针了。</p>
<p><mark>它的功能是：返回字符串的长度。</mark><br>
该字符串可能是自己定义的，也可能是内存中随机的，该函数实际完成的功能是从代表该字符串的第一个地址开始遍历，直到遇到结束符’\0’停止。返回的长度大小不包括‘\0’。</p>
<h4><a id="_22"></a>总结一下二者的区别</h4>
<p>二者的区别主要是以下四点：</p>
<ol>
<li>sizeof()是运算符，strlen()是库函数</li>
<li>sizeof()在编译时计算好了，strlen()在运行时计算</li>
<li>sizeof()计算出对象使用的最大字节数，strlen()计算字符串的实际长度</li>
<li>sizeof()的参数类型多样化（数组，指针，对象，函数都可以），strlen()的参数必须是字符型指针（传入数组时自动退化为指针）</li>
</ol>
<h4><a id="_29"></a>出一个简单但是很有学问的题</h4>
<p>阅读下面代码，写出运算结果。</p>
<pre class="prettyprint"><code class="has-numbering" onclick="mdcp.copyCode(event)" style="position: unset;">void Test1()
{
	char p[] = "hello";
	cout &lt;&lt; "p: " &lt;&lt; p &lt;&lt; "   " &lt;&lt; strlen(p) &lt;&lt; "   " &lt;&lt; sizeof(p) &lt;&lt; endl;
	char p1[] = "hello\0";
	cout &lt;&lt; "p1: " &lt;&lt; p1 &lt;&lt; "   " &lt;&lt; strlen(p1) &lt;&lt; "   " &lt;&lt; sizeof(p1) &lt;&lt; endl;
	char p2[] = "hello\\0";
	cout &lt;&lt; "p2: " &lt;&lt; p2 &lt;&lt; "   " &lt;&lt; strlen(p2) &lt;&lt; "   " &lt;&lt; sizeof(p2) &lt;&lt; endl;
	char p3[] = "hello\\\0";
	cout &lt;&lt; "p3: " &lt;&lt; p3 &lt;&lt; "   " &lt;&lt; strlen(p3) &lt;&lt; "   " &lt;&lt; sizeof(p3) &lt;&lt; endl;
	char p4[] = "hel\0lo";
	cout &lt;&lt; "p3: " &lt;&lt; p4 &lt;&lt; "   " &lt;&lt; strlen(p4) &lt;&lt; "   " &lt;&lt; sizeof(p4) &lt;&lt; endl;
	char p5[] = "hel\\0lo";
	cout &lt;&lt; "p5: " &lt;&lt; p4 &lt;&lt; "   " &lt;&lt; strlen(p5) &lt;&lt; "   " &lt;&lt; sizeof(p5) &lt;&lt; endl;
}

<div class="hljs-button {2}" data-title="复制"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li><li style="color: rgb(153, 153, 153);">2</li><li style="color: rgb(153, 153, 153);">3</li><li style="color: rgb(153, 153, 153);">4</li><li style="color: rgb(153, 153, 153);">5</li><li style="color: rgb(153, 153, 153);">6</li><li style="color: rgb(153, 153, 153);">7</li><li style="color: rgb(153, 153, 153);">8</li><li style="color: rgb(153, 153, 153);">9</li><li style="color: rgb(153, 153, 153);">10</li><li style="color: rgb(153, 153, 153);">11</li><li style="color: rgb(153, 153, 153);">12</li><li style="color: rgb(153, 153, 153);">13</li><li style="color: rgb(153, 153, 153);">14</li><li style="color: rgb(153, 153, 153);">15</li><li style="color: rgb(153, 153, 153);">16</li></ul></pre>
<p>哈哈~  看到这里是不是就有些方了呢？<br>
其实仔细看我上面的分析，一步一步的计算就不会有错了。</p>
<p><strong>首先一定要记住：strlen 计算的是字符串的实际长度，遇到\0即停止；sizeof 计算整个字符串所占内存字节数的大小，当然\0也要+1计算；</strong></p>
<p>下面我们分析结果：</p>
<ol>
<li>首先p的结果大家应该不会错吧? p=hello，strlen=5，sizeof=6</li>
</ol>
<blockquote>
<p>字符串hello的长度是5，但在字符串最后隐藏一个\0故sizeof是6。</p>
</blockquote>
<ol start="2">
<li>p1 的结果：p1=hello，strlen=5，sizeof=7</li>
</ol>
<blockquote>
<p>这里\0主动结束了字符串长度的计算，但隐藏的\0仍然有意义，故此时字符串中存在两个\0。</p>
</blockquote>
<ol start="3">
<li>p2 的结果：p2=hello\0，strlen=7，sizeof=8</li>
</ol>
<blockquote>
<p>这里的\转义了\0，使得\0不再具有原来停止计算长度的作用而只是两个普通字符\和0。</p>
</blockquote>
<ol start="5">
<li>p3 的结果：p3=hello\，strlen=6，sizeof=8</li>
</ol>
<blockquote>
<p>这里第一个\只转义了第二个\，使得第二个\不能再转义后面的\0，此时\0仍然有结束的作用</p>
</blockquote>
<ol start="6">
<li>p4 的结果：p4=hel，strlen=3，sizeof=7</li>
</ol>
<blockquote>
<p>strlen碰到\0停止输出和计算，所以结果是3。但sizeof不会停止而且两个\0都计算。</p>
</blockquote>
<ol start="7">
<li>p5 的结果：p5=hel\0lo，strlen=7，sizeof=8</li>
</ol>
<blockquote>
<p>第一个\转义了紧接的\0变成两个普通字符\和0，sizeof比strlen多计算一个\0。</p>
</blockquote>
<p>下面是运行的结果：<br>
<img src="https://img-blog.csdnimg.cn/20190423215341527.png" alt="Tk=,size_16,color_FFFFFF,t_70)"></p>
<p>加油吧！！！</p>
                                    </div>