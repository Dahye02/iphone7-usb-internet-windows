# iPhone 7 USB Internet on Windows 11

윈도우 노트북의 인터넷을 USB-A–Lightning 데이터 케이블로 아이폰에 공유

이 프로젝트는 [0xbaksa/iphone-usb-reverse-tethering-windows](https://github.com/0xbaksa/iphone-usb-reverse-tethering-windows)를 기반으로 정리한 파생 프로젝트입니다. 핵심 USB 모드 전환과 인터넷 공유 구현은 원저자의 작업이며, MIT 라이선스와 저작권 고지를 유지합니다.

## 환경과 범위
- Windows 11 Pro, 빌드 26200
- iPhone 7, Lightning 데이터 케이블
- USB 네트워크 모드 전환, Windows NCM 어댑터 연결, 인터넷 연결 공유(ICS), 아이폰의 DHCP 주소 할당 확인
- 아이폰의 정확한 iOS 버전은 확인하지 않았습니다. 다른 기기와 환경의 호환성 및 실제 웹 탐색 성공은 별도 확인이 필요합니다.

## 준비물
- 인터넷에 Wi-Fi로 연결된 Windows 11 노트북
- Python 3
- 관리자 권한
- 데이터 전송이 가능한 USB–Lightning 케이블

## 설치와 실행
1. 이 저장소를 다운로드하거나 복제
2. 아이폰을 케이블로 연결하고 잠금 풀기
3. PowerShell에서 `powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Setup.ps1`을 실행하고 관리자 승인
4. 드라이버 적용을 위해 케이블을 뽑고 5초 뒤 다시 연결
5. `USB Share.cmd`를 더블클릭하면 자동 연결을 시도 or 아래 명령으로 연결

```powershell
& '.\USB Share.cmd' on
```

`USB SHARE ON`과 아이폰의 IP 주소가 표시되면 아이폰에서 인터넷 연결 확인

```powershell
& '.\USB Share.cmd' status # 상태 확인
& '.\USB Share.cmd' off    # 인터넷 공유 중지 (드라이버 제거 아님)
```



## 라이선스
프로젝트 코드는 [MIT License](LICENSE)를 따릅니다. 원저자: Baksa (0xbaksa), 2026.

드라이버 및 DLL 바이너리는 이 저장소에 포함하지 않습니다. `Setup.ps1`이 libusb-win32 1.4.0.2 공식 릴리스를 내려받고 SHA-256과 카탈로그 서명을 확인합니다. libusb-win32의 라이선스와 배포 정보는 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)를 참고하세요.
