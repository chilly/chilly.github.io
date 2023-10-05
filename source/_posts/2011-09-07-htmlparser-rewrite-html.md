---
layout: post
title: "htmlParser rewrite HTML"
date: 2011-9-7
wordpress_id: 500
comments: true
tags: ["api", "code", "html", "java", "java", "parser"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="194" />
<meta name="_su_description" content="use java htmpParser api to rewrite html TagNode TextNode Node" />
<meta name="_su_keywords" content="TagNode,TextNode,Node,html,parser" />
<meta name="_su_title" content="htmlParser rewrite HTML" />
Yesteday, I have tried many methods to rewrite HTML.
And find TagNode can not write TextNode, so if I want to replace "&lt;p&gt; id="hello"&gt;&lt;/p&gt;" to "&lt;p id="hello"&gt;it's me&lt;/p&gt;". There is no method to support. And in htmlParser, there is a method call setText(). But setText is only set text into&lt;p id="hello"&gt;, like "&lt;p id="hello" class="pp"&gt;". The method can not write text into"&lt;p&gt;&lt;/p&gt;". If you want to add a TextNode as TagNode child. HtmlParser can not support this idea also. Like:

NodeList nl = p.parse(null);
NodeVisitor nv = new NodeVisitor(){
    public void visitTag(Tag tag) {
        if (tag instanceof ParagraphTag) {
            String attribute = tag.getAttribute("id");
            if (attribute == null){
                return;
            }
            TextNode tn = new TextNode(tag.getText());
            tn.setText("sidafnaf ");
            if(tag.getAttribute("id").equals("hello")){
                Text t = new TextNode("abc");
                t.setParent(tag);
                // It does not work
                NodeList temp = new NodeList();
                temp.add(t);
                tag.setChildren(temp);
            } else if (tag.getAttribute("id").equals("author")){
                tag.setAttribute("innerHTML","id a");
            }
        } else if (tag instanceof LinkTag){
            if (tag.getAttribute("id").equals("come-from")){
                // can write an attribute innerHtml = "ok?"
                tag.setAttribute("innerHtml","ok?");
                // can rewrite URL attribute 
                tag.setAttribute("href", "http://www.google.com");
            }
        }
    }
};

Solve this problem, you should flag where you want to insert. like<strong>&lt;p&gt;messgae&lt;/p&gt;</strong>
Then use following code:

NodeList nl = p.parse(null);
NodeVisitor nv = new NodeVisitor(){
   public void visitStringNode(Text string){
          if(string.getText().equals("message")){
               string.setText("It's me");
          }
          // if you want change attributes of &lt;p&gt;    
          TagNode tn = string.getParent();
          tn.setAttribute("id","good!");
   }
}

Now it's done.
