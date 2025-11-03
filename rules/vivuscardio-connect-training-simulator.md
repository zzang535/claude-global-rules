# CPR 훈련 시뮬레이터 - 개발 규칙

## 프로젝트 개요

- **목적**: CPR 훈련 시뮬레이션 데모 프로그램
- **기술 스택**: React, Next.js, Tailwind CSS, Framer Motion
- **디자인 기준**: Figma 시안 정확한 구현

## 중요 참조 사항

**`app/training` 디렉토리 작업 시:**

- 항상 `app/training/TRAINING_RULES.md`를 참조하여 상세 명세 확인
- TRAINING_RULES.md에는 다음의 포괄적인 기능 문서가 포함되어 있음:
  - 완전한 컴포넌트 명세
  - 각 화면의 구현 세부사항
  - 기술 아키텍처 및 데이터 흐름
  - 사용자 인터랙션 시나리오
  - 시각적 디자인 명세

## 개발 규칙

### 1. 파일 구조

- 모든 컴포넌트는 `app/training` 디렉토리 내에 생성
- Next.js App Router 규칙 준수

### 2. 디자인 구현

- Figma 시안의 정확한 픽셀 값 사용
- 색상 코드 엄격히 준수:
  - 녹색: `#56ED89`, `#34C85A`
  - 파란색: `#0061F2`
  - 회색: `#666666`, `#999999`, `#333333`
  - 배경: `#F5F5F5`, `#F5F5F6`
  - 빨간색: `#E84858`, `#EF4444`
- `rounded-[50px]` 사용 (완전한 원형 아님)
- 아이콘 크기: 헤더는 일반적으로 12.8px × 12.8px, 사이클 카운터는 25px × 25px

### 3. 컴포넌트 명명

- 명확하고 기능을 설명하는 이름 사용
- 예시: `TrainingHeader`, `FeedbackGauge`, `CycleCounter`
- 아이콘 컴포넌트: `Icon` 접두사 사용 (예: `IconCompression`, `IconVentilation`)

### 4. 상태 관리

- 로컬 상태는 React hooks 사용
- 공유 데이터는 부모 컴포넌트로 상태 끌어올리기
- Props 흐름: 부모 (page.jsx) → 자식 컴포넌트
- 실시간 데이터: clickPosition, isPressed, depth, rateData, ventilationVolume

### 5. 아이콘 컴포넌트

모든 아이콘 컴포넌트는 다음 패턴을 따라야 함:

```jsx
export default function IconName({ width = 20, height = 20, color = "#999999", ...props }) {
  return (
    <svg width={width} height={height} viewBox="..." fill="none" {...props}>
      <path stroke={color} ... />
    </svg>
  );
}
```

### 6. CPR 가이드라인 및 표준

- **압박 속도**: 100-120회/분 (0.5-0.6초 간격)
- **압박 깊이**: 5-6 cm (백분율로 표현)
- **압박-환기 비율**: 30:2
- **속도 평가**:
  - 너무 빠름: < 0.4초 (빨간색)
  - 적정: 0.4-0.8초 (녹색)
  - 너무 느림: > 0.8초 (주황색)

### 7. 실시간 피드백 시스템

- **Position 게이지**: 압박 위치 정확도 표시
- **Depth 게이지**: 위에서 아래로 채워짐 (녹색 영역 + 파란 기준선)
- **Rate 게이지**: 압박 간격을 수직선으로 표시
- **Volume 게이지**: 환기를 위해 아래에서 위로 채워짐

### 8. 평가 시스템

압박 평가 항목:

- Position: 심장 위치로부터의 거리
- Depth: 적정 범위 내 (40-80%)
- Rate: 압박 간 간격
- Overall: 세 가지 모두 통과해야 함

환기 평가 항목:

- Volume: 적정 범위 내 (40-80%)

### 9. 시각적 피드백 상태

- **성공**: 녹색 (#34C85A) + 체크마크 아이콘
- **실패**: 빨간색 (#E84858) + X 아이콘
- **대기**: 회색 (#F5F5F6) + 숫자

### 10. 애니메이션 및 전환

- 페이지 전환은 Framer Motion 사용
- 부드러운 전환: 인터랙션 200ms, 실시간 업데이트 75ms
- 마네킹의 인터랙티브 포인트에 맥박 애니메이션 적용

## 주요 컴포넌트 참조

### TrainingCycleCounter

- 레이아웃: 사이클 라벨 (150px) + flex-1 카운터 영역 (justify-between)
- 30개 압박 + 2개 환기 인디케이터
- 성공/실패 상태는 숫자 대신 아이콘으로 표시

### TrainingGaugeDepth

- 아이콘: IconCompression
- 위에서 아래로 채워짐 (녹색 영역 + 파란 기준선)
- 라벨: "Too shallow" (상단), "Too deep" (하단)

### TrainingGaugeRate

- 아이콘: IconRate
- 현재 압박 간격을 수직선으로 표시
- 아이콘: IconSpeedSlow (거북이), IconSpeed (속도계)

### TrainingGaugeVolume

- 아이콘: IconVentilation
- 아래에서 위로 채워짐
- 특수: "Damaged" 태그에 IconInfantWarning 사용

### TrainingGaugePosition

- 아이콘: IconTarget
- 압박 위치에 따라 녹색 점이 이동
- "Head"와 "Center" 라벨이 있는 격자 배경

## 테스트 및 검증

- 압박 속도 계산 테스트
- 위치 정확도 측정 검증
- 깊이 백분율 변환 확인
- 환기 볼륨 임계값 검증

## 문서 업데이트

### 기획 문서화 규칙

**새로운 기능이나 기획이 개발 완료된 경우:**

- **반드시** `app/training/TRAINING_RULES.md`에 해당 기획 내용을 추가해야 함
- 다음 내용을 포함하여 작성:
  - 기능 개요
  - 화면 구성 요소
  - 시각적 명세
  - 상호작용 흐름
  - 기술적 구현 세부사항
  - 데이터 구조 및 상태 관리
  - 향후 확장 계획 (해당하는 경우)

**문서화 타이밍:**

- 기능 구현이 완료되고 테스트가 끝난 직후
- 커밋 전에 문서 업데이트 완료

**문서화 형식:**

- 기존 TRAINING_RULES.md의 형식과 구조를 따를 것
- 표(Table)를 활용하여 명세를 명확하게 작성
- 코드 예시는 ```jsx 블록으로 표시

새로운 기능 구현 시 이 파일(.cursorrules)에 다음 내용 업데이트:

- 기술 구현 세부사항
- 시각적 명세
- 사용자 인터랙션 흐름
- 다른 컴포넌트와의 통합 지점

## Git 워크플로우 규칙

### 커밋 및 푸시 정책

- 사용자의 명시적 요청 없이 **절대** 자동으로 커밋하거나 푸시하지 않음
- 사용자가 명시적으로 커밋을 요청할 때만 커밋
- 사용자가 명시적으로 푸시를 요청할 때만 푸시
- 작업 완료 후 변경사항이 준비되었음을 알리되, 커밋/푸시는 사용자의 지시를 대기
- 사용자가 "커밋 푸시" 또는 유사한 요청을 하면 두 작업 모두 진행
