---
title: java recursion
date: 2018.07.12 14:35:34
tags:
  - java
  - json
---

### 项目目标

&nbsp;&nbsp;&nbsp;&nbsp;一个字符串的内容转换成json对象，并根据json中的内容寻找到从起点到终点的可连通的路径

### 字符串内容

``` bash
"[
  {id:-1, preNode:[], nextNode:[1], branch:null},
  {id:1, preNode:[-1], nextNode:[2], branch:null},
  {id:2, preNode:[1], nextNode:[3], branch:null},
  {id:3, preNode:[2], nextNode:[4,5], branch:oneofonestart},
  {id:4, preNode:[3], nextNode:[6], branch:null},
  {id:5, preNode:[3], nextNode:[7], branch:null},
  {id:6, preNode:[4], nextNode:[8], branch:null},
  {id:7, preNode:[5], nextNode:[9], branch:null},
  {id:8, preNode:[6], nextNode:[10], branch:null},
  {id:9, preNode:[7], nextNode:[10], branch:oneofend},
  {id:10, preNode:[8,9], nextNode:[11], branch:null},
  {id:11, preNode:[10], nextNode:[12,13,14], branch:oneofonestart},
  {id:12, preNode:[11], nextNode:[15], branch:null},
  {id:13, preNode:[11], nextNode:[16], branch:null},
  {id:14, preNode:[11], nextNode:[17], branch:null},
  {id:15, preNode:[12], nextNode:[-2], branch:null},
  {id:16, preNode:[13], nextNode:[-2], branch:null},
  {id:17, preNode:[14], nextNode:[-2], branch:null},
  {id:-2, preNode:[15,16,17], nextNode:[], branch:oneofoneend}
]"

```

### 字符串所表达的图



### 新建项目所依赖的主要包

```code

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.9.6</version>
</dependency>

<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.9.6</version>
</dependency>

```

### 创建实体类

```code

package com.yf.af.junitTest;

public class Node {
    private int id;
    private int preNode[];
    private int nextNode[];
    private String branch;

    private boolean disable = false;

    public Node(int id, int[] preNode, int[] nextNode, String branch) {
        this.id = id;
        this.preNode = preNode;
        this.nextNode = nextNode;
        this.branch = branch;
    }

    public boolean isDisable() {
        return disable;
    }

    public void setDisable(boolean disable) {
        this.disable = disable;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int[] getPreNode() {
        return preNode;
    }

    public void setPreNode(int[] preNode) {
        this.preNode = preNode;
    }

    public int[] getNextNode() {
        return nextNode;
    }

    public void setNextNode(int[] nextNode) {
        this.nextNode = nextNode;
    }

    public String getBranch() {
        return branch;
    }

    public void setBranch(String branch) {
        this.branch = branch;
    }

    public Node() {
    }
}

```

### 具体实现java代码

```
package com.yf.af.junitTest;


import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.util.*;
import java.util.Stack;

public class JsonTest {

    private List<Node> nodes;
    private Map<String, List<Node>> paths;

    public static void main(String[] args) {
        String nodeStr = "[\n" +
                "    {\"id\":-1, \"preNode\":[], \"nextNode\":[1], \"branch\":null},{\"id\":1, \"preNode\":[-1], \"nextNode\":[2], \"branch\":null},{\"id\":2, \"preNode\":[1], \"nextNode\":[3], \"branch\":null}," +
                "    {\"id\":3, \"preNode\":[2], \"nextNode\":[4,5], \"branch\":\"oneofonestart\"},{\"id\":4, \"preNode\":[3], \"nextNode\":[6], \"branch\":null},{\"id\":5, \"preNode\":[3], \"nextNode\":[7], \"branch\":null}," +
                "    {\"id\":6, \"preNode\":[4], \"nextNode\":[8], \"branch\":null},{\"id\":7, \"preNode\":[5], \"nextNode\":[9], \"branch\":null},{\"id\":8, \"preNode\":[6], \"nextNode\":[10], \"branch\":null}," +
                "    {\"id\":9, \"preNode\":[7], \"nextNode\":[10], \"branch\":\"oneofend\"},{\"id\":10, \"preNode\":[8,9], \"nextNode\":[11], \"branch\":null},{\"id\":11, \"preNode\":[10], \"nextNode\":[12,13,14], \"branch\":\"oneofonestart\"}," +
                "    {\"id\":12, \"preNode\":[11], \"nextNode\":[15], \"branch\":null},{\"id\":13, \"preNode\":[11], \"nextNode\":[16], \"branch\":null},{\"id\":14, \"preNode\":[11], \"nextNode\":[17], \"branch\":null}," +
                "    {\"id\":15, \"preNode\":[12], \"nextNode\":[-2], \"branch\":null},{\"id\":16, \"preNode\":[13], \"nextNode\":[-2], \"branch\":null},{\"id\":17, \"preNode\":[14], \"nextNode\":[-2], \"branch\":null}," +
                "    {\"id\":-2, \"preNode\":[15,16,17], \"nextNode\":[], \"branch\":\"oneofoneend\"}" +
                "]";

        JsonTest jsonTest = new JsonTest();

        //用于保存所有的node
        List<Node> nodes = new ArrayList<>();

        try {
            //将字符串转化成list<Node>
            nodes = new ObjectMapper().readValue(nodeStr, new TypeReference<List<Node>>() {
            });
        } catch (IOException e) {
            e.printStackTrace();
        }

        nodes.forEach(node -> {
            mappedNodes.put(node.getId(), node);
        });

        jsonTest.findPath(nodes.get(0));

        for (int i = 0; i < jsonTest.getPaths().size(); i++) {
            System.out.println(jsonTest.getPaths().get(i));
        }
    }

    // mapped nodes
    static Map<Integer, Node> mappedNodes = new HashMap<>();

    Stack<Integer> path = new Stack<>();

    Stack<Integer> branchs = new Stack<>();



    //找出所有的完整的路径
    public void findPath(Node node) {

        path.push(node.getId());

        //
        if (node.getId() == -2) {
            System.out.println(path.toString());

            System.out.println(branchs.toString());

            System.out.println("found path");
        }

        //
        int[] branches = node.getNextNode();
        for (int i = 0; i < branches.length; i++) {
            Node next = mappedNodes.get(branches[i]);

            findPath(next);

            //
            System.out.println("return from findPath");
        }

        path.pop();
    }


    public Map<String, List<Node>> getPaths() {
        return paths;
    }

    public void setPaths(String key, List<Node> nodes) {
        if (this.paths == null) {
            this.paths = new HashMap<>();
        }
        this.paths.put(key, nodes);
    }
}

```

