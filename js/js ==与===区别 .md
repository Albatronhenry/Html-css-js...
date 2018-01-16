### js ==与===区别 


快速区别不同:

--------

```javascript
    //全等===和相等==的区别
    console.log(100 === '100');//false
    console.log(100 == '100');//true
    //总结是===类型不同也会返回false ，==类型不同，值相同返回true
    
```


下面是详细分析:

-----------

<span class="RichText CopyrightRichText-richText" itemprop="text">"==="叫做严格运算符，"=="叫做相等运算符。<br><br>严格运算符的运算规则如下，<br>(1)不同类型值<br>如果两个值的类型不同，直接返回false。<br>(2)同一类的原始类型值<br><p>同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回true，值不同就返回false。</p><p>(3)同一类的复合类型值</p><p>两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个对象。</p><p>(4)undefined和null</p><p>undefined 和 null 与自身严格相等。</p><div class="highlight"><pre><code class="language-js"><span></span><span class="kc">null</span> <span class="o">===</span> <span class="kc">null</span>  <span class="c1">//true</span>
<span class="kc">undefined</span> <span class="o">===</span> <span class="kc">undefined</span>  <span class="c1">//true</span>
</code></pre></div><br><p>相等运算符在比较相同类型的数据时，与严格相等运算符完全一样。</p><p>在比较不同类型的数据时，相等运算符会先将数据进行类型转换，然后再用严格相等运算符比较。类型转换规则如下：</p><p>(1)原始类型的值</p><p>原始类型的数据会转换成数值类型再进行比较。<b>字符串和布尔值都会转换成数值，所以题主的问题中会有第二个string输出。</b></p><p>(2)对象与原始类型值比较</p><p>对象（这里指广义的对象，包括数值和函数）与原始类型的值比较时，对象转化成原始类型的值，再进行比较。</p><p>(3)undefined和null</p><p>undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。</p><p>(4)相等运算符的缺点</p><p>相等运算符隐藏的类型转换，会带来一些违反直觉的结果。</p><div class="highlight"><pre><code class="language-js"><span></span><span class="s1">''</span> <span class="o">==</span> <span class="s1">'0'</span>           <span class="c1">// false</span>
<span class="mi">0</span> <span class="o">==</span> <span class="s1">''</span>             <span class="c1">// true</span>
<span class="mi">0</span> <span class="o">==</span> <span class="s1">'0'</span>            <span class="c1">// true</span>

<span class="kc">false</span> <span class="o">==</span> <span class="s1">'false'</span>    <span class="c1">// false</span>
<span class="kc">false</span> <span class="o">==</span> <span class="s1">'0'</span>        <span class="c1">// true</span>

<span class="kc">false</span> <span class="o">==</span> <span class="kc">undefined</span>  <span class="c1">// false</span>
<span class="kc">false</span> <span class="o">==</span> <span class="kc">null</span>       <span class="c1">// false</span>
<span class="kc">null</span> <span class="o">==</span> <span class="kc">undefined</span>   <span class="c1">// true</span>

<span class="s1">' \t\r\n '</span> <span class="o">==</span> <span class="mi">0</span>     <span class="c1">// true</span>
</code></pre></div>这就是为什么<b>建议尽量不要使用相等运算符</b>。<br><br>至于使用相等运算符会不会对后续代码造成意外影响，答案是有可能会。<br><div class="highlight"><pre><code class="language-js"><span></span><span class="kd">var</span> <span class="nx">a</span> <span class="o">=</span> <span class="kc">undefined</span><span class="p">;</span>
<span class="k">if</span><span class="p">(</span><span class="o">!</span><span class="nx">a</span><span class="p">){</span>
    <span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s2">"1"</span><span class="p">);</span> <span class="c1">//1</span>
<span class="p">}</span>
</code></pre></div><div class="highlight"><pre><code class="language-js"><span></span><span class="kd">var</span> <span class="nx">a</span> <span class="o">=</span> <span class="kc">undefined</span><span class="p">;</span>
<span class="k">if</span><span class="p">(</span><span class="nx">a</span> <span class="o">==</span> <span class="kc">null</span><span class="p">){</span>
    <span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s2">"1"</span><span class="p">);</span> <span class="c1">//1</span>
<span class="p">}</span>
</code></pre></div><div class="highlight"><pre><code class="language-js"><span></span><span class="kd">var</span> <span class="nx">a</span> <span class="o">=</span> <span class="kc">undefined</span><span class="p">;</span>
<span class="k">if</span><span class="p">(</span><span class="nx">a</span> <span class="o">===</span> <span class="kc">null</span><span class="p">){</span>
    <span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s2">"1"</span><span class="p">);</span> <span class="c1">//无输出</span>
<span class="p">}</span>
</code></pre></div>也就是说当a为undefined时，输出的值会有变化，而在编程中对象变成undefined实在是太常见了。
