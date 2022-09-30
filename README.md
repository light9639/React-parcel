# :zap: Parcel로 React 개발환경 구축하기
:octocat: 바로가기 : https://light9639.github.io/React-parcel/
![localhost_8080_](https://user-images.githubusercontent.com/95972251/191442624-70ca8d5a-dafb-44b4-9ee2-66446ef00ed6.png)
**✨React Parcel 템플릿✨**
## 📋 템플렛 생성법
### 📦 package.json 생성
 - 폴더를 생성하고 아래 명령어로 package.json 파일을 생성한다.

    ```bash
    yarn init -y
    ```

### 🚅 React 설치
 - React 설치

    ```bash
    yarn add react react-dom
    ```

### 🚅 Bable 설치
 - 파셀에 내장되어 있기 때문에 필요 없다! .babelrc 파일을 만들어서 직접 설정할 수도 있지만 안 해도 Parcel이 알아서 작동한다.

### 🚅 Parcel 설치
 - 웹팩은 로더와 플러그인을 설치하고 세팅해야 하지만 파셀은 전부 필요 없고, 기본적으로 알아서 해주는 편.
 
    ```bash
    yarn add parcel
    ```

### 🚅 Parcel 플러그인 설치
 - parcel-transformer-interpolate-html - html 파일에서 %ENV% 같은 템플릿 구문 사용 가능. 꼭 필요한 건 아니지만 CRA 기본 예제 파일에서 %PUBLIC_URL%을 사용하기 때문에 이를 변환하기 위해 설치. 파셀 v1에는 해당 기능을 하는 플러그인이 있었는데 v2엔 없어서 만들었다.
 - parcel-reporter-clean-dist - 빌드 전 빌드 폴더 안 내용을 정리하고 빌드

    ```bash
    yarn add parcel-transformer-interpolate-html parcel-reporter-clean-dist
    ```

### 📦 .parcelrc 생성
 - 만약 플러그인을 설치 안 했다면 정말 아무 설정 파일도 없었겠지만 아쉽게도 플러그인을 설치하는 바람에 설정 파일이 하나 필요하게 됐다. 

    ```bash
    {
      "extends": "@parcel/config-default",
      "transformers": {
        "*.{html,htm}": ["parcel-transformer-interpolate-html", "..."]
      },
      "reporters": ["parcel-reporter-clean-dist", "..."]
    }
    ```

### 📦 .env 생성
 - .env는 dotenv의 환경변수 파일이다. 파셀에 dotenv가 내장되어 있어 자동으로 .env 파일을 읽어 환경변수를 설정해준다.

    ```bash
    PUBLIC_URL=.
    ```
    
 - 이렇게 환경변수를 지정해두면 parcel-transformer-interpolate-html가 %PUBLIC_URL%을 .으로 교체한다.

### ✒️ React 컴포넌트 작성
 - 그냥 CRA에서 기본으로 만들어주는 예제 파일을 그대로 복사한다. (src, public 폴더) 단, 예제 소스에서는 web-vitals을 사용하므로 이걸 추가로 설치해야 한다.

    ```bash
    yarn add web-vitals
    ```

 - 그리고 진입점이 index.js이고 번들링 후 index.html에 삽입되는 웹팩과 다르게 파셀은 진입점이 index.html이다. index.html에 index.js를 추가해야 파셀이 파일을 타고타고 가면서 알아서 번들링 한다.

    ```bash
    <script type="module" src="../src/index.js"></script>
    ```
 - public/index.html 파일을 열고 위 부분을 </body> 위에 추가한다.

### ➕ package.json에 source, scripts 추가
 - "main" 필드가 있으면 파셀은 라이브러리로 인식하기 때문에 지워야 한다.

    ```bash
    // "main": "index.js",
    "source": "public/index.html",
    "scripts": {
      "start": "parcel -p 3000 --open",
      "build": "parcel build --dist-dir build --no-source-maps"
    }
    ```

 - 그리고 scripts에 진입점(public/index.html)을 추가할 수 있지만 이렇게 source 필드를 추가함으로써 진입점을 설정해줄 수 있다.

 - yarn start - 개발 서버를 실행해 프로젝트를 바로 확인. 주소는 http://localhost:3000/
 - yarn build - 빌드. 결과물은 build 폴더에 생성

## **:paperclip: 출처**
- 출처1 : <a href="https://ko.parceljs.org/recipes.html">링크1</a>
- 출처2 : <a href="https://blog.joyfui.com/1248">링크2</a>
