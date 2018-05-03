# sendEmail
# 发送自定义邮件

---
### 1.EmailBody：自定义邮件体类

* 类初始化：对邮件主题、收件人、附加文件和发件人初始化，并用MIMEMultipart函数创建邮件

* 添加标头：可以自定义标头大小、颜色和空行

		def attachHeader(self,header, size, color, newLine)

* 添加段落标题：可以自定义标题大小、颜色和空行

		def attachTitle(self,title, size, color, newLine)
	
* 添加文本：参数为str类型的文本

		def attachText(self,text)

* 添加图片：参数为图片的名字或绝对路径

		def attachPic(self,pic)

* 添加附件：参数为文件名称或绝对路径

		def attachFile(self, attachment)

* 添加markdown文本：参数为str类型的markdown文本

		def attachMd(self,md)

* 添加图片列表：参数为图片名字的列表

		def attachImageList(self,imgList)

* 添加一个段落：参数为段落文本

		def attachPara(self,text)

* 将整个正文转化为xml格式附加到邮件：

		def attachBody(self)
   
* 以str类型返回整个邮件：

		def msg_str(self)



---

### 2.buildEmail：创建邮件

* 参数：邮件主题、一段str文本、收件人列表、附件列表、发送人

		def buildEmail(subject, textStr, to_address_list=None, attach_file_name=None, author="WQ_ITRobot"):

* 返回值：返回实例化邮件体类后的一封邮件

* 功能：邮件体类实例化一封邮件，并可以自定义给邮件添加标头、段落标题、文本、图片、段落、附件，可根据需求添加，如果附件有图片文件，则将图片文件显示在正文结尾，不在附件列表中添加


		emailbody = EmailBody(subject, to_address_list, author)  #实例化EmailBody
    		emailbody.attachHeader("header", "20", 'blue', 0)        #给邮件添加标头，大小为20，颜色为蓝色
    		emailbody.attachTitle("title", "10", 'red', 0)           #添加标题
    		emailbody.attachPara(textStr)                            #添加段落
    		emailbody.attachPic("D:/pic1.jpg")                       #添加一副图片

    		emailbody.attachBody()                                   #将正文附加到整个邮件                 

    		input_file = codecs.open("D://cleanupDisk.md", mode="r", encoding="utf-8")
    		text = input_file.read()
    		emailbody.attachMd(text)                                 #添加一段markdown文本

		emailbody.attachImageList(imgList)                       #将附件里的图片显示在正文结尾



---

### 3.sendEmail：发送邮件
* 参数：邮件体，收件人列表

* 功能：连接服务器，登录邮箱，将邮件以str格式进行发送

		def sendEmail(emailbody,to_address_list):
    		smtp = smtplib.SMTP('smtp.exmail.qq.com')
    		smtp.login('**邮箱名称**', '**邮箱密码**')
    		try:
        		smtp.sendmail('**收件人**', to_address_list, emailbody.msg_str())
    		finally:
        		smtp.close()
