# [심화] 인디해커를 위한 루비온레일즈 8 완벽 강의

### Ruby의 핵심 개념

1. 모든 것이 객체 - 정수, 문자열, nil 등 모두 객체
2. 개발자 친화적인 문법 - 가독성과 표현력 강조
3. 동적 타이핑 - 타입 선언 불필요 (Ruby 3에서 선택적 타입 시스템 추가)
4. 블록과 클로저 - 강력한 함수형 프로그래밍 지원
5. 메타프로그래밍 - 런타임에 코드 생성 및 수정 가능
6. 믹스인 - 다중 상속 대신 모듈 사용

### Ruby 문법

- Rails의 상수는 재할당이 가능하다. Warning만 발생하게 됨.

  ```ruby
  MY_CONSTANT = "첫 번째 값"
  
  MY_CONSTANT = "두 번째 값"  # => warning: already initialized constant MY_CONSTANT
  ```

- boolean의 컨벤션은 is_xxxx, has_xxxx 등이 있다.

- Heredoc(히어닥) 문법은 3가지가 존재한다.

  - Squiggly Heredoc(스퀴글리 히어닥) -> 보통 이것만 씀

  - 일반 Heredoc

  - 대시 Heredoc

    ```ruby
    # 들여쓰기 자동 제거
    <<~TEXT
      안녕하세요
      Ruby입니다
      들여쓰기가 제거됩니다
    TEXT
    # => "안녕하세요\nRuby입니다\n들여쓰기가 제거됩니다\n"
    
    # 들여쓰기 그대로 유지
    <<TEXT
      안녕하세요
      Ruby입니다
      들여쓰기가 그대로 유지됩니다
    TEXT
    # => "    안녕하세요\n    Ruby입니다\n    들여쓰기가 그대로 유지됩니다\n"
    
    
    # 종료 구분자만 들여쓰기 가능
    <<-TEXT
      안녕하세요
      Ruby입니다
      들여쓰기가 그대로 유지됩니다
    TEXT  # 이 종료 구분자만 들여쓰기 가능
    # => "    안녕하세요\n    Ruby입니다\n    들여쓰기가 그대로 유지됩니다\n"
    ```

    

- range 변수는 다음과 같이 사용한다.

  ```ruby
  numbers = 1..10 # => 1..10
  chars = 'a'...'b' # => "a"..."b"
  numbers.each {|number| puts number}
  1
  2
  3
  4
  5
  6
  7
  8
  9
  10
  # => 1..10
  chars.each {|char| puts char} # ...은 마지막 요소 제거
  a
  # => "a"..."b"
  
  numbers.reduce {|sum, n| sum + n}
  # => 55
  ```

- 배열에서의 유용한 문법

  ```ruby
  fruits = ["사과", "바나나", "오렌지"]
  
  fruits[0..1]
  # => ["사과", "바나나"]
  
  fruits << "망고"
  # => ["사과", "바나나", "오렌지", "망고"]
  
  fruits.push("포도") # << 연산자랑 같은 기능
  # => ["사과", "바나나", "오렌지", "포드"]
  
  fruits.unshift("딸기") # 앞에 추가
  # => ["딸기", "사과", "바나나", "오렌지", "포드"]
  
  fruits.pop # 마지막 요소 제거
  # => "포도"
  
  fruits.shift # 첫 요소 제거
  # => "딸기"
  
  numbers = [5, 3, 8, 1, 2]
  
  numbers.map {|n| n*2}
  # => [10, 6, 16, 2, 4]
  
  numbers.select {|n| n > 3} # 자바의 stream filter 같은 개념
  # => [5, 8]
  
  numbers.reject {|n| n > 3}
  # => [3, 1, 2] 
  ```

- 해시에서의 유용한 문법

  ```ruby
  user.fetch(:email, "Not Found")
  # 해시 함수에 존재하지 않을 경우 Not Found, 즉 설정한 기본 값 반환
  # => "Not Found"
  ```

- 조건문에서 추가 조건을 나열할 시 elsif를 사용해야하며, if not과 unless는 같은 syntax이다.

- nil일 경우만 할당하는 방법도 존재한다. (nil 가드)

  ```ruby
  error ||= "에러 없음"
  # => "에러 없음"
  ```

- 키워드 파라미터

  ```ruby
  def greet(name:, age:)
    puts "안녕하세요, #{name}님! 나이: #{age}세"
  end
  
  greet(name: "홍길동", age: 30)
  greet(age: 25, name: "김철수")  # 순서 바뀌어도 OK
  # greet("홍길동", 30)  # ❌ 에러! 키워드로 넘겨야 함
  ```

- 가변인자

  ```ruby
  def sum(*numbers)  # 여러 개 받을 수 있음
    numbers.sum
  end
  
  sum(1, 2, 3, 4, 5)
  # => 15
  ```

- 키워드 가변인자

  ```ruby
  def user_info(name:, **options)
    puts "이름: #{name}"
    puts "추가 정보: #{options.inspect}"
  end
  
  user_info(name: "홍길동", age: 30, city: "서울", job: "개발자")
  # => 이름: 홍길동
  # => 추가 정보: {:age=>30, :city=>"서울", :job=>"개발자"}
  ```

- ! (뱅)은 함수가 원본값을 바꾼다는 것을 의미한다.

  ```ruby
  def upcase!(str)
    str.replace(str.upcase)
  end
  ```

   
