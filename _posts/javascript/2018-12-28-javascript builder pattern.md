---
title: "[design pattern] Builder pattern - javascript"
author_profile: true
categories: 
  - javascript
toc: true
---

inner class 를 이용한 빌더 패턴
--------------------

[inner class]
```javascript
class User {
  constructor(build) {
    if (build) {
      this.id = build.id;
      this.name = build.name;
      this.age = build.age;
    }
  }

  static get Builder() {
    class Builder {

      setId(id) {
        this.id = id;
        return this;
      }

      setName(name) {
        this.name = name;
        return this;
      }

      setAge(age) {
        this.age = age;
        return this;
      }

      build() {
        return new User(this);
      }
    }

    return new Builder();
  }
}
```

[Test code]
```javascript
function testBuilderPattern() {

  const builder = User.Builder;
  const user1 = builder.setId(1).setName('Alice').setAge(22).build();
  console.log('user1', user1);

  const builder2 = User.Builder;
  const user2 = builder2.setId(2).setName('Bob').build();
  console.log('user2', user2);

}

testBuilderPattern();
```

[Console output]  
![screenshot](/assets/img/2018-12-30_js_builder_screenshot1.png){:style="max-width:400px;"}


class 상속을 이용한 빌더 패턴
--------------------

[builder class]
```javascript
class Builder {

  init() {
    Object.keys(this).forEach((key) => {
      const setterName =
        `set${key.substr(0, 1).toUpperCase()}${key.substr(1)}`;

      this[setterName] = (value) => {
        this[key] = value;
        return this;
      };
    });
  }

  build() {
    return this;
  }

}

class User extends Builder {
  constructor() {
    super();

    this.id = -1;
    this.name = null;
    this.age = 0;

    super.init();
  }
}
```

[Test code]
```javascript
function testBuilderPattern() {

  const user = new User()
    .setId(1)
    .setName('jason')
    .setAge(22).build();

  console.log(user);

}

testBuilderPattern();
```

[Console output]  
![screenshot](/assets/img/2018-12-30_js_builder_screenshot2.png){:style="max-width:400px;"}  
  
상속을 이용한 방법은 클래스에 존재하는 멤버 변수들의 setter 함수(instance를 반환 하는)
들을 자동으로 만들어 주므로 위의 콘솔 출력과 같이 setter들이 같이 생기게 된다.
