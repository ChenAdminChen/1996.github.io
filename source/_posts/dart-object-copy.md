---
title: dart-object-copy
date: 2019.06.04 15:36:45
reward: false
tags: 
    - dart
    - class
---

## dart object copy

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;现在创建一个普通的实体类，并且该类带有main方法，如下代码

``` bash
//copy class 
import 'dart:math';

void main() {
  
  Todo todo1= Todo("todo1",10);
    
  Todo todo2 = Todo.copy(todo1);
  
  print("todo1 == todo2 : ${todo1 == todo2}");
  
  todo2.todo = "fffff";
   
  print("todo2 tom todo1.copy: ${todo1 == todo2}");
  
}

class Todo{
  String todo;
  int priority;
  int hash;
  
  factory Todo.copy(Todo t){
    return new Todo(t.todo,t.priority,hash:t.hash);
  }
  
  Todo(this.todo,this.priority,{this.hash});
  
  bool operator == (o){
    return o is Todo && o.todo == todo && o.priority == priority;
  }
  
  int get hashCode => hash *100 ;
}


```
