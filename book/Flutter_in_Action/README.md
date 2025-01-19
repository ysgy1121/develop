
> 🐭 Flutter in Action을 읽고, 나만의 방식으로 다시 정리해보고자 한다.


### PART1. 플러터와 다트
##### Chapter1. 플러터
- 플러터는 구글에서 만들어 오픈 소스로 공개한 모바일 SDK다
- 플러터는 하나의 코드 베이스로 안드로이드, IOS, macOS, Linux, Window, Web으로 배포할 수 있다 (2025년 기준)
- 플러터는 개발자가 도메인 기능에 집중할 수 있도록 렌더링 엔진, UI 컴포넌트, 테스트 프레임워크, 도구, 라우터 등 앱을 만드는 데 필요한 모든 기능을 제공하는 플랫폼이다.
- 플러터는 배우기 쉽고, 모든 코드가 오픈 되어 있어 세부적인 제어도 가능하다.
- 플러터는 자바스크립트 기반의 크로스 플랫폼과 달리 자바스크립트 브리지를 발생시키지 않는다. 웹뷰로 구현할때의 DOM 성능이슈도 존재하지 않는다. 자체 렌더링 엔진을 탑재하고 있어 네이티브와 바로 소통하며 화면의 모든 픽셀을 직접 제어한다.
- 플러터는 테스트가 용이하다
- 플러터는 작은 컴포넌트(위젯)를 조합해 모바일 UI를 만든다.
- 스타일, 애니메이션, 리스트, 텍스트, 버튼, 페이지 등 UI를 구성하는 모든 것이 위젯이다. (플러터의 APP객체도 위젯)
- 플러터는 작은 위젯을 조합하는 방법으로 커스텀 위젯을 만든다.
    ```dart
    // 상속 방법 🚫
    class AddToCartButton extends Button {} 

    // 조합 방법 🟢
    class AddToCartButton extends StatelessWidget {
        @overrid
        build() {
            return Center(
                child: Button(
                    child: Text('Add to Cart')
                )
            );
        }
    } 
    ```
- 위젯은 `build()`를 포함한 다양한 생명주기 메서드와 객체 맴버를 포함한다.
- 위젯은 `StateFulWidget`과 `StatelessWidget`이 있다.
- `StatelessWidget`은 정보를 저장하지 않는 위젯으로, 생명주기를 프레임워크가 관리한다. (제거, 리빌드 등)
- `StatelessWidget`도 정보가 변경되면 다시 그려진다. <!--정보를 저장하지는 않지만 정보를 가지고 있고 이 정보가 변경되면 리빌드됨-->
- `StatefulWidget`은 `State` 객체를 가지고 정보를 저장하며 `setState()` 함수를 호출하여 정보 변경 및 위젯을 다시 그려야함을 알린다.
    ```Dart
    Widget build(BuildContext context) {
        return IconButton(
            icon: Icons.add,
            onPressed: () {
                setState(() {
                    this.quntity++;
                });
            }
        )
    }
    ```

###### StatefulWidget의 생명주기
 ![StatefulWidget의 생명주기](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzG45G%2Fbtrv45wNche%2FJJqA7K0w6FSRMAdnNCApY0%2Fimg.png)
 이미지 출처: [[flutter] 플러터 StatefulWidget 라이프 사이클 (lifecycle)](https://fronquarry.tistory.com/16)
1. 페이지로 이동하면 플러터가 객체를 만들고 이 객체는 위젯과 관련된 `State` 객체를 만든다.
2. 위젯이 마운트되면 플러터가 `initState`를 호출한다[^1].
3. 상태를 초기화하면 플러터가 위젯을 빌드한다. 그 결과 화면에 위젯을 그린다.
4. 위젯은 세가지 이벤트 중 하나를 기다린다.
    - 사용자가 앱의 다른화면으로 이동하면서 화면을 폐기(Dispose)한다
    - 트리의 다른 위젯이 갱신되면서 위젯이 의존하는 설정이 바뀜. 위젯의 상태는 `didUpdateWidget`을 호출하며 필요하다면 위젯을 다시 그림. 예를 들면 트리의 상위 위젯에서 해당 위젯의 버튼을 비활성화하는 경우
    - 사용자가 버튼을 눌러 `setState`를 호출해 위젯의 내부 상태가 갱신되어 플러터가 위젯을 다시 빌드하고 그리는 상황인 경우
   
[^1]: 위젯이 마운트(Mount) 된다는 것은, Flutter에서 해당 위젯이 위젯 트리에 추가되어 렌더링 준비를 마쳤다는 것을 의미한다. 쉽게 말해, 화면에 나타나거나 나타날 준비가 된 상태.  
  
  


##### Chapter2. 다트
- 플러터는 다트 언어를 사용한다.
- 다트는 JIT[just-in-time] 컴파일과 AOT[ahead-of-time] 컴파일을 모두 지원하기 때문에 뛰어난 성능과 개발의 편의를 동시에 누릴 수 있다.
    * AOT 컴파일러 / 다트 코드를 효율적인 네이티브 코드로 바꿈, 빠르게 동작하고 전체 프레임워크를 거의 다트로 구현할 수 있기 때문에 거의 모든 것을 커스터마이즈할 수 있다.
    * 선택형 JIT 컴파일러 / 핫 리로드 지원, 빠른 개발 속도
- 다트는 객체지향 언어다. <!--책에서 마크다운 언어가 아니라는 걸 강조했는데, 맥락이 이해가 되지않는다 -->
- 다트는 생산성이 좋고 예측 가능한 언어다. 다른 고급 프로그래밍 언어들과 비슷
- 다트의 형식 시스템과 객체지향 덕분에 재사용할 수 있는 UI 컴포넌트를 쉽게 구현 가능
- 다트는 데이터를 UI로 편리하게 변환하도록 다양한 함수형 프로그래밍 기능도 지원
- 다트는 처음 웹 개발 언어로 만들어졌고, 다트 코드를 자바스크립트로 변환하는 컴파일러를 구현했다. 따라서 플러터의 웹 배포가 가능하다


 

    



    

##### Chapter3. 플러터의 세계로



### PART2. 사용자 상호작용과 스타일, 애니메이션
##### Chapter4. 플러터 UI: 주요 위젯, 테마, 레이아웃
##### Chapter5. 사용자 입력: 폼과 제스처
##### Chapter6. 픽셀 제어: 플러터 애니메이션과 캔버스 사용하기


### PART3. 상태 관리와 비동기 작업
##### Chapter7. 플러터 라우팅
##### Chapter8. 상태 관리
##### Chapter9. 비동기 다트와 플러터 그리고 무한 스크롤


### PART4. 기초를 넘어
##### Chapter10. 데이터 처리: HTTP, 파이어스토어, JSON
##### Chapter11. 플러터 앱 테스트


### PART5. 부록