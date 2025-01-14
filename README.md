# 2024-NC2-A14-WeatherKit
## 🎥 Youtube Link
[WeatherKit](https://youtu.be/ebXouR47Qiw?si=t6h6qLLPst74z5T1)

## 💡 About WeatherKit

> # **위치를 기반으로 여러 종류의 기상 데이터를 제공하는 프레임 워크**
> 
> - Current weather -> 체감 온도, 습도, 자외선, 이슬점
> - Minute forecast -> 강수 확률, 강수 강도
> - Hourly forecast -> 체감 온도, 습도, 자외선, 이슬점
> - Daily forecast -> 최고 기온, 최저 기온, 달 형상, 강수량, 일출, 일몰
> - Weather alerts -> 지역, 위험도
> - Historical weather -> 체감 온도, 습도, 자외선, 이슬점

## 🎯 What we focus on?

> # Daily forecast - DayWeather
> 
> - 옷차림 추천 / 달 모습 보여주기 / 비 오는 날 알려주기 / 선크림 바르라고 알려주기 등…
> - 날씨가 안 좋을 때 나가기 싫어하는 구름과 폰
> - 날씨 앱과 일정 기록 앱을 번갈아가며 보는 것이 불편했던 상황에 Engage
> - 일정을 정하는 기능과 날씨를 확인하는 기능을 함께 제공하여 불편함을 줄여보자!
> - 일정에 큰 영향을 주는 날씨 정보는 무엇일까?
> - 날짜별로 날씨(대표), 최고 기온, 최저 기온, 강수 여부, 자외선, 풍속 정보 제공 필요

## 💼 Use Case

> **WeatherKit을 활용해 [ 날씨 확인과 일정 관리를 통합 ]시켜**
> 
> 
> **[ 선호하지 않는 날씨인 날 일정을 잡기를 꺼려하는 사용자 ] 가**
> 
> **[ 날씨를 고려해 일정을 잡을 때 편리 ] 하게 해주는 캘린더**
>

## 🖼️ Prototype

![prototype1](https://github.com/YunKi-H/2024-NC2-A14-WeatherKit/assets/80469941/dd1636e9-faa5-4c8b-b3fa-45ef321b8a8c)
![prototype2](https://github.com/YunKi-H/2024-NC2-A14-WeatherKit/assets/80469941/f13276cd-9738-4b73-92d1-c5fd07726907)


- 사용자의 현재 위치 기준 오늘포함 이후 열흘간 날씨 정보를 보여줌 (위치 권한 사용 허용시)
- 날씨 정보가 있는 날짜의 주의할만한 특이사항을 알려줌 (강수, 추위, 더위, 일교차, 강풍)
- 일정이 있는 날에는 작은 원형의 인디케이터가 표시됨
- 사용자가 약속을 잡기 좋은 날씨인 날을 골라서 일정을 잡기 수월해짐

## 🛠️ About Code

```swift
import WeatherKit // **중요**
import CoreLocation.CLLocation // 원하는 일부만 import 하고 싶을 때

/// Apple 에서 제공하는 날씨 정보 총괄 객체
/// - WeatherService 객체를 통해 WeatherKit 의 정보를 불러올 수 있음
/// - WeatherService 내부의 .shared 프로퍼티를 사용해야 함 (다수 객체 생성 방지)
let weatherService: WeatherService = WeatherService.shared

/// 날씨 정보를 받아올 위치 (예시: C5 좌표)
let location: CLLocation = CLLocation(latitude: 36.0135, longitude: 129.3263)

Task {
	/// weatherService 객체의 weather(...) 메서드를 사용해 날씨 정보를 받아올 수 있음
	/// .daily 외에도 .alerts, .current, .availability, .minute, .hourly 등 원하는 종류의 데이터 호출 가능
	/// 외부 데이터 호출이라 비동기 함수이고, Error 발생 가능하기 때문에 [try, await] 키워드 사용 필요
	let weatherData = try? await weatherService.weather(for: location, including: .daily)
	/// 받아온 Forecast<DayWeather> 형식의 데이터에서 사용하기 편하게 [DayWeather] 형식(배열)으로 변환
	let dayWeathers = weatherData.forecast
	
	/// 데이터 배열에서 원하는 정보를 뽑아 사용
	for dayWeather in dayWeathers {
		print(dayWeather.lowTemperature.value) // 최저기온
	}
}
```

## 🛠️ Before you Build

Before you run this project in Xcode:
1. Download, install, and launch the latest version of Xcode.
2. In Safari, visit the [Certificates, Identifiers, and Profiles](https://idmsa.apple.com/IDMSWebAuth/signin?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&path=%2Faccount%2Fresources%2F&rv=1) section of the developer website.
3. Select Identifiers and click the Add button to create a new App ID for WeatherPlan. Follow the steps until you reach the Register an App ID page.
4. For the Bundle ID, select Explicit, and enter a unique bundle identifier. Use a reverse-DNS format for your identifier, as [Preparing your app for distribution](https://developer.apple.com/documentation/xcode/preparing-your-app-for-distribution) describes.
5. Click the App Services tab, and select the WeatherKit checkbox.
6. Complete the App ID creation process.
7. Wait 30 minutes while the service registers your app’s bundle ID.
8. In Xcode, from the Project navigator, select the WeatherPlan project and click the Signing & Capabilities tab.
9. Enter the unique bundle ID from step 4 in the Bundle Identifier field of the WeatherKit target.
10. From the scheme pop-up menu, select a run destination.
11. If necessary, click Register Device on the Signing & Capabilities tab to create the provisioning profile.
12. On the toolbar, click Run, or choose Product > Run.
