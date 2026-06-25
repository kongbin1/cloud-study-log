# 4일차 (2026-06-25)

## 오늘 한 것
- nano로 터미널에서 파일 편집하기
- apt로 패키지 설치하려다 → 네트워크가 아예 안 되는 걸 발견 → 처음부터 끝까지 직접 해결
- 결국 `tree` 패키지 설치 성공

> 원래 계획은 "nano + apt 설치"였는데, 인터넷이 안 돼서 **네트워크 트러블슈팅 풀코스**를 하게 됐다.

---

##  막혔던 문제 & 해결 (오늘의 핵심)

### 문제 상황
`sudo apt update`를 했더니 인터넷 서버가 아니라 `file:/cdrom`만 쳐다보고 에러가 났다.
```
The repository 'file:/cdrom noble Release' no longer has a Release file.
```
게다가 `ping 8.8.8.8`을 해보니 `Network is unreachable` — 인터넷 자체가 안 됐다.

### 원인 (세 가지가 겹쳐 있었음)
1. **네트워크 카드가 꺼져 있었다** — `ip a`로 보니 `ens33`이 `state DOWN`이고 IP(`inet`)가 없었음.
2. **netplan 설정 파일이 아예 없었다** — `/etc/netplan/` 폴더가 텅 비어 있어서 우분투가 네트워크를 어떻게 켤지 몰랐음.
3. **apt 소스가 cdrom으로만 설정돼 있었다** — 인터넷 주소록(`ubuntu.sources`)이 `.curtin.orig`로 이름이 바뀌어 비활성화돼 있었음. (apt는 `.sources`/`.list` 파일만 읽고 `.orig`는 무시한다)
4. **DNS가 없었다** — IP 연결은 됐지만 `archive.ubuntu.com` 같은 도메인 이름을 숫자로 못 바꿔서 `Temporary failure resolving` 에러가 났음.

### 해결 과정
1. VMware 네트워크 설정 확인 → 이미 NAT, Connected라 VMware 쪽은 문제없었음. → 문제는 우분투 내부였다.
2. `ip a`로 `ens33`이 DOWN + IP 없음을 확인.
3. `/etc/netplan/01-netcfg.yaml` 파일을 nano로 새로 작성:
   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       ens33:
         dhcp4: true
         nameservers:
           addresses: [8.8.8.8, 8.8.4.4]
   ```
4. `sudo netplan apply` → IP를 받아오고 `ping 8.8.8.8` 성공.
5. 비활성화돼 있던 인터넷 소스를 되살림:
   ```bash
   sudo cp /etc/apt/sources.list.d/ubuntu.sources.curtin.orig /etc/apt/sources.list.d/ubuntu.sources
   ```
6. cdrom 소스 줄을 주석 처리:
   ```bash
   sudo sed -i 's/^deb/#deb/' /etc/apt/sources.list
   ```
7. `sudo apt update` → 드디어 인터넷에서 받아오기 성공!
8. `sudo apt install tree` → 첫 패키지 설치 성공.

---

## 배운 것 / 헷갈렸던 것

- **netplan은 YAML이라 들여쓰기(스페이스)에 엄격하다.** Tab 쓰면 안 되고, 칸 수가 한 칸만 틀려도 에러. (network=0칸 → version/renderer/ethernets=2칸 → ens33=4칸 → dhcp4/nameservers=6칸 → addresses=8칸)
- **`ping IP`는 되는데 도메인이 안 되면 = DNS 문제다.** (전화선은 됐는데 전화번호부가 없는 상태)
- **apt는 `.sources`/`.list` 파일만 읽는다.** `.orig` 같은 확장자는 무시.
- **root 전용 파일은 `sudo`를 붙여야 읽고 쓸 수 있다.** `chmod 600`을 해놓으면 그냥 `cat`으론 못 읽고 `sudo cat`을 써야 함.
- **리눅스는 대소문자/띄어쓰기에 엄격하다.** (`ip A`는 에러, `ip a`가 맞음 / `remount`를 `remout`로 오타 냈더니 안 됨)
- **`$`는 일반 사용자, `#`은 root(관리자).** 프롬프트만 봐도 권한을 알 수 있다.

## 어려웠던 점 / 다음에 다시 볼 것
- netplan 구조가 아직 완전히 익숙하진 않다. 나중에 한 번 더 정리하기.
- 왜 처음부터 netplan이 비어 있었는지, `.curtin.orig`가 왜 생겼는지는 아직 정확히 모름. (설치 방식 때문인 듯)

## 한 줄 회고
> 욕 나올 만큼 헤맸지만, "인터넷 안 되는 리눅스 서버를 직접 진단하고 살린" 경험을 했다. 면접에서 트러블슈팅 경험 물으면 이걸 그대로 말하면 된다.
