---
layout: post
title: '.net中使用Ajax做到局部刷新'
date: 2007-01-13 18:43
comments: true
tags: ['ajax','.net','server']
---
```html
<atlas:ScriptManager  ID  = "noRefresh"  EnablePartialRendering  ="true"  runat="server"></  atlas:ScriptManager  >  

&nbsp;&nbsp;  
<atlas:UpdatePanel  ID  = "noRefresh1"  Mode  ="Always"  RenderMode  ="Block" runat  ="server"  >  
    <ContentTemplate  >  
    <%  ...  \--  想要局部刷新的控件  \--  %>  
    </ContentTemplate  >  
</atlas:UpdatePanel  >

```

这就可以使其中的控件刷新了。

