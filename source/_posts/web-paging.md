---
title: web pagin
date: 2018.3.14 10:36:45
reward: false
tags: 
    - web
    - paging
    - jsp

---

## 代码如下

``` bash

<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<div>
        <font style="float: left;margin-left: 20px;">共${personNum}条&nbsp;&nbsp;&nbsp;第${currentPage}页/共${pageTimes}页</font>
        <div id="div" style="float: left;margin-left: 500px;">
            <c:if test="${paging=='search'}">
                <c:if test="${currentPage==1}">
                    <span class="current">首页</span>
                </c:if>
                <c:if test="${currentPage!=1}">
                    <a href="LinkMessage?page=1&mName=${Na}">首页</a>
                </c:if>
                <c:if test="${currentPage==1}">
                    <span class="disabled">上一页 </span>
                </c:if>
                <c:if test="${currentPage !=1}">
                    <a href="LinkMessage?page=${currentPage-1}&mName=${Na}">上一页</a>
                </c:if>
                <c:if test="${currentPage==1}">
                    <span class="current">1</span>
                </c:if>
                <c:if test="${currentPage!=1}">
                    <a href="LinkMessage?page=1&mName=${Na}">1</a>
                </c:if>
                <%
                    int pageTimes=(Integer)request.getAttribute("pageTimes");
                    for(int i=1;i<pageTimes;i++){
                        request.setAttribute("page", i+1);
                %>
                <c:if test="${currentPage == page}">
                    <span class="current"><%= i+1 %></span>
                </c:if>
                <c:if test="${currentPage != page}">
                    <a href="LinkMessage?page=<%= i+1 %>&mName=${Na}"><%= i+1 %></a>
                </c:if>
                <% } %>
                <c:if test="${currentPage == pageTimes}">
                    <span class="disabled">下一页</span>
                </c:if>
                <c:if test="${currentPage != pageTimes}">
                    <a href="LinkMessage?page=${currentPage+1}&mName=${Na}">下一页</a>
                </c:if>
                <c:if test="${currentPage == pageTimes}">
                    <span class="disabled">尾页</span>
                </c:if>
                <c:if test="${currentPage != pageTimes}">
                    <a href="LinkMessage?page=${pageTimes}&mName=${Na}">尾页</a>
                </c:if>
            </c:if>
            <c:if test="${paging=='paging'}">
                <c:if test="${currentPage==1}">
                    <span class="current">首页</span>
                </c:if>
                <c:if test="${currentPage!=1}">
                    <a href="Message?page=1">首页</a>
                </c:if>
                <c:if test="${currentPage==1}">
                    <span class="disabled">上一页 </span>
                </c:if>
                <c:if test="${currentPage !=1}">
                    <a href="Message?page=${currentPage-1}">上一页</a>
                </c:if>
                <c:if test="${currentPage==1}">
                    <span class="current">1</span>
                </c:if>
                <c:if test="${currentPage!=1}">
                    <a href="Message?page=1">1</a>
                </c:if>
                <%
                    int pageTimes=(Integer)request.getAttribute("pageTimes");
                    for(int i=1;i<pageTimes;i++){
                    request.setAttribute("page", i+1);
                %>
                <c:if test="${currentPage == page}">
                    <span class="current"><%= i+1 %></span>
                </c:if>
                <c:if test="${currentPage != page}">
                    <a href="Message?page=<%= i+1 %>"><%= i+1 %></a>
                </c:if>
                <% } %>
                <c:if test="${currentPage == pageTimes}">
                    <span class="disabled">下一页</span>
                </c:if>
                <c:if test="${currentPage != pageTimes}">
                    <a href="Message?page=${currentPage+1}">下一页</a>
                </c:if>
                <c:if test="${currentPage == pageTimes}">
                    <span class="disabled">尾页</span>
                </c:if>
                <c:if test="${currentPage != pageTimes}">
                    <a href="Message?page=${pageTimes}">尾页</a>
                </c:if>
            </c:if>
        </div>
</div>

分页的action写法
// 获得新闻类型
    @RequestMapping("/GetTypeNewsInfo")
    public String getTypeNews(Model model, @RequestParam(value = "nId") Integer nId, String page) {
        int pageSize = 10;
        List<News> news = userService.getTypeNews(nId);
        // 查到的总用户数
        model.addAttribute("personNum", news.size());
        // 总页数
        int pageTimes;
        if (news.size() == 0) {
            pageTimes = 1;
            } else if (news.size() % pageSize == 0) {
            pageTimes = news.size() / pageSize;
            } else {
            pageTimes = news.size() / pageSize + 1;
            }
        model.addAttribute("pageTimes", pageTimes);
        // 页面初始的时候page没值
        if (null == page) {
            page = "1";
            }
        // 每页总第几条记录开始
        int startRow = (Integer.parseInt(page) - 1) * pageSize;
        news = this.userService.getTypeNewsPage(nId, startRow, pageSize);
        model.addAttribute("currentPage", Integer.parseInt(page));
        model.addAttribute("paging", "paging");
        model.addAttribute("nId", nId);
        model.addAttribute("News", news);
        return "back/sub/ByIdNews.jsp";
    }

```
