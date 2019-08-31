---
title: flutter
date: 2018.03.27 14:36:45
reward: false
tags:
    - flutter
    - image
    - install
---

## dart one array divide into three group 

```dart

import 'dart:async';

import 'package:async/async.dart';
import 'package:rxdart/rxdart.dart';
import 'package:test_api/test_api.dart';
import 'package:training/models/storing_data.dart';

main() {
  group('array_x_y_z', () {
    test('array_x_y_z_1', () {
      List list = List();

      List array = createArray(101);

      for (int i = 0; i < (array.length) / 3; i++) {
        int x = array[i];
        int y = array[i + 33];
        int z = array[i + 33 + 33];

        list.add({'x': x, 'y': y, 'z': z});
      }

      print(list.length);
    });

    test('array_x_y_z_2', () {
      List list = createArray(101);

      List result = cal(list, 5);

      list.map((f) {});
      assert(result.length == (list.length / 5).floor());

      assert(result[0].length == 5);

      assert(result[0][0] == list[0]);

      assert(result[10][4] == list[result.length * 4 + 10]);
    });

    // test("array_x_y_z_3", () {
    //   var array1 = array.getRange(0, 32);
    //   var array2 = array.getRange(33, 65);
    //   var array3 = array.getRange(66, 98);
  });

  test("array_x_y_z_3", () async {
    var firstCompleter = Completer();
    
    List array = createArray(101);
    // // 5
    StreamZip([
      Stream.fromIterable(array.getRange(0, 50)),
      Stream.fromIterable(array.getRange(50, 100)),
    ])
    .toList()
    .then(print)
    .whenComplete((){
      return firstCompleter.complete();
    })
    ;

    await firstCompleter.future;
    
  }, timeout: Timeout(Duration(seconds: 20)));
}

List createArray(int size) {
  return List<int>.generate(size, (int index) => index);
}

List cal(List array, int size) {
  // List<List> list = List();

  var length = (array.length / size).floor();

  return List<List>.generate(length,
      (index) => List<int>.generate(size, (c) => array[length * c + index]));

  // for (int j = 0; j < length; j++) {
  //   var result = [];

  //   for (int i = 0; i < size; ++i) {
  //     result.add(array[length * i + j]);
  //   }

  //   list.add(result);
  // }

  // return list;
}

Future future() async {
  return await Future.delayed(Duration(seconds: 1), () {
    return 1;
  });
}

```
