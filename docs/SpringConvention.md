# SpringConvention

## 패키지 구조
core모듈과 feature모듈로 나누어 구성한다.

### core
- 공통으로 사용되는 클래스, 인터페이스, 유틸리티 등을 포함한다.
- core 모듈간에는 의존성이 허용된다
- global, feature 모듈에 대한 의존성을 가지지 않는다.
- 외부 라이브러리에 대한 의존성을 가지지 않는다.

### global
- 전역적으로 사용되는 클래스, 인터페이스, 유틸리티 등을 포함한다.
- core 모듈에 대한 의존성을 가질 수 있다.
- feature 모듈에 사용되는 횡단 관심사를 처리한다.
- feature 모듈에서 사용되는 클래스, 인터페이스, 유틸리티에 대한 의존성을 가지지 않는다.

### feature
- 각 기능별로 모듈을 나누어 구성한다.
- core 모듈에 대한 의존성을 가질 수 있다.

### Layer 구조
- 데이터 영역에서 웹 영역에 의존성을 가지지 않는다.
- 패키지 의존성은 단방향으로 유지하고 순환 참조을 피한다.


## 세부 규칙

### 공통
- 생성자 생성을 지양하고 `빌더 패턴` 혹은 `팩토리 메서드`를 사용한다.
  - `from` : 파라미터로 넘어온 값들을 해당 클래스의 인스턴스로 변환할 때 사용한다. 이 과정에서 파라미터의 값들 중 일부는 유실될 수 있다.
  - `create` : 파라미터로 넘어온 값들을 이용하여 새로운 인스턴스를 생성할 때 사용한다. 이 과정에서 파라미터의 값들은 모두 사용된다.

### controller
- `Swagger`의 어노테이션을 이용하여 API에 대한 문서화를 진행하여 주석의 역할을 대신한다.
- `@Operation` 어노테이션을 사용하여 `summary`에 API에 대한 설명과 `description`에 상세한 설명을 동작과 응답값을 명시한다.
- `@RequestMapping` 어노테이션대신 각각의 API에 FullPath를 명시한다.
- `@ResponseStatus` 어노테이션을 사용하여 응답 상태 코드를 명시한다.

### dto
dto 패키지에는 `Request`, `Response`, `Model` 클래스를 포함한다.
- `Request` 클래스는 클라이언트로부터 받은 요청을 담는 클래스이다.
- `Response` 클래스는 클라이언트에게 응답을 보내기 위한 클래스이다.
- `Model` 클래스는 클리이언트에서 사용되는 데이터의 형태를 담는 클래스이다.

### service
- `@Service` 어노테이션을 사용하여 서비스 클래스를 명시한다.
- 트랜잭션 스크립트를 지양하고 도메인 모델을 사용한다.

### domain
domain 패캐지는 POJO 클래스로 구성한다.
- `record`와 같은 데이터 클래스를 사용하지 않는다.

### repository
- Database 혹은 외부 API와의 통신을 담당하는 클래스이다.
- 외부 API와의 통신을 담당하는 클래스는 `Client`라는 접미사를 붙일 수 있다.
- `Service`에서 직접적으로 외부 API와 통신하지 않고 Repository를 통해 통신한다.


### entity
- `@Entity` 어노테이션을 사용하여 JPA Entity 클래스를 명시한다.
- 테이블의 이름은 단수형으로 작성한다.
- 연관관계에는 항상 `FetchType.LAZY`를 사용한다.
- `@Enumerated`에는 `EnumType.STRING`을 사용한다.
- Setter를 사용하지 않으며 생성자를 통해 값을 설정한다.