### 具体实现kotlin代码

```
    //返回字符串
    @Bean
//    @Order(value=1)
    fun nodesString(): String {

        return "[" +
                "{\"id\":-1, \"prev\":[], \"next\":[1], \"branch\":null,\"expr\":null,\"index\":null},"+
                "{\"id\":1, \"prev\":[-1], \"next\":[2], \"branch\":null,\"expr\":\"i21 < 8 && i21>=2\",\"index\":21},"+
                "{\"id\":2, \"prev\":[1], \"next\":[3], \"branch\":null,\"expr\":\"i22==1\",\"index\":22}," +
                "{\"id\":3, \"prev\":[2], \"next\":[4,5], \"branch\":\"oneofonestart\",\"expr\":\"i23>130 && i23 <200\",\"index\":23}," +
                "{\"id\":4, \"prev\":[3], \"next\":[6], \"branch\":null,\"expr\":\"i24 > 300\",\"index\":24}," +
                "{\"id\":5, \"prev\":[3], \"next\":[7], \"branch\":null,\"expr\":\"i25 == 1\",\"index\":25}," +
                "{\"id\":6, \"prev\":[4], \"next\":[8], \"branch\":null,\"expr\":\"i26 == 1\",\"index\":26}," +
                "{\"id\":7, \"prev\":[5], \"next\":[9], \"branch\":null,\"expr\":\"i27 > 66\",\"index\":27}," +
                "{\"id\":8, \"prev\":[6], \"next\":[10], \"branch\":null,\"expr\":\"i28 == 1\",\"index\":28}," +
                "{\"id\":9, \"prev\":[7], \"next\":[10], \"branch\":\"oneofend\",\"expr\":\"i29 == 0\",\"index\":29}," +
                "{\"id\":10, \"prev\":[8,9], \"next\":[11], \"branch\":null,\"expr\":\"i30 >=45\",\"index\":30}," +
                "{\"id\":11, \"prev\":[10], \"next\":[12,13,14], \"branch\":\"oneofonestart\",\"expr\":\"i31 >100\",\"index\":31}," +
                "{\"id\":12, \"prev\":[11], \"next\":[15], \"branch\":null,\"expr\":\"i32>256\",\"index\":32}," +
                "{\"id\":13, \"prev\":[11], \"next\":[16], \"branch\":null,\"expr\":\"i33 == 89 && i32 >356\",\"index\":33}," +
                "{\"id\":14, \"prev\":[11], \"next\":[17], \"branch\":null,\"expr\":\"i34 == 0\",\"index\":34}," +
                "{\"id\":15, \"prev\":[12], \"next\":[-2], \"branch\":null,\"expr\":\"i35 ==1\",\"index\":35}," +
                "{\"id\":16, \"prev\":[13], \"next\":[-2], \"branch\":null,\"expr\":\"i36 ==1 \",\"index\":36}," +
                "{\"id\":17, \"prev\":[14], \"next\":[-2], \"branch\":null,\"expr\":\"i37 >=47\",\"index\":37}," +
                "{\"id\":-2, \"prev\":[15,16,17], \"next\":[], \"branch\":\"oneofoneend\",\"expr\":null,\"index\":null}" +
                "]"
    }

    val nodeMap: MutableMap<Int, Node> = mutableMapOf()
    val path: Stack<Node> = Stack()

    //循环获得连通的路径
    fun findPath(node: Node) {
        path.push(node)

        if (node.id == -2) {
            print("{\"path\": [")
//            path.forEach { print("{\"id\": ${it.id}}, ") }
            path.forEach { print("${it.id}, ") }
            println("]}, ")
        }

        node.next.forEach {
            findPath(nodeMap[it]!!)
        }

        path.pop()
    }

//根据字符串内容转化成json对象，并且进行第一个递归调用
//    @Order(value=2)
    @Bean
    fun pathFind(nodeString: String): CommandLineRunner {
        return CommandLineRunner {
            val nodes = ObjectMapper().readValue(nodeString, Array<Node>::class.java)

            nodes.forEach { nodeMap.put(it.id!!, it) }

            val first: Node = nodeMap[-1]!!

            findPath(first)
        }
    }

```