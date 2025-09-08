```mermaid
sequenceDiagram
    participant User
    participant VSCode as VS Code Extension
    participant Gateway
    participant Coding as Coding Agent
    participant Parser as Parser Agent
    participant Context as GitHub Context Agent
    participant Checker
    participant Translator

    User ->> VSCode: Enter query
    VSCode ->> Gateway: Forward query
    Gateway ->> Coding: Send query

    Coding ->> Parser: Extract repo links
    Parser -->> Coding: Parsed info

    Coding ->> Context: Request repo context
    Context -->> Coding: Repo data

    Coding ->> Checker: Generated code/solution
    alt Error Detected
        Checker ->> Coding: Request fix / human feedback
    else Validated
        Checker ->> Gateway: Validated result
        Gateway ->> Translator: Request explanation
        Translator -->> Gateway: Polished response
        Gateway ->> VSCode: Final response + logs
        VSCode ->> User: Display solution
    end
