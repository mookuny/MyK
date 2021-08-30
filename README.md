# MyKiwoom
파이썬으로 만든 키움증권 시스템 트레이딩 프로그램입니다.
- ![화면 캡처 2021-08-31 045712](https://user-images.githubusercontent.com/78009194/131397706-0d8f6f0f-234b-48a6-9811-4fcb2d38edbf.png)

1. day 폴더내에는 단기, 중기, 장기 전략용 백테스터와 업데이터가 포함되어 있습니다.
- 백테스터는 각각의 전략을 테스트하고 주요종목을 선별하는 목적의 파일입니다.
- 업데이터는 장마감 후 각 전략을 운영하기 위한 DB 갱신용입니다.

2. tick 폴더내에는 초당 틱데이터 수집코드 및 백테스터 두가지, 백파인더가 포함되어 있습니다.
- backtester.py
- Total과 BackTester에 넘겨진 num 인자는 리스트이며 각 요소 또한 리스트입니다.
- 그 값이 [0., 10., 1, 0.1] 일 때를 예를 들어 설명하면
- 변수를 0부터 10까지 1단위로 1차 백테스트하고 최고수익인 변수를 골라내어
- 골라낸 변수값의 전후를 0.1단위로 다시 2차 백테스트하여 최고수익인 변수를 찾아냅니다.
- 하나의 변수를 가지고 이렇게 백테스트를 하고 난 후 최고수익인 변수를 정한 다음
- 해당 변수값을 고정시키고 다음 변수를 백테스팅하는 방식입니다.
- 리스트[2]와 리스트[3]이 같은 값일 경우 1차 백테스트만 진행됩니다.
- 특정 변수의 수치를 임시로 고정할려면 [2.5, 2.5, 0, 0] 이렇게
- 두개는 고정값 뒤에 두개는 0으로 설정하면 됩니다.
- 백테스트한 모든 데이터는 DB에 저장되고
- 수익급합계 그래프 또한 당일날짜 + 변수값들로 파일명을 정해서 png파일로 저장됩니다.
- 여기서 조금 더 응용하면 경우의 수 모두를 백테스트할 수 있는 코드도 됩니다.
- 수십코어 이상의 컴퓨터를 보유하고 계신분은 workcount = int(last / 6) + 1 이부분의 숫자 6를
- 코어수 만큼 늘려서 모든 경우의 수를 백테스트 해보십시오.
- 단, 시간이 얼마나 걸릴지는....저도 모릅니다. ㅎㅎ;;
- 64비트 파이썬을 사용하면 시간이 더 단축되겠죠.
- self.ccond는 조건검색식 진입 이후 경과한 시간(초) 계산용입니다.
- self.indexn은 현재 인덱스의 값이 아닌 번호이며, 특정기간 특정칼럼의 평균값 계산용입니다.
- self.indexb는 매수시점 인덱스의 번호이며, 매수이후 특정칼럼의 최고값 계산용입니다.
- self.csell은 매수시 0, 매도조건을 만족하면 +1됩니다. 매도조건 지속만족 시 청산용입니다.
- 변수의 순서에 따라 최적치가 달라질 수 있습니다.
- backfinder.py는 backtester의 역개념 코드라고 생각하시면 됩니다.
- backtester가 변수값들을 변경해가며 최적치를 찾는 작업이라면,
- backfinder는 미래의 특정시간 이내에 수익조건을 만족하는 현재시점의 변수값들을 찾는 작업입니다.
- 작성된 코드는 저장된 틱데이터를 불러와서
- 현재가 대비 이후 300초 이내에 최고가가 2%이상 상승한 시점의 데이터들이 어떠했는지 기록하는 코드입니다.
- 지금은 초단위 코드이지만, 일봉으로 해서 3일이내 10%이상 상승한 시점의 데이터들도 검토할 수 있겠죠.
- backtesterm.py
- 변수들의 값을 고정하여 백테스트할 수 있는 코드입니다.

3. login 폴더는 버전처리와 자동로그인 설정 파일입니다.
- versioupdater.bat는 버전 업그레이드용 파일입니다
- autologin1.py는 user.txt 파일에서 설정한 첫번째 계정 자동로그인 설정이며
- autologin2.py는 두번째 계정 자동로그인 설정입니다.
- "고객아이디" 및 "공동인증서" 관련 오류창이 뜰 경우 "고객아이디 저장"를 체크 하신 후
- 수동으로 로그인을 한번 한 후에 재실행하시면 오류가 해결됩니다.

4. trader 폴더는 자동매매 코드입니다.
- 화면해상도 3440 x 1440에 최적화되어 있습니다.
- 실시간 주식체결 수신부에서 BuyStrategy, SellStrategy 중 전략과 관련된 코드는 비공개상태입니다.

5. utility 폴더는 각종 설정 및 static 함수 파일입니다.
- UI 색상 및 글꼴을 변경하실려면 setting.py를 변경하시면 됩니다.

6. 배치파일 실행방법
- 저는 컴퓨터가 8시 45분에 켜지고 48분에 B0_backtester.bat 파일이 실행되고
- 51분에 A0_trader.bat 파일이 실행됩니다.
- 윈도우 작업스케줄러에 적당한 시간으로 설정하여 두개의 배치파일을 관리자 권한으로 실행하시면 됩니다.
- 장이 열리지 않을 경우 9시 1분에 모든 프로그램 및 컴퓨터가 종료되도록 설정되어 있습니다.
- 이외의 배치파일은 파일 하나씩 실행되는 파일이며 배치파일 마지막에 pause가 추가되어 있어
- 코드 수정 후 오류 검사용 배치파일입니다.

<설정방법>

- 기본 환경은 miniconda 32bit python 3.9버전입니다.
- https://repo.anaconda.com/miniconda/Miniconda3-py39_4.10.3-Windows-x86.exe
- 미니콘다 환경에서 추가로 설치할 라이브러리는 다음과 같습니다.
- pip install psutil pyqt5 pandas pyttsx3 pyqtgraph matplotlib BeautifulSoup4 lxml python-telegram-bot

1. login/user.txt 파일에 계정 두개 설정


3. utility/setting.py 파일에 각종 경로 설정


4. 모든 배치 파일을 열어서 두번째줄 경로 수정


5. B1_versionupdater.bat 관리자권한으로 실행


6. A1_autologin1.bat 관리자권한으로 실행


7. A2_trader.bat 실행
- ![화면_캡처_2021-08-12_133730](https://user-images.githubusercontent.com/78009194/131397716-e9e53d51-3334-4e7d-9eb1-0d40e3f2f155.png)
- 설정이 올바르게 되었다면, 데이터베이스에 필요한 테이블이 만들어지고
- OPENAPI 로그인 후 계정정보 및 지수차트 조회, VI발동해체등록, 장운영시간등록, 업종지수등록까지 완료한 후
- 정상적으로 프로그램이 구동됩니다. 여기서부터 장운영시간 알림을 받아 프로그램이 단계별로 작동됩니다.
- 혹, 장중에 프로그램을 구동하면 15시 20분까지 장운영시간 알림을 받지 못하므로
- 시스템설정탭에 "장운영상태" 버튼을 클릭하면 수동으로 장운영상태로 변경됩니다.

7. 시스템설정탭으로 가서 텔레그램을 설정합니다.
- ![화면_캡처_2021-08-26_195057](https://user-images.githubusercontent.com/78009194/131397719-f53e2355-149f-4495-b21b-0f91b7d38eb5.png)
- 모니터 해상도가 작아서 안보이시는 분은
- trader/window.py self.setWindowFlags(Qt.FramelessWindowHint)
- trader/window.py self.ButtonClicked_4(2)
- 두 부분을 주석처리하고 실행하시면 UI를 움직일 수 있습니다.
- 해상도 작으신 분들은 나중에 설명할 "차트유형 변경" 및 "창크기변경" 버튼을 클릭하지 마십시오.
- ui가 안보이는 영역으로 가버립니다. ㅎㅎ;;

8. 시스템종료 버튼을 눌러 프로그램을 종료합니다.


9. 키움증권 HTS를 실행하여 조건검색식 0번 1번 두개를 만듭니다.
- 0번은 시가총액10조원이하, 관리종목 제외, 거래대금상위 100종목
- 1번은 시가총액10조원이하, 관리종목 제외

10. A2_trader.bat 재실행합니다.
- 구동 완료 후 텔레그램으로 "자동매매 시스템을 시작하였습니다."라는
- 메세지가 수신되면 텔레그램 정보가 올바르게 설정된 겁니다.

11. 로그 및 설정 위젯 우측 하단에 버튼이 세개있습니다.
- 차트탭변경 버튼은 지수차트 및 종목차트의 탭 상태를 변경합니다.
- 차트유형변경 버튼은 두종목차트 >> 한종목차트 >> 복기차트 순으로 변경됩니다.
- 창크기변경 버튼은 UI의 크기를 최소모드, 최대모드로 변경합니다.

12. 각종목록의 첫번째 칼럼 클릭시 차트1 및 호가창1이 표시되고
- 두번째 칼럼 클릭시 차트2 및 호가창2가 표시됩니다.
- 복기차트 모드시 데이터베이스에 저장된 체결목록을 불러와서 차트에 매수 및 매도가격를 표시합니다.

13. 거래목록탭에서 달력의 날짜를 클릭하면 해당 날짜의 거래목록을 불러옵니다.


14. 수익현황 탭에서는 기간별 집계를 구분하였습니다.


15. 테스트모드 ON으로 변경하면 로그인 후 아무런 명령이 실행되지 않고
- 시스템설정에 버튼들을 누르면 각각의 명령이 하나씩 실행됩니다.
- 모의투자 ON 상태는 실서버에 접속해서 테스트 하지만, 실제 매매가 이뤄지지 않고 매수 매도를 기록합니다.
- 모의투자 OFF 상태시 실제 매매 주문이 전송됩니다.

16. 차트는 마우스 좌클릭으로 드래그하면 선택한 영역이 줌인됩니다.
- 줌아웃은 마우스 우클릭하시면 됩니다.
- 차트 캔들 영역 좌측 하단은 십자선 기준가격 및 전일대비 십자선 등락율이 표시됩니다.

17. 호가창
- ![화면_캡처_2021-07-04_144709](https://user-images.githubusercontent.com/78009194/131397709-915b3d1f-f6a7-4681-aed6-3c60e8866d5c.png)
- 호가창 우측 상단의 금액을 선택하여 매수주문 영역에 마우스를 클릭하면 매수주문이 전송됩니다.
- 호가창 좌측 상단의 퍼센트를 선택하여 매도주문 영역에 마우스를 클릭하면 매도주문이 전송됩니다.
- 시장가 매도수와 매도수 취소 버튼은 버튼의 설명 그대로 명령이 실행됩니다.
- 미체결 수량이 남아 있으면 매도주문, 매수주문 영역에 남은 수량이 표시됩니다.
- 키움증권 8282 호가주문창과 거의 동일하지만, 중복 및 복수 주문은 되지 않으니 착오없으시길 바랍니다.

18. 텔레그램 사용자 버튼은 아래와 같이 설정되어 있습니다.
- ![P20210812_160357000_127FD7AC-B059-4E5C-BAEA-8401B325A423](https://user-images.githubusercontent.com/78009194/131397701-9b2e7d97-623e-471c-8582-bfbcf61e97c4.jpg)
