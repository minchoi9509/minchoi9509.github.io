---
title:  "크롬 확장 프로그램 만들기"
excerpt: "크롬 확장 프로그램 만들기 - To DO + Calendar 앱"

categories: TIL
tags: [TIL]
---


* 웹팩 (Webpack)
	* HTML 파일 로딩 시 필요한 javaScript, css, img 자원들을 한번에 불러오기 위한 자바스크립트 파일 > 수 많은 network request를 하나의 Request로 만들어서(=하나로 만드는 과정을 번들링) 브라우저 성능을 높일 수 있음. 
	* index.html에서 설정되어 있는 `/dist/build.js` 이 부분이 Webpack을 통해 번들링된 결과물이 저장되는 장소
	* `webpack.config.js` 웹팩이 프로젝트 빌드 시 참고하는 설정 파일로 핵심 파일이라고 생각 하면 됨. 	
	
---
	
Uncaught TypeError: Failed to resolve module specifier "vue". Relative references must start with either "/", "./", or "../". 
* 프로젝트 설정 중에 만난 문제. 보일러 플레이트 프로젝트(Vite)를 그냥 이용했는데 팝업 실행이 안됐다. 선택했던 이유는 그냥 참고했던 사이트에서 사용 중이기 때문이었는데 그러다 보니까 어떻게 문제 해결이 안될 것 같았다. 그래서 그냥 webpack-simple 형태로 우선 진행했다. 
