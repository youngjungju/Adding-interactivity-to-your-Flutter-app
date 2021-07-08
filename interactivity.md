Adding-interactivity-to-your-Flutter-app
===

![image](https://user-images.githubusercontent.com/70834005/124849629-15690200-dfda-11eb-81a9-afc10dfdecfb.png)

목표 : 탭을 통해 반응하는 앱 만들기 
---
![image](https://user-images.githubusercontent.com/70834005/124850627-f4091580-dfdb-11eb-934b-6541dc78eb2b.png)

Stateful and stateless widgets 
---

### Stateless Widget
<pre>
<code>
화면이 로드될 때 한 번만 그려지는 State가 없는 정적 위젯 
Ex) 히즈넷의 상단 바 중앙 로고 
</code>
</pre>

### Stateful Widget 
<pre>
<code>
위젯이 동작하는 동안 Data 변경이 필요한 경우 화면을 다시 그려서 변경된 부분을 위젯에 반영하는 동적 위젯 
Ex) 인스타그램 좋아요 버튼 
</code>
</pre>

0.Get ready
---
반응하는 앱을 만들기 전 Widget만  코드이다.
```java

import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    Column buildButtonColumn(IconData icon, String label) {
      Color color = Theme.of(context).primaryColor;
      return new Column(
        mainAxisSize: MainAxisSize.min,
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          new Icon(icon, color: color),
          new Container(
            margin: const EdgeInsets.only(top: 8.0),
            child: new Text(
              label,
              style: new TextStyle(
                fontSize: 12.0,
                fontWeight: FontWeight.w400,
                color: color,
              ),
            ),
          )
        ],
      );
    }

    Widget titleSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Row(
        children: [
          new Expanded(
            child: new Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                new Container(
                  padding: const EdgeInsets.only(bottom: 8.0),
                  child: new Text(
                    'Oeschinen Lake Campground',
                    style: new TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
                new Text(
                  'Kandersteg, Switzerland',
                  style: new TextStyle(color: Colors.grey[500]),
                ),
              ],
            ),
          ),
          new Icon(
            Icons.star,
            color: Colors.red[500],
          ),
          new Text('41'),
        ],
      ),
    );

    Widget buttonSection = new Container(
      child: new Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          buildButtonColumn(Icons.call, 'CALL'),
          buildButtonColumn(Icons.near_me, 'ROUTE'),
          buildButtonColumn(Icons.share, 'SHARE'),
        ],
      ),
    );

    Widget textSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Text(
        '''
Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese Alps. Situated 1,578 meters above sea level, it is one of the larger Alpine Lakes. A gondola ride from Kandersteg, followed by a half-hour walk through pastures and pine forest, leads you to the lake, which warms to 20 degrees Celsius in the summer. Activities enjoyed here include rowing, and riding the summer toboggan run.
        ''',
        softWrap: true,
      ),
    );
    return new MaterialApp(
      title: 'Flutter Demo',
      theme: new ThemeData(
        primaryColor: Colors.white,
        accentColor: Colors.orangeAccent
      ),
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Top Lakes'),
        ),
        body: new ListView(
          children: [
            
            titleSection,
            buttonSection,
            textSection
          ],
        ),
      ),
    );
  }
}

```


1.Subclass StatefulWidget 
--- 

Widget을 Build하려고 할 때 즉 별이 증가하려고 할 때 createState()를 재정의하여 State Object를 생성한다.

```java
class FavoriteWidget extends StatefulWidget {
  @override
  _FavoriteWidgetState createState() => _FavoriteWidgetState();
}
```

2.Subclass State
---

FavoriteWidgetState class는 Widget이 변경할 수 있는 변동 가능 Data를 저장한다.
앱을 처음 실행하면 41이라는 종하요 숫자와 함께 isFavorited 가 true가 된다. 

```java
class _FavoriteWidgetState extends State<FavoriteWidget> {
  bool _isFavorited = true;
  int _favoriteCount = 41;
  // ···
}
```
버튼을 누르면 즉 Tab을 하면 Callback 함수 (toggleFavorite)를 정의하여 다시 40으로 돌아온다. 

```java
class _FavoriteWidgetState extends State<FavoriteWidget> {
  // ···
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        Container(
          padding: EdgeInsets.all(0),
          child: IconButton(
            padding: EdgeInsets.all(0),
            alignment: Alignment.centerRight,
            icon: (_isFavorited ? Icon(Icons.star) : Icon(Icons.star_border)),
            color: Colors.red[500],
            onPressed: _toggleFavorite,
          ),
        ),
        SizedBox(
          width: 18,
          child: Container(
            child: Text('$_favoriteCount'),
          ),
        ),
      ],
    );
  }
}
```

3.toggleFavorite
---
toggleFavorite이 호출되면 Widget을 다시 그려줘야하므로 setState()를 호출한다.
조건은 아래를 따른다.
* A star icon and the number 41
* A star_border icon and the number 40

```java
void _toggleFavorite() {
  setState(() {
    if (_isFavorited) {
      _favoriteCount -= 1;
      _isFavorited = false;
    } else {
      _favoriteCount += 1;
      _isFavorited = true;
    }
  });
}
```

## 참고문서
<https://flutter.dev/docs/development/ui/interactive>
