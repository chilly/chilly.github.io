---
layout: post
title: "JAVA Mail API problem"
date: 2012-07-08 23:07
comments: true
sumary: java mail api base64编码不能执行。图片看不到，以及html被当作正文
tags: java, mail, email, API, problem, base64, mime, picture 
categories:
- knowledge
- java
---

 * text/html mime 没有base64 编码问题

java mail api 使用MimeBodyPart.setDataHandler()不能对text/html进行正常的base64编码而是使用quote-printable编码。 错误的样本：

```
// add headers 不能在最前面，一定要放在后面，否则可能不能正确编码
mimeBody.addHeader("Content-Type", "text/html; charset=utf-8");
mimeBody.addHeader("Content-Transfer-Encoding", "base64");
DataHandler textDataHandler = new DataHandler(new ByteArrayDataSource(
                message.getBytes("utf-8"), "text/html; charset=utf-8"));
mimeBody.setDataHandler(textDataHandler);
```

正确的样本：

```
DataHandler textDataHandler = new DataHandler(new ByteArrayDataSource(
                message.getBytes("utf-8"), "text/html; charset=utf-8"));
mimeBody.setDataHandler(textDataHandler);
// add headers 一定要放在后面，否则可能不能正确编码
mimeBody.addHeader("Content-Type", "text/html; charset=utf-8");
mimeBody.addHeader("Content-Transfer-Encoding", "base64");
```

 * 发送到新浪等邮箱，图片看不到

rfc2045-2047中并没有规定Content-ID的形式必须是: Content-ID:<cid>的形式。但是新浪的邮箱服务解析得比较严格。所以在JavaMail Api 中使用MimeBodyPart 对其进行setContentID时必须加上"<>"。否则图片会看不到。例如:   

```
MimeBodyPart tmpBodyPart = new MimeBodyPart();
tmpBodyPart.setContentID("<"+attachment.getFileid()+">");
```

 * html格式正文被163等邮箱当作附件，但是某些邮箱解析正常

这是因为html格式的正文并不是MultiPart中的MimeBodyPart，而MultiPart的第一个MimeBodyPart可能是附件。在添加html正文时可以这样写：

```
//将html正文放入到mimBodyPart中
String html="<html>xxxx</html>";
MimeBodyPart mimeBody = new MimeBodyPart();
mimeBody.setText(html, "utf-8");     
mimeBody.addHeader("Content-Type", "text/html; charset=utf-8");
mimeBody.addHeader("Content-Transfer-Encoding, "base64");

//创建mixed 和 related两种mime subType
Multipart mPartRelated = new MimeMultipart("related");
Multipart mPartMixed = new MimeMultipart("mixed");
mPartRelated.addBodyPart(mimeBody);
MimeBodyPart mimeBodyPart = new MimeBodyPart();
mimeBodyPart.setContent(mPartRelated);
// 设置为第一个 bodyPart,这样就不会再变为附件。
mPartMixed.addBodyPart(mimeBodyPart, 0);

// mPartMix就是最后要set到mimeMessage中的内容。
```

 * inline图片当作附件，或者inline图片在某些邮箱中显示了两次

有些是邮箱会将这个作为feature,但是有些不是。这时要使用这些邮箱来发一封类似的邮件来看到底是那个邮箱的独特功能还是你的bug. 如果是你的错误，那应该是你将inline图片和html直接包到mime/mixed中，其实应该放入到mime/related中的。 所以把这个错误改正，应该就ok.

 * 邮件中的文本，线上打开乱码

这是因为发送邮件的编码问题。文本的charset不要编码为utf-8，因为这样可能有非utf-8的文本放入到邮件中发送。所以要写二进制编码 Content-Type: application/octet-stream 然后使用base64编码。 这样大部分编码就解决了。但是qq等其他客户端会有问题，这是他们自己的问题.



