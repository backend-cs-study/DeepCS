## 프로그래밍 패러다임

### 순수 함수와 고차 함수의 차이점은?

*
    <details>
    <summary>예상 답변</summary>

    * 순수 함수는 입력값만을 기준으로 출력을 결정하지만 고차 함수는 함수차체를 매개변수로 받거나 반환하는 함수
    * 두 함수는 역할과 목적이 완전히 다름
        * 순수 : 예측가능, 부작용 없는 코드
            1) 외부 상태에 의존 ❌
            2) side-effect ❌
        * 고차 : 재사용성 / 유연성
            1) 함수를 `데이터`처럼 다룸
            2) 콜백, 커링 등에 사용
    * 예시
        * 순수
          ```java
          int add(int x, int y) {return x + y;}
        * 고차
          ```java
          public class FunctionReturningFunction {
        
            // 고차 함수: 함수를 반환하는 함수
            public static Function<Integer, Integer> makeAdder(int toAdd) {
                return x -> x + toAdd;
            }
        
            public static void main(String[] args) {
                Function<Integer, Integer> addFive = makeAdder(5);   // x -> x + 5
                System.out.println(addFive.apply(10));               // 결과: 15
            }
          }
</details>

<br>

### 오버로딩과 오버라이딩의 차이점은?

*
  <details>
  <summary>예상 답변</summary>

  * 오버로딩 : 같은 이름을 메소드의 매개변수만 다르게 여러개 정의
  * 오버라이딩 : 상속받은 메소드 재정의
  * 사용 목적
    * 오버로딩 - 다양한 입력에 대해 동일한 기능을 유연하게 처리할 때 사용
    * 오버라이딩 - 상위클래스의 기능을 하위 클래스가 변경/확장할 때 사용
  * 예시
    * 오버로딩
      ```java
      public class Calculator {
      
        public int add(int a, int b) {
            return a + b;
      }

        public double add(double a, double b) {  // 매개변수 타입이 다름
            return a + b;
      }

        public int add(int a, int b, int c) {    // 매개변수 개수가 다름
            return a + b + c;
        }
      }
    * 오버라이딩
      ```java
      class Animal {
      
       public void sound() {
            System.out.println("동물이 소리를 낸다");
         } 
       }

      class Dog extends Animal {
      
      @Override
      public void sound() {
           System.out.println("멍멍!");
        }
      }
      </details>