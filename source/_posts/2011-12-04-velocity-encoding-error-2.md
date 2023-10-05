---
layout: post
title: "velocity encoding charset error"
date: 2011-12-4
wordpress_id: 514
comments: true
tags: ["charset", "encoding", "error", "exception", "java", "java", "see-hear", "velocity"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="There is a solution for solve velocity encoding or charset error." />
<meta name="_su_keywords" content="encoding, charset, error, velocity" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="encoding, charset, velocity, error, exception, java, web" />
<meta name="views" content="64" />
<meta name="_wp_old_slug" content="velocity-encoding-error" />
<p>   First, set some properties for velocity engine. Set vm file reading path. use file read-mode not resource read-mode.<br />
<br />
// set velocity path, if you don't do these, you will get Resource not found exception.<br />
Properties p = new Properties();<br />
p.setProperty("resource.loader", "file");<br />
p.setProperty("class.resource.loader.class","org.apache.velocity.runtime.resource.loader.FileResourceLoader");<br />
// where is your vms path, set it<br />
p.setProperty("file.resource.loader.path", realPath + "/vm");<br />
// set output encoding<br />
// nearly almost articals said the following will work, but it won't<br />
p.setProperty(Velocity.INPUT_ENCODING, Constant.DEFAULT_ENCODING);<br />
p.setProperty(Velocity.OUTPUT_ENCODING, Constant.DEFAULT_ENCODING);<br />
p.setProperty("contentType", "text/html;charset="+Constant.DEFAULT_ENCODING);<br />
Velocity.init(p);<br />
<br />
   The above code should be write in beginning of use any of velocity objects. In Spring or Java web application, you should write them into Servlet context listener.<br />
   If you use ascii word. There will no error encoding. But if you use gb2312 or UTF-8. You will find something weird. The above code dose not work. So I use response output to solve those encoding problem. In web application, we always use response.getOutputStream() to write something back. But now, we should construct output stream to write correct encoding word to the page. So velocity can encoding them correctly. The following code will help you.<br />
<br />
String encoding = "utf-8";<br />
// If you use UTF-8 encoding in your application<br />
Writer writer = new PrintWriter(new PrintStream(resp.getOutputStream(), true, encoding));<br />
// velocity will find abc.vm in your file path folder.<br />
String pageName = "abc";<br />
Template template = Velocity.getTemplate(pageName);<br />
VelocityContext context = new VelocityContext();<br />
context.put("test", new String("hello".getBytes(encoding), encoding));<br />
// here velocity will use the writer to write correct encoding words to response stream. So we can see the correct words showed by our page.<br />
template.merge(context, writer);<br />
<br />
   If you use jetty to run your web application, it works fine.<br />
   But maybe you use windows resin to run war package. Sometimes, error encoding comes again. There is something wrong with your resin application. You should change config file in resin conf folder. In conf/resin.xml( resin 4.0+ ), add &lt;character-encoding&gt;UTF-8&lt;/character-encoding&gt;  in  &lt; web-app-default &gt;. Like following:<br />
<blockquote>
&lt; web-app-default &gt;<br />
    &lt;character-encoding&gt;UTF-8&lt;/character-encoding&gt;<br />
...<br />
&lt; /web-app-default &gt;<br />
</blockquote>
   And you should use correct charset to start your resin. Else resin will use default charset of your operation system.</p>
<blockquote><p>java -Dfile.encoding=UTF-8 -jar lib/resin.jar </p></blockquote>
<p>   Addition, if you use spring framework, don't forget the encoding filter :<br />
<blockquote>
&lt;filter&gt;<br />
    &lt;filter-name&gt;characterEncodingFilter&lt;/filter-name&gt;<br />
        &lt;filter-class&gt;org.springframework.web.filter.CharacterEncodingFilter&lt;/filter-class&gt;<br />
        &lt;init-param&gt;<br />
            &lt;param-name&gt;encoding&lt;/param-name&gt;
            &lt;param-value&gt;UTF-8&lt;/param-value&gt;
        &lt;/init-param&gt;<br />
    &lt;/filter&gt;<br />
    &lt;filter-mapping&gt;<br />
        &lt;filter-name&gt;characterEncodingFilter&lt;/filter-name&gt;<br />
        &lt;url-pattern&gt;/*&lt;/url-pattern&gt;<br />
    &lt;/filter-mapping&gt;<br />
</blockquote>
   In encoding or charset error. You should consider the following :</p>
<li>  <b>environment (like resin, jetty, eclipse and etc.)</b></li>
<li>  <b>Input (from outside, like user input, or reading from database )</b></li>
<li>  <b>Output (to outside, like page presented, or writing to database or file)</b></li>
<li>  <b>Process (maybe you encode words twice or more, or encode them to some other different charset)</b></li>
