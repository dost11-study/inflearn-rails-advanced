# inflearn-rails-advanced
인프런의 [[심화] 인디해커를 위한 루비온레일즈 8 입문 강의](https://www.inflearn.com/course/%EC%9D%B8%EB%94%94%ED%95%B4%EC%BB%A4-%ED%94%84%EB%A6%AC%EB%9E%9C%EC%84%9C-rubyonrails-%EC%99%84%EB%B2%BD%EC%8B%AC%ED%99%94)를 정리한 내용입니다.

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

- 블록을 받는 메서드

  ```ruby
  def my_method
    puts "메서드 시작"
    yield if block_given?
    puts "메서드 종료"
  end
  
  # 블록과 함께 호출
  my_method { puts "블록 실행!" }
  # => 메서드 시작
  # => 블록 실행!
  # => 메서드 종료
  
  # 블록 없이 호출
  my_method
  # => 메서드 시작
  # => 메서드 종료
  
  my_method do
    puts "블록 실행!"
  end
  # => 메서드 시작
  # => 블록 실행!
  # => 메서드 종료
  ```

- Proc은 블록을 객체로 만드는 방법이다

  ```ruby
  # 기본 사용법
  my_proc = Proc.new { puts "안녕하세요!" }
  my_proc.call  # 실행
  # => 안녕하세요!
  
  # 파라미터가 있는 경우
  my_proc = Proc.new { |x| x * 2 }
  my_proc.call(5)
  # => 10
  ```

- Proc 객체를 인자 형태로 넘기면 오류가 난다. Proc 객체를 블록으로 변환하기 위해선 &를 사용해야함
  &의 역할은 블록 ↔ Proc 자동 변환이다

  ```ruby
  # 블록 → Proc (파라미터에서 &)
  def my_method(&block)  # 블록을 Proc으로 받음
    block.call
  end
  
  my_method { puts "Hi" }
  
  
  # Proc → 블록 (호출할 때 &)
  my_proc = Proc.new { |n| n * 2 }
  [1, 2, 3].map(&my_proc)  # Proc을 블록으로 전달
  # => [2, 4, 6]
  
  
  # Symbol → Proc → 블록 (&:method)
  ["a", "b"].map(&:upcase)
  # :upcase.to_proc → Proc.new { |obj| obj.upcase }
  # & 붙여서 블록으로 전달
  # => ["A", "B"]
  ```

- 람다는 함수를 객체로 만들어서 변수에 저장하고 전달할 수 있게 하는 것

  ```ruby
  # 방법 1: lambda 키워드
  add = lambda { |a, b| a + b }
  
  # 방법 2: -> (stabby lambda) - 더 많이 씀!
  add = ->(a, b) { a + b }
  
  # 실행
  add.call(3, 5)  # => 8
  add[3, 5]       # => 8 (축약형)
  ```

- Proc과 Lambda의 차이

  ```ruby
  ##### 인자 개수의 차이
  
  # Lambda - 인자 개수 엄격
  my_lambda = ->(x, y) { x + y }
  my_lambda.call(1, 2)     # ✅ OK
  my_lambda.call(1)        # ❌ ArgumentError!
  
  # Proc - 인자 개수 유연
  my_proc = Proc.new { |x, y| x + y }
  my_proc.call(1)          # ✅ OK (y는 nil)
  my_proc.call(1, 2, 3)    # ✅ OK (3은 무시)
  ```

  ```ruby
  ##### return 동작
  
  def test_proc
    my_proc = Proc.new { return "Proc return" }
    my_proc.call
    "메서드 끝"  # 실행 안 됨!
  end
  
  def test_lambda
    my_lambda = lambda { return "Lambda return" }
    my_lambda.call
    "메서드 끝"  # 실행됨!
  end
  
  puts test_proc    # => "Proc return" (메서드 자체를 return)
  puts test_lambda  # => "메서드 끝" (lambda만 return)
  ```

- **Lambda 사용**
  - 함수형 프로그래밍
  - 재사용 가능한 로직
  - Rails scope
  - 인자 검증 필요할 때 
- **Proc 사용**
  - 콜백, 이벤트 핸들러
  - 유연한 인자 처리

- 모듈

  - 메서드와 상수를 그룹화하는 컨테이너 (클래스처럼 보이지만 인스턴스를 만들 수 없음)

    ```ruby
    module Math
      PI = 3.14
      
      def self.circle_area(r)
        PI * r ** 2
      end
    end
    
    Math::PI  # => 3.14
    Math.circle_area(5)  # => 78.5
    ```

- 믹스인

  - 모듈을 클래스에 섞어 넣어서 기능 추가

    ```ruby
    module Greetable
      def say_hello
        "안녕!"
      end
    end
    
    class Person
      include Greetable  # 믹스인!
    end
    
    Person.new.say_hello  # => "안녕!"
    ```

- include vs extend vs prepend

  - include는 인스턴스 메서드이다. 여러 클래스에서 같은 기능(메서드)를 추가할 때 쓴다.
  - extend는 클래스 메서드
  - prepand는 AOP 혹은 decorator 같은 개념이다. 원래 메서드를 감싸서(wrap) 앞뒤로 뭔가 추가하고 싶을 때 사용한다.

- 왜 모듈을 사용하는가?

  - Ruby는 다중 상속이 안되기 때문에, 모듈로 해결한다.

##  Fat Model

- Fat Model은 비즈니스 로직을 모델에 집중시키는 것이다
  - 복잡한 쿼리, 데이터 변환, 유효성 검증
  - 재사용 가능한 메서드 정의
  - 모델 내부에 스코프와 콜백 활용

    ```ruby
    class Post < ApplicationRecord
      belongs_to :user
      has_many :comments
      
      # 스코프 - 재사용 가능한 쿼리
      scope :published, -> { where(published: true) }
      scope :draft, -> { where(published: false) }
      scope :recent, -> { order(created_at: :desc) }
      scope :popular, -> { where('views_count > ?', 100) }
      scope :by_category, ->(category) { where(category: category) }
      scope :created_after, ->(date) { where('created_at > ?', date) }
      
      # 스코프 체이닝 가능
      scope :featured, -> { published.popular.recent }
      
      # 조건부 스코프
      scope :search, ->(query) {
        return all if query.blank?
        where('title LIKE ? OR content LIKE ?', "%#{query}%", "%#{query}%")
      }
    end
    ```

    ```ruby
    class User < ApplicationRecord  
    	has_many :posts
      has_one :profile
      
      # 유효성 검사 전
      before_validation :normalize_email
      
      # 저장 전 (create/update 모두)
      before_save :encrypt_password, if: :password_changed?
      before_save :set_slug
      
      # 생성 전/후
      before_create :set_default_role
      after_create :send_welcome_email
      after_create :create_default_profile
      
      # 업데이트 전/후
      before_update :track_changes
      after_update :notify_followers, if: :saved_change_to_name?
      
      # 삭제 전/후
      before_destroy :check_dependencies
      after_destroy :cleanup_files
      
      # 커밋 후 (트랜잭션 완료 후)
      after_commit :index_for_search, on: [:create, :update]
      after_commit :clear_cache, on: :update
      
      private
      
      def normalize_email
        self.email = email.to_s.downcase.strip
      end
      
      def encrypt_password
        self.encrypted_password = BCrypt::Password.create(password)
      end
      
      def set_slug
        self.slug = name.parameterize if name_changed?
      end
      
      def set_default_role
        self.role ||= 'user'
      end
      
      def send_welcome_email
        UserMailer.welcome(self).deliver_later
      end
      
      def create_default_profile
        create_profile!(bio: "새로운 사용자입니다.")
      end
      
      def track_changes
        # 변경 사항 로깅
        Rails.logger.info "User #{id} updated: #{changes.inspect}"
      end
      
      def notify_followers
        followers.each do |follower|
          NotificationMailer.name_changed(follower, self).deliver_later
        end
      end
      
      def check_dependencies
        if posts.exists?
          errors.add(:base, "게시글이 있는 사용자는 삭제할 수 없습니다")
          throw(:abort)  # 삭제 중단
        end
      end
      
      def cleanup_files
        # S3에서 프로필 사진 삭제 등
        avatar.purge if avatar.attached?
      end
      
      def index_for_search
        SearchIndexJob.perform_later(self)
      end
      
      def clear_cache
        Rails.cache.delete("user_#{id}")
      end
    end
    ```

### Thin Controller

컨트롤러는 간결하게 유지한다

- HTTP 요청/응답 처리에 집중
- 모델에서 데이터 로드
- 뷰로 데이터 전달
- 복잡한 로직은 모델로 위임

컨트롤러의 액션(Action)은 다음과 같다

| 액션        | HTTP 메서드    | URL             | 용도           |
| :-------- | :---------- | :-------------- | :----------- |
| `index`   | GET         | `/posts`        | **목록** 보기    |
| `show`    | GET         | `/posts/1`      | **개별** 상세 보기 |
| `new`     | GET         | `/posts/new`    | 생성 폼         |
| `create`  | POST        | `/posts`        | 생성 처리        |
| `edit`    | GET         | `/posts/1/edit` | 수정 폼         |
| `update`  | PATCH / PUT | `/posts/1`      | 수정 처리        |
| `destroy` | DELETE      | `/posts/1`      | 삭제           |

    
## Convention over Configuration
Rails는 "설정보다 관례"를 중요시하는 철학을 가지고 있음.
의례적으로 이렇게 하면 설정을 따로 할 필요가 없다라는 의미를 Rails에서 포함하고 있으며
관례를 최대한 따라라라고 이야기한다.

모든게 커스터마이징 가능한데 그냥 디폴트 설정을 내가 쓸 거면 설정을 추가적으로 할 필요가 없다.

- 명명규칙
	- 모델: 단수형, 카멜 케이스 (`User`, `BlogPost`)
	- 테이블: 복수형, 스네이크 케이스 (`users`, `blog_posts`)
	- 컨트롤러: 복수형, 카멜 케이스
- 파일 위치 규칙
	- 모델: `app/models/user.rb`
	- 컨트롤러: `app/controllers/users_controller.rb`
	- 뷰: `app/views/users/index.html/erb`
- 자동 라우팅
	- `resources :posts` (7개의 RESTful 라우트 자동 생성)
- 관계 설정
```ruby
class User < ApplicationRecord
  has_many :posts # posts 테이블의 user_id 외래키 자동 인식
end
```
- 유효성 검증
```ruby
validates :email, presence: true # 에러 메시지 자동 생성
```
- 레이아웃
`app/views/layouts/application.html.erb` 자동 적용

## Rails 폴더 구조
- `app/` 애플리케이션의 핵심 코드
	- `models/` 데이터 모델 클래스
	- `views/` 템플릿 파일
	- `controllers/` 컨트롤러 클래스
	- `helpers/` 뷰 헬퍼 메서드
	- `mailers/` 이메일 관련 클래스
	- `jobs/`  백그라운드 작업
	- `assets/` 자바스크립트, CSS, 이미지
	- `channels/` ActionCable 채널 (웹소켓, 스트리밍 등)
- `config/` 애플리케이션 설정
	- `routes.rb` 라우팅 정의
	- `database.yml` 데이터베이스 설정 (저장소 푸시 X)
	- `environments/` 환경별 설정
- `db/` 데이터베이스 관련 파일
	- `migrate/` 마이그레이션 파일
	- `schema.rb` 데이터베이스 스키마
	- `seeds.rb` 초기 데이터 설정
- `lib/` 라이브러리 모듈
- `log/` 로그 파일
- `public/` 정적 파일 (404.html, robots.txt 등)
- `test/`, `spec/` 테스트 코드
- `tmp/` 임시 파일
- `vendor/` 서드파티 코드 (부트스트랩과 같은 외부 CSS/JS 파일, Gem 직접 수정 후 사용 시)
- `Gemfile` 의존성 정의
- `Rakefile` Rake 작업 정의 (터미널에서 자주 실행할 법한 스크립트들을 Rake 파일에 정의하고 호출)
 
## Rails 환경

- Development (개발 환경)
	- 코드 변경 시 자동 리로드
	- 상세한 에러 메시지와 디버깅 정보
	- 캐싱 비활성화
	- 이메일 전송 대신 로컬에서 확인
- Test (테스트 환경)
	- 테스트 실행을 위한 최적화
	- 데이터베이스 트랜잭션으로 테스트 격리(테스트마다 자동 롤백)
	- 이메일 전송 비활성화
	- 에셋 컴파일 비활성화
- Production (운영 환경)
	- 코드 변경 시 리로드 안됨 (애플리케이션 재부팅 필요)
	- 성능 최적화
	- 에러 상세 정보 숨김
	- 에셋 사전 컴파일
	- 캐싱 활성화
	- 실제 이메일 전송

## Rails 환경 전환 방법
- 환경 변수 설정
	- `RAILS_ENV=production rails server`
- 명령어에 환경 지정
	- `rails server -e production`
	- `rails db:migrate RAILS_ENV=production`