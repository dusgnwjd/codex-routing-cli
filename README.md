# Codex Routing CLI

Windows에서 기존 ChatGPT 로그인으로 Codex 모델을 선택하고, 독립 작업의 결과를 합치는 개인용 CLI입니다. Python 3.10 이상과 PATH에 등록된 네이티브 Codex CLI가 필요합니다. 사용할 모델은 계정에서 이용할 수 있어야 합니다.

작업할 Git 저장소에서 PowerShell을 열어 실행합니다. 아래 예시에서 `route.py`는 내려받은 파일의 경로입니다.

```powershell
codex login status
python route.py "프로젝트 구조를 설명해줘" --dry-run
python route.py "프로젝트 구조를 설명해줘"
python route.py --route routine "README의 설치 요구사항을 표로 정리해줘"
python route.py --route review "두 설계안의 운영상 위험을 비교해줘"
python route.py "설치와 테스트 절차를 합쳐줘" --task "설치 절차 정리" --task "테스트 절차 정리" --log routing.jsonl
```

- 기본 경로는 Sol / Medium입니다. `--route routine`은 Luna / Low, `--route tool-heavy`는 Luna / High, `--route review`는 Astra / Medium입니다.
- `--task` 또는 `--tool-task`를 반복 지정하면 최대 네 작업을 Luna에서 병렬 처리하고 Sol이 결과를 통합합니다. 각 작업에는 해당 작업에 필요한 맥락을 넣으세요.
- 시간 초과나 빈 응답은 Luna에서 한 번 더 시도하고, 두 번째 실패 후 Sol로 넘깁니다. Codex 오류는 즉시 중단합니다. 답변 품질과 Astra 검토는 사용자가 판단합니다.
- `--cwd`로 다른 Git 저장소를 선택할 수 있습니다. 모든 호출은 읽기 전용이며, 로컬 라우팅 설정을 수정하지 않습니다.
- `--dry-run`은 실행 계획만 출력합니다. `--log`는 모델, 추론 강도, 상태, 경과 시간만 저장합니다. 이 CLI는 로그에 프롬프트, 응답, 자격 증명을 저장하지 않습니다.
