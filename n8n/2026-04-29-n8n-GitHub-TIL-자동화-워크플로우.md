### Context
사용자가 디스코드 채널에 메시지를 남기면 이를 자동으로 감지하여 기술 문서(TIL) 형식으로 변환하고, 자신의 GitHub 저장소에 문서를 업로드하는 자동화 시스템을 구축하였습니다. 단순히 파일만 생성하는 것이 아니라 전체 문서 리스트를 관리하는 README.md 파일의 인덱스까지 자동으로 갱신하여 관리의 편의성을 극대화하는 것이 이번 워크플로우의 핵심 목표입니다.

### Core
전체 워크플로우는 5개의 주요 단계로 구성되며, 각 노드는 이전 노드의 결과값(Output)을 받아 다음 작업의 입력값(Input)으로 사용합니다.

*   **Discord Trigger**: 특정 디스코드 채널에 메시지가 게시되면 실행됩니다. (Event: Message Created)
*   **AI Agent (LLM)**: 수신된 메시지를 분석하여 정해진 TIL 템플릿(Context, Core, Insight)으로 구조화합니다. 이때 'Anthropic'이나 'OpenAI' 모델을 도구(Tool)와 연결하여 프롬프트를 수행합니다.
*   **GitHub Node (Push File)**: 생성된 마크다운 내용을 기반으로 신규 파일을 생성합니다. (Resource: File, Operation: Create/Update, Path: `TIL/YYYY-MM-DD-제목.md`)
*   **GitHub Node (Update README)**: 기존 README.md 내용을 가져온 뒤, 새롭게 추가된 문서의 링크를 리스트 하단에 추가(Append)하여 파일을 업데이트합니다.
*   **Discord Node (Response)**: 모든 작업이 완료되면 처리 결과와 저장소 링크를 디스코드 채널에 발신하여 알립니다.

```json
{
  "nodes": [
    {
      "parameters": {
        "resource": "file",
        "operation": "createOrUpdate",
        "owner": "username",
        "repository": "til-repo",
        "filePath": "={{$json.filename}}",
        "fileContent": "={{$json.content}}",
        "commitMessage": "Add new TIL document"
      },
      "name": "GitHub_Push_TIL",
      "type": "n8n-nodes-base.github"
    }
  ]
}
```

### Insight
n8n 워크플로우 설계 시 가장 중요한 핵심은 각 노드의 데이터 구조를 파악하는 것입니다. 각 노드가 어떤 데이터(Input)를 요구하고 수행 후 어떤 형식(Output)으로 데이터를 내보내는지 정확히 이해한다면 복잡한 로직도 손쉽게 구현할 수 있습니다. 특히 GitHub 노드와 같이 기존 파일을 수정해야 하는 경우, 기존 파일의 SHA 값을 참조하거나 원본 데이터를 먼저 불러오는 단계가 필수적임을 확인했습니다. 직관적인 UI 덕분에 코딩 지식이 부족하더라도 데이터 흐름만 제어할 줄 안다면 강력한 자동화 도구를 만들 수 있다는 점이 n8n의 큰 장점입니다.

**출처:**
*   [n8n GitHub Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/)
*   [n8n AI Agent Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.ai-agent/)
*   [n8n Discord Trigger Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.discord-trigger/)