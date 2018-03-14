---
title: spring mvc image uploading
date: 2018.3.14 10:50:45
tags: 
    - spring mvc
    - imgae uploading
---

### upload.jsp

``` bash

<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form" %> 
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Insert title here</title>
    </head>
    <body>
        <form action="addAction" method="post" enctype="multipart/form-data">
            <input type="text" name="photoid"/>
            <input type="file" name="filename"/>
            <input type="submit" value="提交"/>
        </form>
    </body>
</html>

```

### java处理代码

``` bash
// 新闻图片上传处理get请求 添加上传页面
    @RequestMapping(value = "/AddPicture", method = { RequestMethod.GET })
    public String addPicture(Model model) {
        List<TypeNews> typeNews = userService.QueryTypeNews();
        NewsPhoto newsPhoto = new NewsPhoto();
        model.addAttribute("NewsPhoto", newsPhoto);
        model.addAttribute("TypeNews", typeNews);
        return "back/sub/uploadPicture.jsp";
    }
     // 新闻图片上传处理post请求 添加上传页面
    @RequestMapping(value = "/AddPicture", method = { RequestMethod.POST })
    public String addPicture(Model model, NewsPhoto newsPhoto, @RequestParam(value = "Newsphotos") MultipartFile file) {
        Resource res = new ServletContextResource(context, "/yfrxhPicture");  //获得项目路径
        File picRootFile = null;
        try {
            picRootFile = res.getFile();  //查询项目中的文件名称
        } catch (IOException e1) {
            return "back/fail/fail_addPhotoNews.jsp";
            }
        if (!picRootFile.exists()) {
            picRootFile.mkdirs();
        }
        try {
            InputStream inStream = file.getInputStream(); // 以流的形式获得图片
            // 虚拟文件路径

            Date date = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
            String picFolderPath = sdf.format(date); // 以日期为文件夹
            File picFolderFile = new File(picRootFile, picFolderPath);

            if (!picFolderFile.exists()) {
                picFolderFile.mkdirs();
            }
            // pic 图片名字 
            String picName = date.getTime() + ".jpg";
            File picFile = new File(picFolderFile, picName);
            FileOutputStream fs = new FileOutputStream(picFile);	
            byte[] buffer = new byte[1024 * 1024];
            int bytesum = 0;
            int byteread = 0;
            while ((byteread = inStream.read(buffer)) != -1) {
                bytesum += byteread;
                fs.write(buffer, 0, byteread);
                fs.flush();
            }
            String path="/yfrxh/img/"+picFolderPath+"/"+picName;   //该路径在spring-mvg.xml中存在映射
            newsPhoto.setNewsPhoto(path);
            fs.close();
            inStream.close();
        } catch (Exception e) {
            return "back/fail/fail_addPhotoNews.jsp";
        }
        int data=userService.insertNewsPhoto(newsPhoto);
        if(data>0){
            return "redirect:/newsController/TypePhoto?nId="+newsPhoto.getTid();
        }
        return "back/fail/fail_addPhotoNews.jsp";
    }

```

### xml配置

``` bash

    <mvc:resources location="/yfrxhPicture/" mapping="/img/**"/>  //静态资源的映射   若在spring mvc 中访问静态资源时，则需要配置

       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
            <property name="maxUploadSize">
                <value>10485760000</value>
            </property>
            <property name="maxInMemorySize">
                <value>40960</value>
            </property>
            <property name="defaultEncoding">
                <value>UTF-8</value>
           </property>
        </bean>

```