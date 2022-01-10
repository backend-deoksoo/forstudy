# express.js
## 주요 개념
> 어플리케이션, 미들웨어, 라우팅, 요청객체, 응답객체<br>

## 어플리케이션
> 역할: 어플리케이션 추가, 라우팅 설정, 서버를 요청 대기 상태 만들기 <br>
> 서버의 몸통이다. 필요한 기능 및 로직을 여기에 추가하는 방식

```javascript
    const express = require('express');
    const app = express();
    // 미들웨어 추가
    app.use(middleware());
    // 라우팅 설정
    app.get('/',function(req,res){});
    // 요청 대기상태
    app.listen(3000,function(){
        console.log('Server is running!!');
    }}
```

## 미들 웨어
> 미들웨어는 함수들의 연속, 서버의 기능은 미들웨어 형태로 추가된다. <br>
> 커스텀 미들웨어를 만들 수 있고, 서드파티 미들웨어를 사용할 수도 있다. <br>
> `next()`함수: 미들웨어 내부에 호출하지 않으면, 다음으로 진행되지 않는다. 클라이언트에서 호출을 해도 서버가 멈춘것처럼 응답이 오질 않음.<br>
> 일반 미들웨어와 에러 미들웨어로 구분된다.

```javascript
    const thirdpartymw = require('thirdpartymw');

    function commonmw(req, res, next){
        console.log('i am commonmw');
        // next()를 해줘야만 다음 로직을 수행. 없을 경우 여기서 멈춤.
        // 정상 진행이 될 경우 다음 미들웨어로 진행
        // 에러가 발생할 경우 에러처리 미들웨어로 진행
        if (notErrorOcurred) next();
        else next(new Error('ohh!')); 
    }
    function errormw(err, req, res, next){
        // ohh!가 출력된다.
        console.log(err.message);
        next();
    }

    app.use(commonmw);
    app.use(errormw);
    app.use(thirdpartymw);

    app.get('/',function(req,res){
        res.json('hello');
    })
    // next()가 없을 경우: 루트로 request를 보내면,'i am commonmw'가 출력 된 이후에 미들웨어와 '/'라우터가 진행되지 않는다.
```

## 라우팅
> 요청 url에 대해서 적절한 핸들러 함수로 연결해 주는 기능<br>
> 어플리케이션 객체의 get(), post() 메소드 등으로 구현가능<br>
> 전용 Router클래스 사용 가능

#### 기본 라우팅 예
```javascript
    app.METHOD(path, handler);
    // ex
    app.get('/user',function(res,req){});
```

#### Router 클래스
```javascript
    // info.js
    const express = require('express');
    const router = express.Router();
    
    router.get('/uesr',function(res, req){});
    module.export = router;

    // index.js
    const info = require('./info');
    // ...
    app.use('/info',info);
    // 'host/info/user' 주소로 접근 가능
```

## 요청객체와 응답객체
> http의 req와 res객체를 한번 더 래핑한 것.<br>
> req.params(), req.query(), req.body() 등의 메소드 사용.<br>
> res.send(), res.status(), res.json() 등의 메소드 사용.