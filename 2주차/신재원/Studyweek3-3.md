데이터 수집 

빅데이터는 대부분의 경우 확장성이 높은 분산 스토리지에 저장 

분산 형의 데이터베이스도 있지만 , 객체 스토리지가 유명 , Hadoop - HDFS , 클라우드 서비스 - S3 

객체 스토리지에서의 파일 읽고 쓰기는 네트워크를 거쳐서 실행. 

빅데이터로 자주 다루는 것은 시계열 데이터. 시간과 함께 생성되는 데이터 

시간이 계속 지날수록 데이터의 크기가 커지기 때문에 처리하기 쉽도록 준비해둘 필요가 있다. 

수집한 데이터를 가공하여 집계 효율이 좋은 분산 스토리지를 만드는 일련의 프로세스를 데이터 수집이라고 함. 

1. 너무 작은 데이터는 모아서 기록 

2. 너무 큰 데이터는 분할해서 기록  TB 이상 


데이터 전송 

벌크 형과 스트리밍 형 두가지 존재 

벌크형 - ETL 서버를 통해 구조화된 데이터 처리에 적합한 데이터 웨어하우스를 위한 ETL 도구와 벌크 전송 도구 를 사용 

스트리밍 형 - 계속해서 작은 데이터가 전송 - 메세지 배송 (높은 처리 성능 요구)

작은 데이터 쓰기에 적합한 NoSQL 이용. 

분산 스토리지에 메세지를 직접 쓰는 것 X , 메세지 큐와 메세지 브로커등의 중계 시스템에 전송할 수 있다. 


MQTT 

MQ Telemetry Transport 는 TCP/IP 를 사용하여 데이터를 전송하는 프로토콜의 하나. 일반적으로  pub-sub 형 메세지 배송 구조를 가짐 

Topic : 메세지를 송수신 하기 위한 대화방  topic 구독시 대화방 참여


메세지 브로커 

메세지 배송에 의해 보내진 데이터를 분산 스토리지에 저장할 때는 주의가 필요하다. 

데이터양이 적을때에는 문제가 되진 않지만, 쓰기의 빈도가 증가함에 따라 디스크 성능의 한계에 도달. 


만일 쓰기 성능의 한계에 의해 오류가 발생하면, 대부분의 경우 클라이언트는 메세지를 재전송하려고 한다. 

하지만 부하만 걸릴 뿐 해결되지 않는다. 

빅데이터 메세지 싯스템에서는 종종 데이터를 일시적으로 축적하는 중산층이 설치된다. 이것을 메세지 브로커라고 한다. 

메세지 브로커는 데이터(메세지)의 쓰기 읽기 속도를 조정하기 위한 완충 부분이며, 푸시 형에서 풀 형으로 멧헤지 배송의 타이밍을 변환한다. 

메세지 브로커에 데이터를 넣는 것을 생산자, 꺼내오는 것을 소비자라고 한다. 


메세지 브로커에 써넣은 데이터는 복사되어 여러 경로로 분기시킬 수 있다. 이것을 메세지 라우팅이라고 한다. 


신뢰성 

End to ENd의 신뢰성은 중복도 결손도 없이 끝에서 끝까지 메세지 배송을 하는 것이고 쉬운 일이 아니다.  성능 과 신뢰성은 항상 Trade-OFF 관계 임을 명심할 것 

그 중에서도 빅데이터는 신뢰성 보다 효율이 중시된다. at least once를 보장하지만 , 중복 제거는 하지 않는 것이 표준적.  


중복을 고려한 시스템 설계 

일반적으로 중복의 가능성이 있다고 생각하는 것이 좋다. 빅데이터를 다루는 시스템은 매우 높은 성능을 요구하기 때문에 아주 작은 중복은 무시하는 경향이 있다. 

실제로 큰 문제가 없다.  아무리 신뢰성이 중시되는 경우에는 스트리명 형 메세지를 피하는 것이 좋다. 예를 들어 과금 데이터. 


시계열 데이터 

이벤트 시간과 프로세스 시간 사이의 이벤트 시간의 차이가 문제를 일으킴 



