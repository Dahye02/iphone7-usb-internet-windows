# iPhone 7 USB Internet on Windows 11

윈도우 노트북의 인터넷을 USB-A–Lightning 데이터 케이블로 아이폰에 공유합니다. 아이폰의 Wi-Fi나 Bluetooth를 사용하지 않습니다.

이 프로젝트는 [0xbaksa/iphone-usb-reverse-tethering-windows](https://github.com/0xbaksa/iphone-usb-reverse-tethering-windows)를 기반으로 정리한 파생 프로젝트입니다. 핵심 USB 모드 전환과 인터넷 공유 구현은 원저자의 작업이며, MIT 라이선스와 저작권 고지를 유지합니다.

## 확인한 환경과 범위

- Windows 11 Pro, 빌드 26200
- iPhone 7, Lightning 데이터 케이블
- USB 네트워크 모드 전환, Windows NCM 어댑터 연결, 인터넷 연결 공유(ICS), 아이폰의 DHCP 주소 할당 확인
- 아이폰의 정확한 iOS 버전은 확인하지 않았습니다. 다른 기기와 환경의 호환성 및 실제 웹 탐색 성공은 별도 확인이 필요합니다.

## 준비물

- 인터넷에 Wi-Fi로 연결된 Windows 11 노트북
- Python 3 (`python` 명령 사용 가능)
- 관리자 권한
- 데이터 전송이 가능한 USB–Lightning 케이블

## 설치와 실행

1. 이 저장소를 다운로드하거나 복제합니다.
2. 아이폰을 케이블로 연결하고 잠금을 풉니다.
3. PowerShell에서 `powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Setup.ps1`을 실행하고 관리자 확인 창을 직접 승인합니다.
4. 드라이버 적용을 위해 케이블을 뽑고 5초 뒤 다시 연결합니다.
5. `USB Share.cmd`를 더블클릭하면 자동 연결을 시도합니다. 또는 아래 명령으로 한 번만 연결합니다.

```powershell
& '.\USB Share.cmd' on
```

`USB SHARE ON`과 아이폰의 IP 주소가 표시되면 아이폰에서 셀룰러 데이터를 잠시 끄고 Safari로 인터넷을 확인합니다. 설정에 Ethernet 항목이 표시될 수 있습니다.

```powershell
& '.\USB Share.cmd' status # 상태 확인
& '.\USB Share.cmd' off    # 인터넷 공유 중지 (드라이버 제거 아님)
```

## 아이폰 7에서 확인한 차이

USB 모드 응답이 신형 기기의 `3:3:3:0` / `5:3:3:0` 대신 `3:3:3` / `5:3:3`일 수 있습니다. 이 두 형태를 모두 상태 표시에서 처리합니다. 실행 끝의 키 입력 대기는 제거했습니다.

아이폰 7에서는 장치 재시작만으로 모드가 초기화되지 않을 수 있습니다. `phone rejected the mode switch`가 나오면 케이블을 뽑고 5초 뒤 다시 꽂아 실행하세요. 이미 실행 중이라는 메시지가 나오면 이전 프로그램 창을 닫아 주세요.

## 시스템에 적용되는 변경

- 아이폰 USB 장치에 libusb-win32 필터 드라이버를 설치합니다.
- USB 구성 및 CDC 열거 설정을 변경하고 AppleLowerFilter를 해당 장치에서 분리합니다.
- USB 사진 가져오기 인터페이스를 비활성화합니다. 사진 가져오기가 필요하면 장치 관리자에서 다시 활성화해야 합니다.
- 실행 시 모바일 핫스팟을 중지하고 기존 ICS 공유 설정을 해제한 뒤 Wi-Fi 연결을 아이폰 USB 어댑터에 공유합니다.

여러 Apple USB 기기가 연결돼 있다면 대상 선택이 모호할 수 있으므로 아이폰 한 대만 연결하세요. 기존 ICS 공유를 사용 중이면 실행 전에 설정을 기록하세요. `off`는 공유를 중지하지만 드라이버와 레지스트리 변경을 되돌리지는 않습니다.

## 라이선스와 의존성

프로젝트 코드는 [MIT License](LICENSE)를 따릅니다. 원저자: Baksa (0xbaksa), 2026.

드라이버 및 DLL 바이너리는 이 저장소에 포함하지 않습니다. `Setup.ps1`이 libusb-win32 1.4.0.2 공식 릴리스를 내려받고 SHA-256과 카탈로그 서명을 확인합니다. libusb-win32의 라이선스와 배포 정보는 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)를 참고하세요.
