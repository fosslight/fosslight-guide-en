---
sort: 1
published: true
---
# How to Run

```note
ORT는 Linux, Windows, macOS에 대하여 지원합니다.  

ORT를 실행하려면 최소한 Java 11이 필요합니다. 메모리 및 CPU 요구 사항은 분석할 프로젝트의 크기와 유형에 따라 다르지만, 8GiB 메모리(-Xmx = 8g)로 Java를 구성하고 코어가 4개 이상인 CPU를 권장합니다.  

ORT에서 프로젝트를 분석하기 위해 필요한 외부 도구는 ort requirements 명령으로 조회 가능합니다. 패키지 관리자가 목록에 없는 경우는 ORT에 내부적으로 직접 통합되며 외부 도구를 설치할 필요가 없는 경우입니다.

[공식 리파지토리](https://github.com/oss-review-toolkit/ort)에서 Source Code를 다운로드 받을 수 있고, Build시 Docker 이미지 또는 실행 파일을 생성할 수 있습니다. 

```
{% include list.liquid all=true %}
