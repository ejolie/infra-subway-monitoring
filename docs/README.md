## 서비스 진단하기

#### 이론 공부 정리 
- 정확하고 정략적으로 측정 가능한 객관적인 기준으로 성능을 이야기 하는 것이 중요하며 이러한 기준을 메트릭 이라고 합니다.
- 메트릭 유형
  - 인지 로드 속도 : 페이지가 얼마나 빠르게 모든 시각적 요소를 화면에 로드하고 렌더링 할수 있는지 확인 
  - 로드 응답성 : 구성 요소가 사용자 상호 작용에 빠르게 응답하기 위해 페이지에서 필요한 JavaScript 코드를 얼마나 빠르게 로드하고 실행할 수 있는지를 확인
  - 런타임 응답성: 페이지 로드 후 페이지가 사용자 상호 작용에 얼마나 빨리 응답 할 수 있는지 확인 
  - 시각적 안정성: 페이지의 요소가 사용자가 예상하지 못한 방식으로 이동해 잠재적으로 상호 작용을 방해하는지를 확인
  - 원활함 : 전환 및 애니메이션이 일관된 프레임 속도로 렌더링 되고 다음 상태로 유동적으로 흐르는지 확인 

- 측정할 중요 메트릭
  - First Contentful Paint(최초 콘텐츠풀 페인트: FCP) : 페이지가 로드되기 시작한 시점부터 페이지 콘텐츠의 일부가 확면에 렌더링 될때까지의 시간을 측정 (실험실/현장)
  - Largest Contentful Paint(최대 콘텐츠풀 페인트: LCP) : 페이지가 로드되기 시작한 시점부터 가장 큰 텍스트 블록 또는 이미지 요소가 화면에 렌더링 될때까지의 시간을 측정 (실험실/현장) 
  - First Input Delay(최초 입력 지연: FID) : 사용자가 링크를 클릭하거나, 버튼을 탭하거나, 사용자 지정 JavaScript 기반 컨트롤을 사용하는 등 처음으로 상호 작용할때부터 해당 상호에 대한 응답으로 브라우저가 실제로 이벤트 핸들러 처리를 시작하기까지의 시간을 측정 (현장)
  - Time To Interactive(상호 작용까지의 시간 : TTI) : 페이지가 로드되기 시작한 시점부터 시각적으로 렌더링되고, 있는 경우 초기 스크립트가 로드되고 사용자 입력에 신속하게 안정적으로 응답할 수 있는 시점까지의 시간을 측정 (실험실)
  - Total Blocking Time(총 차단 시간, TBT) : 메인 스레드가 입력 응답을 막을 만큼 오래 차단되었을때 시점까지의 시간을 측정 (실험실)
  - Cumulative Layout Shift(누적 레이아웃 이동: CLS) : 페이지 로드 시작될때와 해당 수명 주기상태가 숨김으로 변경될때 사이에 발생하는 모든 예기치 않은 레이아웃 이동의 누락점수를 측정 (실험실/현장)

- 참고 사이트
- 웹 성능 예산
  - Quantity-based 
    - 압축된 리소트 최대 크기 200kb 미만등
  - Timing-base
    - FCP 3초 미만등 
  - Rule-based
    - 80점이상이어야 한다등 
[사용자 중심 성능 메트릭](https://web.dev/user-centric-performance-metrics/)

- 측정 결과 
  - [서울교통공사](https://www.seoulmetro.co.kr/kr/cyberStation.do)
  ```text
     mobile
     | FCP      | Speed Index | LCP     | TTI    | TBT    | CLS    | 성능
     | 6.5s     | 8.5s        | 6.9s    | 8.7s   | 1.010s | 0      |  30
     pc
     | 1.6s     | 2.3s        | 3.6s    | 2.0s   | 0.110s  | 0.014s |  69
  ```
  - [네이버지도](https://m.map.naver.com/subway/subwayLine.naver?region=1000)
  ```text
     mobile
     | FCP      | Speed Index | LCP     | TTI    | TBT    | CLS    | 성능
     | 2.1s     | 5.9s        | 7.5s    | 7.4s   | 0.490s | 0.03   |  51
     pc
     | 0.5s     | 2.4s        | 1.6s    | 0.7s   | 0s     | 0.006s |  89
  ```
  - [지하철노선도](https://mannue.kro.kr/path)
  ```text
     mobile
     | FCP      | Speed Index | LCP     | TTI    | TBT    | CLS    | 성능
     | 16.1s     | 16.1s      | 16.1s   | 16.2s  | 0.80s  | 0.04   |  45
     pc
     | 2.9s     | 2.9s        | 2.9s    | 2.9s   | 10s    | 0s     |  66
  ```
----------------
----------------

## STEP 2 : 부하 테스트 
#### 요구 사항
- [x] 부하 테스트
  - [x] 테스트 전제조건 정리
    - [x] 대상 시스템 범위
    - [x] 목표값 설정 
    - [x] 부하 테스트 시 저장될 데이터 건수 및 크기
  - [x] 각 시나리오에 맞춰 스크립트 작성
    - [x] 접속 빈도가 높은 페이지
    - [x] 데이터를 갱신하는 페이지
    - [x] 데이터를 조회하는데 여러 데이터를 참조하는 페이지 
  - [x] Smoke, Load, Stress 테스트 후 결과를 기록 

- 부하 테스트 소개 
  - k6 설치 
  ```text
     sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
     echo "deb https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
     sudo apt-get update
     sudo apt-get install k6
  ```
  - 시나리오 상 두 번의 요청이 있고 총 latency 목표값을 0.2s 라고 가정한다면
    - T(VU iteration) = (요청수 * http_req_duration) + a 
    - T = (2 * 0.2) + 0 = 0.4s
  
  - 목표 rps = (VUser * T) / 요청수
    - VUser = (100 * 0.4) / 2 = 20
  
  - 일반적으로 부하 테스트는 30~ 2시간 정도를 권장 

-----------
-----------

## 3단계 - 로깅, 모니터링 

#### 요구 사항
- [x] 애플리케이션 진단하기 실습을 진행해보고 문제가 되는 코드를 수정
- [x] 로그 설정하기
- [x] Cloudwatch 로 모니터링


- 요구사항 설명
  - 로그 설정하기
    - [x] Application Log 파일로 저장하기
      - 회원 가입, 로그인 등의 이벤트에 로깅을 설정
      - 경로 찾기 등의 이벤트 로그를 JSON 으로 수집
    - [ ] Nginx Access Log 설정하기
  
  - Cloudwatch 로 모니터링
    - [x] Cloudwatch 로 로그 수집하기
    - [x] Cloudwatch 로 메트릭 수집하기
    - [x] USE 방법론을 활용하기 용이하도록 대시보드 구성 
  
  - 로깅
    - Avoid side effects
    - Be concise and descriptive
      - logging 에는 데이터와 설명이 모두 포함되어야 한다.
    - Log method arguments and return values
      - 메소드의 input 과 output 을 로그로 남긴다.
    - Delete personal information
      - 개인정보를 남기지 않는다.
    
    - level
      - ERROR : 예상하지 못한 심각한 문제가 발생하여 즉시 조사해야 함
      - WARN : 로직상 유효성 확인, 예상 가능한 문제로 인한 예외처리 등을 남김, 서비스는 운영될수 있지만, 주의해야 함
      - INFO : 운영에 참고할만한 사항으로, 중요한 비지니스 프로세스가 완료됨
      - DEBUG / TRACE : 개발 단계에서만 사용하고 운영 단계에서는 사용하지 않음 