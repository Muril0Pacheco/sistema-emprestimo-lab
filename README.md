# Sistema de Empréstimo de Equipamentos do Laboratório 💻

Repositório destinado à entrega da atividade "Diagnóstico de Ferramental e Confronto de Paradigmas" da disciplina de Projeto de Software.

## Sobre o Projeto
Este projeto analisa e modela um mini-caso de controle de empréstimo de equipamentos (notebooks, projetores, kits de robótica) para alunos e professores. A análise confronta dois paradigmas de desenvolvimento:
- **Visão Estruturada:** Modelagem dos processos utilizando Diagramas de Fluxo de Dados (DFD).
- **Visão Orientada a Objetos:** Modelagem das entidades utilizando Diagramas de Classes.

## Toolchain (Ferramentas Utilizadas)
- **Modelagem Visual:** Visual Paradigm
- **Ambiente de Desenvolvimento:** Visual Studio Code
- **Controle de Versão:** GitHub Desktop

---

## 📊 Visualização dos Modelos

### DFD Nível 0 (Visão Estruturada)
```mermaid
flowchart TD
    Ent[Aluno / Professor]
    Proc0((0. Sistema de Empréstimo\nde Equipamentos))
    
    Ent -- "Solicitação, Dados,\nDevolução" --> Proc0
    Proc0 -- "Confirmação, Prazo,\nBloqueio" --> Ent
```
---
### DFD Nível 1 (Visão Estruturada)
```mermaid
flowchart TD
    Ent[Aluno / Professor]
    P1((1. Verificar\nSolicitante))
    P2((2. Verificar\nDisponibilidade))
    P3((3. Registrar\nEmpréstimo))
    P4((4. Registrar\nDevolução))
    D1[(D1. Usuários)]
    D2[(D2. Equipamentos)]
    D3[(D3. Empréstimos)]
    D4[(D4. Bloqueios)]

    Ent -->|Dados da solicitação| P1
    P1 --> D1
    D1 -->|Usuário autorizado| P2
    P2 --> D2
    D2 -->|Equipamento disponível| P3
    P3 -->|Grava empréstimo| D3
    Ent -->|Devolução de equipamento| P4
    P4 -->|Registra devolução| D3
    P4 -->|Verifica atraso| D4
```
---
### Diagrama de Classes (Visão Orientada a Objetos)
```mermaid
classDiagram
    class Solicitante {
        -id: int
        -nome: String
        -matricula: String
        -bloqueado: boolean
        +podeEmprestar() bool
        +verificarBloqueio() void
    }

    class Aluno {
        +limiteItens: int = 1
        +prazoDias: int = 2
        +validarLimite() void
    }

    class Professor {
        +limiteItens: int = 3
        +prazoDias: int = 7
        +validarLimite() void
    }

    class Equipamento {
        -id: int
        -nome: String
        -tipo: String
        -disponivel: boolean
        +emprestar() void
        +devolver() void
        +estaDisponivel() bool
    }

    class Emprestimo {
        -id: int
        -dataRetirada: Date
        -dataPrevista: Date
        -dataDevolucao: Date
        +registrar() void
        +registrarDevolucao() void
        +verificarAtraso() boolean
    }

    class Bloqueio {
        -dataInicio: Date
        -dataFim: Date
        -ativo: boolean
        +aplicar() void
        +verificarValidade() void
    }

    Solicitante <|-- Aluno
    Solicitante <|-- Professor
    Solicitante "1" --> "*" Emprestimo : realiza
    Emprestimo "*" --> "1" Equipamento : contém
    Emprestimo --> Bloqueio : pode gerar
