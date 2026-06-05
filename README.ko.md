# ChaPT 사후 분석(post-mortems)

_[Read this in English 🇬🇧](README.md)_

[Cha Physical Therapy](https://chaphysicaltherapy.com)의 예약 및 분석 플랫폼 — 제가 혼자 만들고 운영하는 소규모 Go + MySQL 웹 애플리케이션 — 운영 과정에서 작성한 사후 검토 및 하드닝 산출물 모음입니다.

포트폴리오 산출물로 작성되었습니다: 책임 추궁 없이(blameless), 증거 기반으로, 그리고 각 장애의 조치 항목을 마무리한 설정 파일들을 포함합니다.

## 장애(Incidents)

- **[2026-04-17 — gp3 처리량 포화 연쇄 장애](2026-04-17/2026-04-17.ko.md)** — 최적화되지 않은 일련의 분석 쿼리가 EC2 인스턴스의 gp3 루트 볼륨을 포화시켰고, systemd DBus 타임아웃, Docker 헬스 체크 실패, SSH 멈춤을 거쳐 약 3시간의 장애로 연쇄되었습니다. 새 인스턴스 + Elastic IP 재할당으로 복구했으며, 데이터 손실은 없었습니다.

## 구조(Layout)

```
.
└── 2026-04-17/                         # 장애별 폴더
    ├── 2026-04-17.md                   # 사후 분석 보고서 (영문)
    ├── 2026-04-17.ko.md                # 사후 분석 보고서 (국문)
    ├── buffer.md                       # 타임라인에서 인용한 원본 로그 증거
    ├── CloudWatchGraphs/               # 본문에 인라인 참조된 CloudWatch 스크린샷
    └── hardening/                      # 이 장애에 대응하여 구현한 조치 항목
        ├── my.cnf                      # MySQL 튜닝 (조치 항목 #2, #4)
        └── docker-compose-logging.yml  # Docker 로그 로테이션 (조치 항목 #6)
```

## ChaPT 소개

1인 개발 프로젝트로, 제가 설계, 개발, 배포, 운영을 모두 담당합니다. Go 백엔드, MySQL, 결제를 위한 Stripe Elements를 사용하며, Elastic IP 뒤의 단일 EC2 인스턴스에서 호스팅됩니다. 여기 실린 사후 분석은 1인 운영 스택이 아무도 미리 경고해 주지 않은 인프라 한계치에 부딪혔을 때 무엇이 잘못되는지를 기록한 사례 중 하나입니다.

## 비식별화(sanitization)에 대한 참고

`buffer.md`에 있는 작성자의 SSH 출발지 IP와 SSH 호스트 지문(fingerprint)은 플레이스홀더(`203.0.113.5`, `[REDACTED]`)로 대체되었습니다. AWS 내부 호스트명은 RFC 1918 플레이스홀더(`ip-10-0-0-10`, `ip-10-0-0-20`)로 일반화되었습니다. 그 외 포렌식 서술은 변경되지 않았습니다.
