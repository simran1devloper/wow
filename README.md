```mermaid
sequenceDiagram
    participant U as User
    participant VSE as VS Code Extension
    participant VSB as VS Code Backend
    participant MCP as MCP Code Microservice
    participant AG as Coding Agent
    participant GR as GitHub RAG
    participant GD as GitHub-Docker Agent
    participant CR as Context RAG
    participant GW as MCP Gateway

    %% Step 1: User opens and installs extension
    U->>VSE: Open project and install extension
    VSE->>U: Chatbot interface available

    %% Step 2: User query
    U->>VSE: Type query/prompt
    VSE->>VSB: Send prompt with context

    %% Step 3: Backend forwards to MCP
    VSB->>MCP: Forward request

    %% Step 4: MCP to Agent
    MCP->>AG: Send prompt/task

    %% Step 5: Agent decides actions
    AG->>GR: Request repo link (if needed)
    AG->>GD: Deploy as service (if needed)
    AG->>CR: Request additional context (if needed)

    %% Step 6: Agent completes task
    AG->>MCP: Send code/plan

    %% Step 7: MCP sends to gateway
    MCP->>GW: Forward result

    %% Step 8: Gateway sends back
    GW->>VSB: Return response
    VSB->>VSE: Convert to VS Code command

    %% Step 9: Execute and log
    VSE->>U: Execute action in VS Code
    VSE->>AG: Send logs/output

    %% Step 10: Agent iterates until resolved
    AG->>MCP: Improve/fix code
    loop Until errors resolved
        MCP->>GW: Send improved code
        GW->>VSB: Forward to backend
        VSB->>VSE: Apply changes
        VSE->>AG: Return logs/errors
    end
    VSE->>U: Final working code delivered
