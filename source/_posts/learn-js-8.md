---
layout: post
title: '8.javascript－－－第4天源代码'
date: 2006-07-30 22:20
comments: true
tags: ['javascript','imagelist','html']
categories:
- [code]
- [learn, javascript]
---

Jex16.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">

<HTML>

<HEAD>
    <TITLE> Pictures Test3</TITLE>
</HEAD>
<script language="javascript">
    var imageList = new Array;
    for (var i = 0; i < 5; i++) {
        imageList[i] = new Image;
        imageList[i].src = "winxpaa1" + i + ".jpg";
    }

    function slide(num) {
        document.slidePicture.src = imageList[num].src;
        num = Math.floor(Math.random() * imageList.length);
        //don't worry about run out of the array.  
        window.setTimeout("slide(" + num + ")", 1000);
    }

    function testPicture() {
        var result = true;
        for (var i = 0; i < imageList.length; i++)
            result = (result && imageList[i].complete);
        if (result) {
            slide(0);
        } else
            window.setTimeout("testPicture()", 1000);
    }
</script>

<BODY onload="testPicture()">
    <img src=winxpaa10.jpg name="slidePicture">

    </form>
</BODY>

</HTML>
```

Jex17.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>

<HEAD>
    <TITLE> New Document </TITLE>

</HEAD>

<BODY>
    <script language="javascript">
        var defaultImage = new Image;
        defaultImage.src = "winxpaa10.jpg";
        var rolledImage = new Image;
        rolledImage.src = "winxpaa11.jpg";
        var url = " [ http://www.baidu.com/ ](http://www.baidu.com/) ";
    </script>
    <!--<a href = " [ http://www.baidu.com/">baidu</a
](http://www.baidu.com/">baidu</a) >  
<br>  
<a href = "#" onMouseOver = "document.A.src = rolledImage.src" onMouseOut =
"document.A.src = defaultImage.src">rolled</a>  
<br>\-->
    <br>
    <br>
    <map name="Imagemap">
        <area shape="rect" coords="0,0,100,200" onMouseOver="document.A.src =
rolledImage.src;document.B.src = rolledImage.src" onMouseOut="document.A.src
= defaultImage.src;document.B.src = defaultImage.src">
    </map>
    <br>
    <br>
    <img src="winxpaa10.jpg" name="A" width=100 height=100 useMap="#Imagemap">
    <img src="winxpaa11.jpg" name="B" width=100 height=100 useMap="#Imagemap">
    <img src="winxpaa13.jpg" name="C" width=100 height=100 useMap="#Imagemap">
</BODY>

</HTML>
```

Jex18.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>

<HEAD>
    <TITLE> New Document </TITLE>

</HEAD>

<BODY>
    <form name="A">
        Enter you code:
        <input type="text" onChange="window.alert(this.form.txt.value)" name="txt">
        <br>
        <!-- for test  "this"  
<input type = "text" onChange = "window.alert(this.form.text.value)" name =
"tddt">  
<br>  
<input type = "text" onChange = "window.alert(this.A.tet.value)" name = "tet">  
\-->
        <select name="choose" onChange="changeTwo(this.form.choose)">
            <option value="One">1</option>
            <option value="Two">2</option>
        </select>
        <a href="#" onClick="alert(document.A.choose.value)">ddd</a>
        <br>
        <a href="#" onClick="change(document.A.choose)">change</a>
        <script language="javascript">
            document.A.choose.length++;
            document.A.choose.options[document.A.choose.length - 1].text = "3";
            document.A.choose.options[document.A.choose.length - 1].value = "ddd";

            function change(list) {
                for (var i = 0; i < list.length; i++) {
                    list.options[i].text = i + 3;
                    list.options[i].value = i;
                }
            }

            function changeTwo(list) {
                if (list.selectedIndex == 1) {
                    change(list);
                }
            }
        </script>
    </form>
</BODY>

</HTML>
```

Jex19.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>

<HEAD>
    <TITLE> New Document </TITLE>

</HEAD>

<BODY>
    <input type="Radio" name="A" value="F">yse<br>
    <input type="Radio" name="A" value="Y" Checked>yfe<br>
    <input type="Radio" name="A" value="F">ye<br>
    <input type="Radio" name="B" value="Y">ye<br>
    <input type="Radio" name="B" value="Y">ye<br>
</BODY>

</HTML>
Jex20.html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>

<HEAD>
    <TITLE> New Document </TITLE>
</HEAD>

<BODY onLoad="first(document.myForm.secondSelect);">
    <form name="myForm" onClick="first(document.myForm.firstSelect)">
        <select name="firstSelect" onChange="upDate(this.form);">
            <option value="1">First Choice</option>
            <option value="2">Second Choice</option>
        </select>
        <br>
        <select name="secondSelect">
        </select>
    </form>
    <script language="javascript">
        function first(list) {
            list.length = 3;
            for (var i = 0; i < list.length; i++) {
                list.options[i].text = "first" + i + 3;
                list.options[i].value = i;
            }
        }

        function second(list) {
            list.length = 3;
            for (var i = 0; i < list.length; i++) {
                list.options[i].text = "second" + (i + 3);
                list.options[i].value = i;
            }
        }

        function upDate(thisForm) {
            if (thisForm.firstSelect.selectedIndex == 0) {
                first(thisForm.secondSelect);
            } else {
                second(thisForm.secondSelect);
            }
        }
    </script>

</BODY>

</HTML>  

```

