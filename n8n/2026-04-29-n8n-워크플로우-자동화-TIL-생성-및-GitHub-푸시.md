### Context

Discord 메시지 수신 시 n8n 워크플로우를 트리거하여, Agent 실행, TIL(Today I Learned) 형식으로 변환, GitHub 푸시, README.md 업데이트, 그리고 완료 메시지 발송까지 자동화하는 과정을 기록합니다.

### Core

*   **Trigger**: Discord 메시지 수신
*   **Agent Execution**: 수신된 메시지를 기반으로 Agent 실행
*   **TIL Conversion**: Agent 실행 결과를 TIL 형식의 Markdown으로 변환
*   **GitHub Push**: 변환된 TIL 파일을 GitHub 저장소에 푸시
*   **README Update**: 푸시된 TIL 파일의 인덱스를 `README.md`에 추가
*   **Completion Message**: 모든 작업 완료 후 Discord 채널에 완료 메시지 발송

(참고: 실제 n8n 워크플로우 설정은 노드 간의 Input/Output 이해를 바탕으로 구성됩니다. 각 노드의 Input과 Output을 명확히 파악하는 것이 중요합니다.)

### Insight

n8n을 처음 사용해 보았는데, 각 노드의 Input과 Output을 명확히 이해하면 워크플로우 자동화가 생각보다 어렵지 않다는 것을 알게 되었습니다. 특히 Agent를 통해 TIL 형식으로 문서를 자동 생성하고 이를 GitHub에 연동하는 과정은 생산성 향상에 크게 기여할 것으로 기대됩니다. 이 과정을 쉽게 이해하고 구현할 수 있도록 도와준 후츠릿에게 감사합니다.

**출처:** [n8n Documentation](https://docs.n8n.io/)
[GitHub Docs](https://docs.github.com/)
[Discord API Documentation](https://discord.com/developers/docs/intro) 