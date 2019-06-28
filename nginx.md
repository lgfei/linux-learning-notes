[nginx location proxy_pass 后面的url 加与不加/的区别](https://yq.aliyun.com/articles/506996?spm=5176.10695662.1996646101.searchclickresult.411f490dl0ZSc0)  
在nginx中配置proxy_pass时，当在后面的url加上了/，相当于是绝对根路径，则nginx不会把location中匹配的路径部分代理走;如果没有/，则会把匹配的路径部分也给代理走。 
<hr/> 
下面四种情况分别用http://192.168.1.4/proxy/test.html 进行访问。  
第一种：
<pre>
<code>
location  /proxy/ {
    proxy_pass http://127.0.0.1:81/;
}
</code>
</pre>
结论：会被代理到http://127.0.0.1:81/test.html 这个url
<hr/>
第二种(相对于第一种，最后少一个 /)
<pre>
<code>
location  /proxy/ {
    proxy_pass http://127.0.0.1:81;
}
</code>
</pre>
结论：会被代理到http://127.0.0.1:81/proxy/test.html 这个url
<hr/>
第三种：
<pre>
<code>
location  /proxy/ {
    proxy_pass http://127.0.0.1:81/ftlynx/;
}
</code>
</pre>
结论：会被代理到http://127.0.0.1:81/ftlynx/test.html 这个url。
<hr/>
第四种(相对于第三种，最后少一个 / )：
<pre>
<code>
location  /proxy/ {
    proxy_pass http://127.0.0.1:81/ftlynx;
}
</code>
</pre>
结论：会被代理到http://127.0.0.1:81/ftlynxtest.html 这个url
