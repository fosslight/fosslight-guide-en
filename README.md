# OSS Review Toolkit (ORT) 란

OSS Review Toolkit (ORT)은 Project에 포함된 Open Source Software의 dependency 정보를 자동으로 추출하고, 해당 Open Source Software를 다운로드 받아 Source Code에 대하여 String Search를 기반으로 분석하는 Open Source tool([ScanCode](https://github.com/nexB/scancode-toolkit), [Askalono](https://github.com/amzn/askalono), [lc](https://github.com/boyter/lc), [Licensee](https://github.com/benbalter/licensee))을 이용하여 License, Copyright등의 정보를 추출합니다. 검출된 License에 대하여 사용자가 정의한 Policy를 적용할 수 있으며, 분석 결과를 다양한 형태의 Report로 생성합니다.    

## 특징
- [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html)으로 배포되는 Open Source로, 무료로 사용할 수 있고, Source Code 수정이 가능합니다.
- [Kotlin](https://kotlinlang.org/)으로 작성되었으며 build system으로 [Gradle](https://gradle.org/)을 이용합니다.   
- 프로젝트의 transitive dependency의 tree와 package의 meta data를 분석합니다.
- Source Code에 대한 License text 분석시, Scanner를 선택 가능합니다.
- 사용자가 정의한 License Policy를 적용할 수 있습니다.
- 다양한 포맷의 Report를 생성할 수 있습니다.

## 관련 링크
- 배포처 : [https://github.com/oss-review-toolkit/ort](https://github.com/oss-review-toolkit/ort)
