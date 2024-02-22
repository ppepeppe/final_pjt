

# 예제 - 쇼핑몰


# 서비스 시나리오

기능적 요구사항
1. 고객이 메뉴를 선택하여 주문한다
1. 주문이 되면 결제 한다
1. 결제가 완료되면 재고 내역에서 수량을 감소시킨다
1. 결제가 완료되면 배송을 시작한다
1. 고객이 주문을 취소할 수 있다
1. 고객이 주문을 취소하면 결제취소한다
1. 결제를 취소하면 배송을 취소한다
1. 결제를 취소하면 재고 내역에서 수량을 증가시킨다
1. 재고담당자는 재고를 추가할수있다

비기능적 요구사항
1. 트랜잭션
    1. 결제가 되지 않은 주문건은 아예 거래가 성립되지 않아야 한다  Sync 호출 
1. 장애격리
    1. 상점관리 기능이 수행되지 않더라도 주문은 365일 24시간 받을 수 있어야 한다  Async (event-driven), Eventual Consistency
    1. 결제시스템이 과중되면 사용자를 잠시동안 받지 않고 결제를 잠시후에 하도록 유도한다  Circuit breaker, fallback
1. 성능
    1. 고객이 자주 상점관리에서 확인할 수 있는 배달상태를 주문시스템(프론트엔드)에서 확인할 수 있어야 한다  CQRS
    1. 배달상태가 바뀔때마다 카톡 등으로 알림을 줄 수 있어야 한다  Event driven

![Alt text](img/event_storming.png)


- 초기 재고량 설정
![Alt text](./img/image.png)

- 주문
![Alt text](./img/image-6.png)

- kafka 확인
![Alt text](./img/image-2.png)

- 재고변화 확인

![Alt text](./img/image-3.png)

- 주문취소 kafka 확인
![Alt text](./img/image-4.png)

- 재고 변화 확인
![Alt text](./img/image-5.png)
- apigateway 확인
![Alt text](./img/image-7.png)

## 클라우드 배포 - conatiner 운영
- 패키지 된 jar 파일을 기반으로 한 이미지 빌드
- 빌드 된 이미지를 docker hub repo에 push
- 이미지를 활용하여 실행

## hpa
![Alt text](./img/image-8.png)

![Alt text](./img/image-9.png)

## 무정지 배포

![Alt text](./img/image-10.png)

availability 50% 로 보아 50 %의 중단이 있었음

readinessPorbe 설정 추가 후
```
readinessProbe:    # 이부분!
            httpGet:
              path: '/orders'
              port: 8080
            initialDelaySeconds: 10
            timeoutSeconds: 2
            periodSeconds: 5
            failureThreshold: 10

```

![Alt text](./img/image-11.png)
availability 100% 로 보아 50 % 모두 가동한 것을 알 수 있음


## PVC
![Alt text](./img/image-12.png)

## configmap

