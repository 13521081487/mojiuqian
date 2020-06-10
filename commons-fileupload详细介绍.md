> commons-fileupload详细介绍
```xml
<!--文件上传工具类 -->
<dependency>
	<groupId>commons-fileupload</groupId>
	<artifactId>commons-fileupload</artifactId>
	<version>1.3.3</version>
</dependency>
```
##### 详解
> 1.文件上传对form表单的要求
```txt
①form表单的method属性必须是&nbsp;post&nbsp;
②form表单的enctype属性必须是&nbsp;multipart/form-data&nbsp;类型的
&emsp;enctype默认值 : application/x-www-form-urlencoded
```
> 2.三个重要的类[DiskFileUpload,FileItem,FileUploadException],简单说明,后续深入了解之后再做补充
######DiskFileUpload类
- DiskFileUpload类是Apache文件上传组件的核心类，应用程序开发人员通过这个类来与Apache文件上传组件进行交互
- FileItem类用来封装单个表单字段元素的数据，一个表单字段元素对应一个FileItem对象，通过调用FileItem对象的方法可以获得相关表单字段元素的数据。
> 代码说明
```java
//创建文件解析对象
DiskFileUpload diskFileUpload = new DiskFileUpload();
//进行文件解析后放在list中,因为这个类库支持多个文件上传,因此把结果会存在List中
List<FileItem> list = diskFileUpload.parseRequest(request);
//获取上传文件,进行分析(不是必须)
File remoteFile = new File(new String(fileItem.getName().getBytes(),"UTF-8"));
//创建新对象,进行流拷贝
file1 = new File(this.getServletContext().getRealPath("attachment"),remoteFile.getName());
file1.getParentFile().mkdirs();
file1.createNewFile();
InputStream ins = fileItem.getInputStream();
OutputStream ous = new FileOutputStream(file1);
try{
	byte[] buffer = new byte[1024];
	int len = 0;
	while((len = ins.read(buffer)) > -1){
		ous.write(buffer,0,len);
	}
	out.println("以保存文件"+file1.getAbsolutePath()+"<br/>");
}
finally{
	ous.close();
	ins.close();
}
//本段代码只做参考,不做实际应用
```
