# 🚀 XRP Ledger 기반 온라인 챌린지 플랫폼

[![XRP Ledger 챌린지 플랫폼 시연 영상](https://img.youtube.com/vi/axnqKjhl4eo/hqdefault.jpg)](https://youtu.be/axnqKjhl4eo)

<p align="center"><i>이미지를 클릭하면 전체 시연 영상을 볼 수 있습니다.</i></p>

본 프로젝트는 **XRP Ledger(XRPL)**의 신뢰성과 **Spring Modulith 기반 이벤트 주도 아키텍처**의 확장성을 결합한 온라인 챌린지 플랫폼입니다.

---

# ✨ 핵심 플로우

## 1단계: `EscrowCreate` - 조건부 예치로 자산의 신뢰성 확보
사용자가 챌린지 참가비를 지불하면, 자금은 플랫폼의 지갑이 아닌 **XRPL의 `EscrowCreate` 트랜잭션**을 통해 조건부 계좌에 안전하게 예치됩니다. 이 에스크로는 '챌린지 종료' 조건이 충족되어야만 자금이 해제되도록 설계되어, 운영 주체조차 자금을 임의로 사용할 수 없습니다.

* **기술적 가치**
    * **탈중앙화된 신뢰**: 운영 주체가 아닌, 블록체인의 **불변 코드(Immutable Code)**가 플랫폼의 신뢰를 보장합니다.
    * **투명성 보장**: 모든 예치 내역은 **XRPL 원장에서 공개적으로 검증**할 수 있습니다.

---

## 2단계: `ChallengeCompletedEvent` - 이벤트 기반 아키텍처로 모듈 결합도 최소화
챌린지 종료 시, `catalog` 모듈에서 `ChallengeCompletedEvent`가 발행됩니다. `xrpl` 모듈은 이 이벤트를 비동기적으로 수신하여 온체인 정산 로직을 실행하는 **트리거**로 사용합니다. 이 과정에서 각 모듈은 직접적인 의존 관계를 갖지 않습니다.

* **기술적 가치**
    * **유연한 확장성**: 새로운 보상 정책 등 비즈니스 로직이 추가되어도, **기존 코드 수정 없이 새 이벤트 리스너를 구현**하여 유연하게 확장할 수 있습니다.
    * **관심사의 분리(SoC)**: 챌린지 상태 관리(`catalog`)와 온체인 정산(`xrpl`)의 **책임을 명확히 분리**하여 시스템 유지보수성을 높입니다.

---

## 3단계: Batch 처리 - 대규모 온체인 트랜잭션의 자동화
이벤트 수신 후, 시스템은 모든 참가자의 에스크로 정산(`EscrowFinish`)과 성공 유저에 대한 환급금 지급(`Payment`) 트랜잭션을 **일괄(Batch)로 생성하여 제출**합니다. 이때 코드 레벨에서 계정의 Sequence 번호를 직접 관리하여 **논스(Nonce) 충돌을 방지**하고 처리 효율을 극대화합니다.

* **기술적 가치**
    * **운영 효율 극대화**: 수백, 수천 건의 정산 및 지급 트랜잭션을 **수동 개입 없이 자동으로 처리**합니다.
    * **비용 및 시간 절감**: 운영 리소스를 최소화하고 사용자에게 빠르고 정확한 보상을 제공합니다.

---

## 4단계: Memo 필드 - 블록체인에 기록하는 영구적인 '성취 증명'
환급금을 지급하는 `Payment` 트랜잭션의 **Memo 필드**에는 트랜잭션의 목적과 내용이 담긴 **JSON 형식의 데이터**를 포함시킵니다.

* **기술적 가치**
    * **온체인 증명(On-chain Proof)**: 챌린지 성공 기록은 **XRPL 원장에 영구적으로 기록**되어 누구나 검증할 수 있는 증거가 됩니다.
    * **데이터 확장성**: 구조화된 Memo 데이터는 향후 사용자 활동 분석, 외부 서비스 연동을 통한 **디지털 자격증명(Credential) 등으로 활용**될 수 있는 기반을 마련합니다.

## 📸 애플리케이션 UI 스크린샷

<p align="center">
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.44.06.png" alt="앱 UI 스크린샷 1" width="600">
  <br><br>
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.44.56.png" alt="앱 UI 스크린샷 2" width="600">
  <br><br>
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.45.09.png" alt="앱 UI 스크린샷 3" width="600">
  <br><br>
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.45.36.png" alt="앱 UI 스크린샷 4" width="600">
  <br><br>
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.45.44.png" alt="앱 UI 스크린샷 5" width="600">
  <br><br>
  <img src="https://klmhccurilugmhhbhpyd.supabase.co/storage/v1/object/public/challenge-proof/Untitled%20folder/Screenshot%202025-09-21%20at%2009.46.15.png" alt="앱 UI 스크린샷 6" width="600">
</p>
