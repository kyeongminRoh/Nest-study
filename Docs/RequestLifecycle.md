### 1. Middleware

요청과 응답 사이에서 동작한다.
Application Lifecycle 중 중간에 위치해서 요청을 처리 응답 수정 요청 중단할 수 있다.

**미들웨어 기능**

1. 요청 및 응답 수정 : 요청이 컨트롤ㄹ러에 도달하기 전에 요청 객체를 수정, 응답이 클라이언트에 전송되기 전에 응답 객체를 수정 가능

2. 요청 필터링 : 특정 조건에 따라 요청을 거부하거나 특정 요청만 처리 가능

3. 요청 중단 : 요청을 중단하고, 특정한 응답을 보낼 수 있다.

4. 로깅 및 감시 : 요청, 응답을 로깅하고 감시하는 용도로 사용가능

5. 인증, 권한 검사 : 요정의 인증, 권한을 검사해서 보안을 강화 시키는 용도도 가능

### 2. Guard

Client의 권한을 체크하는 로직수행 하며, JWT를 사용할 경우 @UserGuards 를 통해 JWT 값을 유효한지 검사할 수 있다.

**처리 순서**

전역 가드, 컨트롤러 가드, 라우트 가드 순서대로 실해하며 사용자의 권한을 체크해 필요에따라 요청을 차단 가능

### 3. Interceptor

컨트롤러에 접근 전, 후에서 바인딩될 수 있다.

Controller에 접근하기 전 사용하려면 return 전에 사용하며,
Controller접근 후 에 사용하려면 return 문 에 사용하면 됨.

Pipe, Controller, Service 에서 발생하는 모든 오류는 catchError에서 읽기가 가능.

### 4. Pipe

Input 을 원하는 형태로 바꾸거나(TransFormation) 검증하거나(Validation) 을 담당한다고 생각하면 된다. 이를 통해 코드의 재사용성을 높여 데이터 일관성을 유지 시킨다.

Pipe 는 @Injextable 데코레이터를 사용하여 선언된다.

**Pipe 의 종류**

1. Validation Pipe : 입력 데이터의 유효성을 검사하는 파이프이며 DTO의 유효성 검사에 사용된다.

2. Transform Pipe : 입력 데이터를 변환하는 파이프.

**ex code**

```
Validation Pipe
  @Get('users')
  getUsers(@Query('id', ValidationPipe) id: number) {
    // 유효성 검사를 통과한 id 값만 사용
    return this.appService.getUserById(id);
  }

Transform Pipe
  @Get(':id')
  getProductById(@Param('id', ParseIntPipe) id: number) {
    // id 값을 숫자로 변환하여 사용
    return this.appService.getProductById(id);
  }
```

**Pipe의 장점**
재사용성 : 파이프는 여러 컨트롤러, 엔드포인트 에서 재사용할 수 있다.

강력한 유효성 검사 : NestJS 에서 기본으로 제공 하고 강력한 유효성 검사 기능을 제공하여 보안과 데이터 무결성을 보장.

단순화된 유효성 검사, 데이터 변환 : 파이프를 사용하면 유효성 검사와 데이터 변환 과정을 단순화 하여 코드 중복 줄임

### 5. Controller

**라우팅**

Get, Post, Put, Delete 데코레이터를 제공하며
@Param, @Body의 요청으로 객체정보를 읽은후 Dto로 입출력을 정의를 해준다.
**ex code**

```
@Post()
create(@Body () createSampleDto: createSamplDto) {
    return this.sampleService.create(createSampleDto);
}

@Get( path: ":id")
findById(@Param( property: "id") id: string) {
    return this.sampleService.findById(+id);
}
```
위와 같은 코드의 형태로 알 수 있다. 그 밖의 @Query, @Headers, @Session 으로 요청 객체를 사용에 맞게 활용이 가능하며, 다음으로 Service 로직을 대체로 수행한다.


### 6. Exception Filter

익셉션필터는 로직을 처리하다가 오류가 발생하면 throw를 한다.

필터는 전역 먼저 확인하지 않는 유일한 구성 요소이다.

라우터 수준 필터가 예외를 포착하면 컨트롤러 또는 전역 필터가 동일한 예외를 포착 할 수 없고, 이와 같은 효과를 얻는 유일한 방법은 필터간에 상속을 사용하는 것이다.

### Providers

의존성 주입의 대상들이다.

의존성 주입이란?

실제 구현 대상을 '주입' 해준다. 어떤 코드를 넣어줄 것인지만 결정하면 코드가 유연해지며, 테스트 하기에도 편하다.

@Injextable : 클래스에 지정하면 해당클래스가 주입의 대상임을 나타냄

@Inject : 클래스의 생성자, 메서드, 속성 등 에 주입할 대상을 표시함

@Optional : 주입할 의존성이 선택적인 경우 사용된다. 의존성 주입 컨테이너가 존재하지 않아도 오류를 발생시키지 않음

@Self : 의존성을 자신의 범위 내에서만 찾도록 지정한다. 일반적으로 클래스 또는 모듈 내의 의존성을 참조할 때 사용

@SkipSelf : 의존성 주입 컨테이너에서 의존성을 찾지 않고 자신을 스킵하도록 지정한다. 즉 자신의 범위를 벗어나 상위 컨테이너에서 의존성을 검색한다. 일반적으로 부모 클래스, 상위 모듈을 참조할 때 사용

