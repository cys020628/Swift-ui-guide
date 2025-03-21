# SwiftUI 개념 정리

## 1. 뷰와 레이아웃 (View & Layout)

SwiftUI에서 가장 중요한 기본적인 요소는 뷰입니다. `VStack`, `HStack`, `ZStack`, `Text`, `Button` 등 다양한 뷰를 사용해 UI를 구성합니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack {  // 수직 스택: 뷰를 위에서 아래로 배치
            Text("Hello, SwiftUI!")  // 텍스트 뷰
                .font(.title)  // 텍스트의 폰트 설정
                .padding()  // 텍스트에 여백 추가
            
            Button(action: {
                print("Button tapped!")
            }) {
                Text("Tap Me!")  // 버튼에 텍스트 추가
                    .padding()  // 버튼에 여백 추가
                    .background(Color.blue)  // 버튼 배경 색
                    .foregroundColor(.white)  // 버튼 텍스트 색
                    .cornerRadius(10)  // 버튼 모서리 둥글게
            }
        }
        .padding()  // VStack에 전체적인 여백 추가
    }
}
```

### 설명
- VStack: 수직으로 뷰를 배치하는 컨테이너.
- Text: 화면에 텍스트를 표시하는 뷰.
- Button: 버튼을 만들어서 사용자와 상호작용할 수 있게 해줌.
- `.padding(),` `.background()`, `.foregroundColor()`, `.cornerRadius()`: 스타일링 모디파이어로 각각 여백, 배경 색상, 텍스트 색상, 모서리 둥글기를 설정.

## 2. 상태 관리 (State Management)
SwiftUI에서는 UI의 상태를 `@State`, `@Binding`, `@ObservedObject`, `@EnvironmentObject` 등을 사용하여 관리합니다. 상태가 변경되면 자동으로 UI가 갱신됩니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    @State private var counter: Int = 0  // @State로 상태 변수를 선언

    var body: some View {
        VStack {
            Text("Counter: \(counter)")  // 상태 변수를 텍스트로 표시
                .font(.title)

            Button(action: {
                counter += 1  // 버튼 클릭 시 상태 변경
            }) {
                Text("Increment")
                    .padding()
                    .background(Color.green)
                    .foregroundColor(.white)
                    .cornerRadius(10)
            }
        }
        .padding()
    }
}
```

### 설명
- @State: 상태 변수를 선언하여 UI가 자동으로 갱신되도록 함. 버튼을 클릭할 때마다 `counter`의 값이 변경되고, `Text` 뷰가 자동으로 업데이트됨.

## 3. 데이터 바인딩 (Binding)
`@Binding`은 부모 뷰와 자식 뷰 간에 데이터를 공유할 수 있도록 하는 데 사용됩니다. 부모 뷰에서 상태를 관리하고 자식 뷰에서 이를 수정할 수 있게 합니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    @State private var isToggled = false  // 상태 변수 선언

    var body: some View {
        ToggleView(isToggled: $isToggled)  // @Binding을 이용하여 상태 전달
    }
}

struct ToggleView: View {
    @Binding var isToggled: Bool  // 부모 뷰의 상태를 바인딩

    var body: some View {
        Toggle("Is Toggled", isOn: $isToggled)  // Toggle의 상태를 바인딩된 값으로 설정
            .padding()
    }
}

```

### 설명
- @Binding: 부모 뷰의 상태를 자식 뷰에서 수정할 수 있도록 바인딩하는 속성 래퍼.
- Toggle: 스위치와 같은 기능을 제공하는 뷰로, `isOn`에 바인딩된 값을 통해 상태 변경.

 ## 4. 네비게이션 (Navigation)

SwiftUI에서 화면 간의 이동은 `NavigationView`와 `NavigationLink`를 사용하여 구현할 수 있습니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        NavigationView {  // 네비게이션을 위한 컨테이너
            NavigationLink(destination: Text("Next Screen")) {  // 링크를 눌렀을 때 이동할 화면
                Text("Go to Next Screen")
                    .font(.title)
                    .padding()
                    .background(Color.blue)
                    .foregroundColor(.white)
                    .cornerRadius(10)
            }
            .navigationTitle("Home")  // 네비게이션 바에 제목 추가
        }
    }
}


```

### 설명
- NavigationView: 네비게이션 컨테이너로, 화면 간 이동을 관리.
- NavigationLink: 사용자가 클릭하여 다른 화면으로 이동하는 링크.
- `.navigationTitle()`: 네비게이션 바에 화면 제목 추가.

- - ## 5. 리스트와 데이터 표시 (List & ForEach)

`List`와 `ForEach`는 데이터를 반복적으로 표시할 수 있도록 해주는 뷰입니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    let items = ["Apple", "Banana", "Cherry"]  // 목록 데이터

    var body: some View {
        List(items, id: \.self) { item in  // items 배열의 각 요소를 리스트로 표시
            Text(item)  // 각 항목을 텍스트로 표시
                .font(.title)
                .padding()
        }
    }
}



```

### 설명
- List: 데이터를 리스트 형태로 반복해서 표시하는 컨테이너.
- ForEach: 컬렉션의 각 요소에 대해 반복하여 뷰를 생성하는데 사용됩니다. List 내부에서 사용되어 데이터를 동적으로 표시함.

## 6. 조건부 뷰 표시 (Conditional Views)

SwiftUI에서는 조건에 따라 다른 뷰를 보여줄 수 있습니다. `if` 문을 사용하여 조건에 맞는 뷰를 동적으로 보여줄 수 있습니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    @State private var isLoggedIn = false  // 로그인 상태 변수

    var body: some View {
        VStack {
            if isLoggedIn {  // 조건에 맞는 뷰 표시
                Text("Welcome, User!")
                    .font(.title)
            } else {
                Button("Log In") {
                    isLoggedIn = true  // 로그인 상태 변경
                }
                .padding()
                .background(Color.green)
                .foregroundColor(.white)
                .cornerRadius(10)
            }
        }
        .padding()
    }
}

```

### 설명
- `if` 문을 사용하여 로그인 여부에 따라 다른 뷰를 표시.
- 로그인 상태가 `false`일 경우 로그인 버튼을, `true`일 경우 환영 메시지를 표시.

- ## 7. 알림 (Alerts)

SwiftUI에서는 `alert()`를 사용하여 사용자에게 알림을 표시할 수 있습니다.

### 예시 코드:
```swift
import SwiftUI

struct ContentView: View {
    @State private var showAlert = false  // 알림을 표시할지 여부

    var body: some View {
        Button("Show Alert") {
            showAlert = true  // 버튼 클릭 시 알림 표시
        }
        .alert("This is an alert", isPresented: $showAlert) {
            Button("OK", role: .cancel) {}  // 알림의 버튼
        }
        .padding()
    }
}

```

### 설명
- alert(): 사용자가 지정한 메시지와 함께 알림을 표시하는 모디파이어.
