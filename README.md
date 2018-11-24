# Code-Drawing 码绘VS手绘
A comparison of Code-drawing ang Hand-drawing
## 绘图结果
![image](https://github.com/Hello-Oliver/Code-Drawing/blob/master/snow.PNG)
## "码绘VS手绘"主题绘图作业链接
#### [https://github.com/Hello-Oliver/Code-Drawing](https://github.com/Hello-Oliver/Code-Drawing)
## 技巧分析
-   构造雪花片类,函数getRandomSize用于生成雪花上的随机大小
```ecmascript 6
float getRandomSize() {
  
  float r = pow(random(0, 1), 3);
  return constrain(r * 32, 2, 32);
  
}
```
-   从图片中获取雪花图片的素材，从构造函数中可以看出（Snowflake(float sx, float sy, PImage simg)）
```ecmascript 6
  Snowflake(float sx, float sy, PImage simg) {
    float x = sx;
    float y = sy;
    img = simg;
    pos = new PVector(x, y);
    vel = new PVector(0, 0);
    acc = new PVector();
    angle = random(TWO_PI);
    dir = (random(1) > 0.5) ? 1 : -1;
    xOff = 0;
    r = getRandomSize();
  }
```
-   给雪花添加一个合适的力，在这个程序中是有gravity提供，这个力指导雪花自由下落的总方向
```ecmascript 6
  void applyForce(PVector force) {
    // Parallax Effect hack
    PVector f = force.copy();
    f.mult(r);

    // PVector f = force.copy();
    // f.div(mass);
    acc.add(f);
  }

```
-   根据每帧添加在雪花上的力（addForce函数），更新这个雪花对象的相关数据，位置，速度等
```ecmascript 6

  void update() {

    xOff = sin(angle * 2) * 2 * r;

    vel.add(acc);
    vel.limit(r * 0.2);

    if (vel.mag() < 1) {
      vel.normalize();
    }

    pos.add(vel);
    acc.mult(0);

    if (pos.y > height + r) {
      randomize();
    }

    // Wrapping Left and Right
    if (pos.x < -r) {
      pos.x = width + r;
    }
    if (pos.x > width + r) {
      pos.x = -r;
    }

    angle += dir * vel.mag() / 200;

  }
```
-  最后将更新过的雪花的位置画出来
```ecmascript 6
  void render() {
    // stroke(255);
    // strokeWeight(r);
    // point(pos.x, pos.y);
    pushMatrix();
    translate(pos.x + xOff, pos.y);
    rotate(angle);
    imageMode(CENTER);
    image(img, 0, 0, r, r);
    popMatrix();


  }
}
```
-   在控制程序（snowfall）中，setup一开始就生成了一系列的雪花片对象，分别进行处理
```ecmascript 6
void setup() {
  size(800,600);
  spritesheet = loadImage("flakes32.png");
  
  snow = new ArrayList<Snowflake>();
  textures = new ArrayList<PImage>();
  
  gravity = new PVector(0, 0.3);
  for (int x = 0; x < spritesheet.width; x += 32) {
    for (int y = 0; y < spritesheet.height; y += 32) {
      PImage img = spritesheet.get(x, y, 32, 32);
      image(img, x, y);
      textures.add(img);
    }
  }
  
  for (int i = 0; i < 400; i++) {
    float x = random(width);
    float y = random(height);
    int designIndex = floor(random(textures.size()));
    PImage design = textures.get(designIndex);
    snow.add(new Snowflake(x, y, design));
  }
}
```
## 对于码绘和手绘的异同分析
-   绘图思路：
最近看了b站上有人搬运了国外大佬的一些processing教程，其中有一些内容很感兴趣，就是“自主个体”，自主个体的现象在生活中十分常见，如一堆觅食的蚂蚁，灯罩外面飞着的飞蛾，主要就是一些粒子的运动，他们的运动规律包括：可以感知的范围有限、有一定的目标、有一定的运动准则，这些粒子在可以感知的范围之内可以将准则进行实施。当然在本例当中飘飞的雪花并额米有一个目标，因为我只需要他在gravity的运动准则之下向下运动即可。
    相同点：在代码中，雪花的下落是由addForce()来决定的，其中力的添加具有随机性，；用牙刷沾上颜料，再用尺子拨动牙刷毛，将颜料溅在纸面上，形成雪花的效果，这个过程充满趣味和随机性。所以随机性是共同点。
    差异点：雪花在码绘之中是运动的，给人一种舒服的感觉；作画是静止的，没有办法表现出雪花运动的感觉。
