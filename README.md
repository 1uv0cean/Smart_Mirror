# Smart_Mirror

#라즈베리파이 업데이트
```
sudo apt-get update -y && sudo apt-get upgrade-y
```

#한글 설정
```
sudo apt install fonts-unfonts-core
설정 ->  preferences -> Raspberry pi Configuration
Locale -> ko, UTF-8로 설정, Timezone Asia, Seoul로 설정
sudo apt remove ibus ibus-hangul
sudo apt install fcitx fcitx-hangul -y
sudo reboot
```


#Magic Mirror2 설치
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt install -y nodejs
git clone https://github.com/MichMich/MagicMirror
cd MagicMirror
npm install
cp config/config.js.sample config/config.js
```

#실행
```
npm run start   
종료할때는 ctrl+q
```

#화면 꺼짐 방지
```
sudo apt-get install xscreensaver
메뉴 -> Preferences -> Screensaver -> Mode에서 Disable Screen Saver 선택 
```

#화면 회전
```
터미널에서 sudo raspi-config 입력
Advanced options -> GL Driver -> GL (Full KMS) OpenGL desktop driver with full KMS 선택
sudo reboot
메뉴 -> 기본설정 -> Screen Configuration
Configure -> Screens -> HDMI-1 -> 방향 -> right
체크표시 클릭해 화면전환 후 OK 클릭
```

#wifi 절전모드 해제
```
sudo nano /etc/rc.local 
exit 0 바로 윗줄에 /sbin/iwconfig wlan0 power off 추가
ctrl+x -> y 입력후 엔터로 저장
sudo reboot
```

#날씨 API 키 발급 및 설정
``` 
https://openweathermap.org/ 가입 후 계정 - My API Keys 에서 API Key를 복사

geany MagicMirror/config/config.js

아래 내용 붙여넣기   
/* Magic Mirror Config Sample
 *
 * By Michael Teeuw https://michaelteeuw.nl
 * MIT Licensed.
 *
 * For more information on how you can configure this file
 * see https://docs.magicmirror.builders/getting-started/configuration.html#general
 * and https://docs.magicmirror.builders/modules/configuration.html
 */
let config = {
	address: "localhost", 	// Address to listen on, can be:
							// - "localhost", "127.0.0.1", "::1" to listen on loopback interface
							// - another specific IPv4/6 to listen on a specific interface
							// - "0.0.0.0", "::" to listen on any interface
							// Default, when address config is left out or empty, is "localhost"
	port: 8080,
	basePath: "/", 	// The URL path where MagicMirror is hosted. If you are using a Reverse proxy
					// you must set the sub path here. basePath must end with a /
	ipWhitelist: ["127.0.0.1", "::ffff:127.0.0.1", "::1"], 	// Set [] to allow all IP addresses
															// or add a specific IPv4 of 192.168.1.5 :
															// ["127.0.0.1", "::ffff:127.0.0.1", "::1", "::ffff:192.168.1.5"],
															// or IPv4 range of 192.168.3.0 --> 192.168.3.15 use CIDR format :
															// ["127.0.0.1", "::ffff:127.0.0.1", "::1", "::ffff:192.168.3.0/28"],

	useHttps: false, 		// Support HTTPS or not, default "false" will use HTTP
	httpsPrivateKey: "", 	// HTTPS private key path, only require when useHttps is true
	httpsCertificate: "", 	// HTTPS Certificate path, only require when useHttps is true

	language: "ko",
	locale: "en-US",
	logLevel: ["INFO", "LOG", "WARN", "ERROR"], // Add "DEBUG" for even more logging
	timeFormat: 24,
	units: "metric",
	// serverOnly:  true/false/"local" ,
	// local for armv6l processors, default
	//   starts serveronly and then starts chrome browser
	// false, default for all NON-armv6l devices
	// true, force serveronly mode, because you want to.. no UI on this device

	modules: [
		{
			module: "alert",
		},
		{
			module: "updatenotification",
			position: "top_bar"
		},
		{
			module: "clock",
			position: "top_left"
		},
		{
			module: "weather",
			position: "top_right",
			config: {
				weatherProvider: "openweathermap",
				type: "current",
				location: "Seoul",
				locationID: "1835847", //ID from http://bulk.openweathermap.org/sample/city.list.json.gz; unzip the gz file and find your city
				apiKey: "여러분의 API KEY",
				onlyTemp: true
			}
		},
		{
			module: "weather",
			position: "top_right",
			//header: "예보",
			config: {
				weatherProvider: "openweathermap",
				type: "forecast",
				location: "Seoul",
				locationID: "1835847", //ID from http://bulk.openweathermap.org/sample/city.list.json.gz; unzip the gz file and find your city
				apiKey: "여러분의 API KEY"
			}
		},
		{
			module: "newsfeed",
			position: "bottom_bar",
			config: {
				feeds: [
					{
						title: "COVID-19 관련 기사",
						url: "https://www.google.co.kr/alerts/feeds/00140512784757926026/17861186366920932868"
					}
				],
				showSourceTitle: true,
				showPublishDate: true,
				broadcastNewsFeeds: true,
				broadcastNewsUpdates: true
			}
		},
	]
};

/*************** DO NOT EDIT THE LINE BELOW ***************/
if (typeof module !== "undefined") {module.exports = config;}




apiKey: 발급받은 API KEY, location: Incheon , locationID: 1843564 로 수정
ctrl+s로 저장 후 창 닫기
cd MagicMirror
npm run start
날씨가 잘 표시되는지 확인
```

#뉴스 맞춤설정   
```
https://www.google.co.kr/alerts (구글에서 구글알리미 검색)으로 접속   
검색창에 원하는 검색어 입력 후 옵션 더보기   
수신 위치를 RSS 피드로 설정   
부채모양 오른클릭 - 링크 복사   
geany MagicMirror/config/config.js   
newsfeed 의 title, url 추가   
여러개를 쓰고싶으면 {} 로 구분   
```



