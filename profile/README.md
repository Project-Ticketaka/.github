# Ticketaka

![Ticketaka](https://user-images.githubusercontent.com/80504636/231347927-9bbd712f-5a26-4619-9913-f871e4cde610.png)

### 🔗 ~~**[배포물 바로가기](https://ticketaka.shop)**~~

<br>

**🗂 GitHub Repository**
- 👨‍👩‍👧‍👦 [Backend-Member](https://github.com/Project-Ticketaka/Backend-Member)

- 🎫 [Backend-Reservation](https://github.com/Project-Ticketaka/Backend-Reservation)

- 🎬 [Backend-Performance](https://github.com/Project-Ticketaka/Backend-Performance)

- 📊 [Data](https://github.com/Project-Ticketaka/Data)

- 🖼 [Frontend](https://github.com/Project-Ticketaka/Frontend)

<br/>

**목차**

- [프로젝트 소개](#intro)
- [기술 스택](#stack)
- [아키텍처](#architecture)
- [구현 기능](#function)
- [회고](#retrospective)

<br>

# <span id="intro">📋 프로젝트 소개</span>

MSA를 활용한 공연 예매 시스템 구축 프로젝트

<br>

# <span id="stack">🚀 기술 스택</span>

| Java | SpringBoot | Python |
| :---: | :---: | :---: |
| <img width="50" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231357444-f19f60b4-b037-4243-9fd0-7335f805e1ab.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356152-2eb8bc40-a505-41db-8b48-34cb08307872.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356153-9515afbe-5f22-4639-98ee-d60992e5627c.png"> | 

| TypeScript | React | Node |
| :---: | :---: | :---: |
| <img width="50" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356155-820c42ce-6b3e-483f-9a81-53a534441fc1.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356159-a1ed8d6a-1945-4c86-8a82-41295fad3d40.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356163-4ab93b84-6ca2-4872-a068-3d6b1d89d7e8.png"> |


| MariaDB | Redis | RabbitMQ |
| :---: | :---: | :---: | 
| <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356164-ed120ca3-c933-41c3-a9a8-bb8513a343d3.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231358877-6a4ccb13-21d4-4567-a493-cbde6226cd0e.png"> | <img width="50" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356154-4dd57ef2-a2e8-467d-a9ec-dda5860ebbfb.png"> |

| Docker | Kubernetes | istio | AWS | 
| :---: | :---: | :---: | :---: |
| <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356165-0f68a283-1f64-433b-939e-d4e582530535.png">  | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356166-2fd54dfa-b3c7-42b3-97f3-955a27b5e550.png"> | <img width="40" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231356168-0bfec787-354d-4e22-bcd4-90ff9fde5215.png"> | <img width="60" alt="Untitled 1" src="https://user-images.githubusercontent.com/80504636/231358883-af99f506-36b0-4912-961c-7f7d5bcc1e12.png">


<br>

# <span id="architecture">🏗 아키텍처</span>

## 클라우드 아키텍처
![aws](https://user-images.githubusercontent.com/80504636/231353998-4c27ff7f-23ba-4d22-b134-4545ddfdc6fe.png)

## 쿠버네티스 아키텍처
![k8s](https://user-images.githubusercontent.com/80504636/231354002-992cdc30-c516-4ad2-a3d3-2244989df91f.png)

# <span id="function">🧩 구현 기능</span>

## 기능 1 ) 트래픽 처리
- ### 🎫 예매 시스템
    
    ![reservationSystem](https://user-images.githubusercontent.com/80504636/231376541-9e390ce9-15b1-4de9-8b4f-40d95b11c1b9.png)

    - Redis를 사용하여 중복 예매를 방지하고 지연시간을 낮춰 안정적인 예매가 가능한 시스템을 구축
    
    - [상세 정보](https://github.com/Project-Ticketaka/Backend-Performance)

<br>

- ### 📧 메시지큐

    ![mqarchitecture](https://user-images.githubusercontent.com/80504636/231376535-70577f4c-5f1c-43df-8f00-9d666c926019.png)

    - 메일 발송을 위한 메시징 큐
    
    - RabbiMQ Cluster를 배포하는 Custom Resource 사용
    
    - Exchange, Binding, Key, Queue등의 정보를 생성하는 `definitions.json` 를 Configmap으로 정의
        - 메일 정보를 가지는 `mail.queue`, 회원가입 인증 메일 정보를 가지는 `valid.queue`  생성
        
        - RabbiMQ SHA256알고리즘을 사용하여 user, password 생성
    
    - 별도의 라우팅 패턴 없이 routing key와 매칭되는 Queue로 Direct Route
    
    - replicas가 증가 됨에 따라 Queue가 mirroing되고  Master를 선출할 수 있도록 queue타입을 qurom으로 설정
    
    - Pulisher와 Consumer는 RabbitMQ를 구독하고 정해진 Queue와 Key를 통해 메시지를 발행 및 소비
    
    - 메시징 큐를 통해 메일을 발송하는 파이썬 서버를 생성함에 따라 Spring API 서버의 부하를 절감할 수 있음

<br>

## 기능 2 ) MSA

- ### 📥 Ingress
    ![ingress](https://user-images.githubusercontent.com/80504636/231360333-ada8357f-9df1-473d-b52a-37e5c5ec4a8b.png)
    
    - AWS 환경에서 이스티오의 Ingress gateway는 Classic Load Balancer(CLB)로 프로비저닝 된다. CLB는 Deprecated 되어 더이상 사용을 권장하지 않으며 경로 기반 라우팅을 위해서는 Application Load Balancer(ALB)로 서비스를 외부에 노출시킬 필요가 있다.
    
    - 쿠버네티스의 Ingress 리소스와 AWS Ingress Controller를 사용하여 ALB를 프로비저닝하고 이스티오의 Ingress gateway service를 NodePort 타입으로 변환해 ALB의 트래픽 전달 대상으로 지정하면 L7의 특성을 활용한 라우팅이 가능하다.
    
    <br>

- ### 🚦 Traffic Management
    ![traffic](https://user-images.githubusercontent.com/80504636/231360335-be15de0a-23a2-42cb-ab8b-326b9a72fd53.png)
    
    - 이스티오의 Ingress gateway를 통해 들어온 인바운드 트래픽은 istio ingress gateway pod를 통해 각 서비스의 istio proxy 컨테이너로 전달된다. 이때 라우팅의 방향과 규칙을 지정해주는 이스티오의 커스텀 리소스가 Gateway와 VirtualService이다. Gateway는 포트와 프로토콜, 인증서를 설정하고 VirtualService는 URI를 기반으로 어플리케이션 서비스로의 라우팅을 담당하고 있다.

    - VirtualService와 DestinationRule을 사용하여 트래픽의 비중을 조절하는 Canary 배포를 구현하였다.

<br>

- ### 📤 Egress
    
    ![egress](https://user-images.githubusercontent.com/80504636/231369912-74ea14b0-2cec-49fd-842c-5311ef0dfcc1.png)

    - 이스티오는 Egress gateway를 통해 아웃바운드 트래픽을 관리한다. Egress gateway에는 아웃바운드 트래픽 정책을 설정할 수 있는데 기본값은 ALLOW_ANY이며 이는 모든 아웃바운드 트래픽을 허용한다. REGISTRY_ONLY 모드는 ServiceEntry를 사용해 등록한 서비스의 아웃바운드 트래픽만 허용하며 그 외엔 모두 차단하도록 한다.

    - Ticketaka의 DB는 EKS 클러스터 외부에 별도의 서브넷에 위치하고 있어 ServiceEntry에 DB의 IP주소, 포트, 프로토콜 등의 정보를 등록해 줌으로써 서비스매쉬 내부의 어플리케이션이 DB에 접근할 수 있도록 하였다.

<br>

- ### 🔐 인증/인가
    
    ![authnauthz](https://user-images.githubusercontent.com/80504636/231377003-c01d2083-cfbf-47cf-97af-d9b3890b5360.png)

    - 이스티오의 Ingress gateway 는 다양한 인증, 인가 방식을 지원합니다. 저희가 선택한 방식은 JWT토큰을 이용한 방식으로, 유효한 토큰값이 헤더에 존재하는 경우에만 회원상세정보조회, 예약 등 회원전용 API 에 대한 접근을 허용하도록 하였습니다.
    
    - Istio 에서 JWT 토큰을 사용할때에는 반드시 Private, Public Key 방식을 이용한 토큰 생성, 파싱을 사용해야 합니다.
    
        - private key 를 이용해 생성한 jwt 토큰을 public key 를 통해 파싱하여 사용합니다.
        
        - **jwksUri - public key 의 주소**

            <details>
            <summary> ✨ code </summary>

            <div markdown="1">
                
            ![image](https://user-images.githubusercontent.com/80504636/231373210-751f59a4-4391-4a2f-bd8b-fc867d782dec.png)
                
            </div>
            </details>


    - 토큰값이 없거나 유효하지 않은 클라이언트가 접근할 경우 Istio Ingress gateway 가 해당 요청을 거부하고, 401 에러를 반환합니다.

    - 401 에러를 받은 클라이언트는 로그인창으로 리다이렉트 됩니다.

<br>

- ### 👀 모니터링
    ![monitoring](https://user-images.githubusercontent.com/80504636/231352003-09bb764f-d47b-4315-96e4-f117d6ef40d6.png)

    ![kiali](https://github.com/Project-Ticketaka/Frontend/assets/80504636/2d3d17ab-8bcf-4111-a9c3-082e85f8088d) |![grafana1](https://github.com/Project-Ticketaka/Frontend/assets/80504636/60e7f0c7-a9ee-43ca-8c48-990225181258) |![grafana2](https://github.com/Project-Ticketaka/Frontend/assets/80504636/31f109e2-830e-4bba-b04a-451c53d728f2)
    --- | --- | --- |

<br>

## 기능 3 ) CICD

- ### Flow

    ![cicd](https://user-images.githubusercontent.com/80504636/231349260-52079185-015c-454a-bcb2-21623cfa93e1.png)

    1. 개발자가 소스코드를 푸쉬하면 코드변경을 GithubAction 이 감지합니다.
    
    2. 변경된 코드를 이용한 새로운 버전의 이미지를 생성하여, 도커허브(이미지 레포지토리) 에 푸쉬합니다.
    
    3. 새로운 버전의 이미지가 생성되면, GithubAction 이 Github 의 Manifest 레포지토리의  kustomize 에 새로운 이미지 태그로 업데이트 합니다.
    
    4. ArgoCD 는 manifest 레포지토리의 변화를 감지하고, 변경된 이미지 버전으로 EKS Cluster에 배포합니다. 

<br>

- ### Why GithubAction + ArgoCD?

    배포전략을 선택하기에 앞서 왜 Github Action 과 ArgoCD 를 이용하여 CICD파이프라인을 구성하였는지 간단하게 설명하고자합니다.

    먼저 저희는 다음과 같은 후보들을 두고 어떠한 조합으로 CICD 파이프라인을 구성할지 고민하였습니다.

    1. Jenkins + ArgoCD
    
    2. GitLab
    
    3. Github Action + ArgoCD
    
    4. AWS CodePipeLine

    이중에 저희는 3번 조합을 선택하였습니다. 그 이유는 다음과 같습니다.

    1. 기존의 코드 관리를 Github 를 사용하여 관리하고 있었기 때문에 빠르게 구축이 가능하였습니다.
    
        - GitLab 의 경우 코드를 전부 GitLab 으로 옮기는 과정이 필요하였습니다.
    
    2. 별도의 서버 구축없이 사용이 가능하였습니다. 
    
        - Jenkins 의 경우 별도의 서버구축이 필요하였습니다.
    
    3. 배포전략의 선택지가 많았습니다. 
    
        - CodePipeLine 의 경우 EKS에 CodeDeploy 를 지원하지 않아 Kubernetes 에서 제공하는 기본 배포전략밖에 사용할 수 없었습니다.

<br>

- ### 배포전략선택
    기존의 Kubernetes 의 경우에는 두가지 배포방법을 지원합니다. 

    - Rolling Update - deploy 를 순차적으로 변경해가는 방식
    
    - Recreate - deploy 를 전부 내렸다가, 다시 생성하는 방식

    <br>

    저희는 기본 배포전략이 아닌 조금 더 다양한 방식의 배포전략에 대해 알아보았습니다. 

    - Blue- Green - 최신버전과 구버전을 동시에 전부 띄워두는 방식(예를 들어 replica =3 이라면 총 6개의 replica 가 배포됩니다.)

    - Canary -  최신버전과 구버전을 동시에 띄워두지만, 비율을 조정할 수 있습니다.(replica =3 이라면 최신 2개 구버전 1개 이런식으로 설정이 가능)

    위의 네가지 배포방식은 각각의 장단점이 있지만 저희는 Canary 배포방식을 선택하였습니다. 그 이유는 다음과 같습니다.

    <br>

    1. 버전변경에 대한 오류 탐지
        
        기능을 추가하거나 변경사항을 적용하고 배포를 하게되면 Canary 는 이전버전을 일정시간동안 살려둘 수 있습니다. 만약 최신버전이 오류가 발생한다면 이전버전으로의 롤백 또한 용이하다 라는 장점이 있습니다.
        
    2. 리소스 최적화(비용 최소화)
        
        1의 장점은 Blue -Green 또한 가지고 있지만, 저희가 요구하는 replica 의 2배만큼 생성될 수 있을 만큼의 리소스를 필요로합니다. 저희는 컴퓨팅 리소스를 최소화하고 비용을 최소화하기 위하여 Canary 배포방식을 선택하였습니다. 
        
        <br>

    하지만 Kubernetes 의 Deployment 는 Canary 배포방식을 지원하지 않습니다. 
    
    따라서 저희는 ArgoCD 의 커스텀 리소스인 Rollout 을 활용하여 저희의 서버를 배포하였습니다. 
    <details>
    <summary> ✨ code </summary>

    <div markdown="1">
        
    ![image](https://user-images.githubusercontent.com/80504636/231374173-2eba79e4-33b3-4d74-9d70-509496b42539.png)

    </div>
    </details>
<br>

# <span id="retrospective">💬 회고</span>

- ![retro](https://user-images.githubusercontent.com/80504636/231395128-b7be313c-3b6d-406d-a093-829fdcc75be0.png)
