# Sistema de Clima Organizacional — HRO / ALVF

Trabalho de Prática de Ensino (TPE) — Componente Curricular: Engenharia de Software
Professor: Radamés Pereira | Turma: AX | Semestre: 2026/1
UNOCHAPECÓ — Curso de Ciência da Computação

## Sobre

Repositório de versionamento e evidências do TPE do Sistema de Clima Organizacional do HRO/ALVF,
seguindo o roteiro de modelagem de Larman (visão, backlog, requisitos, casos de uso, diagramas UML,
GRASP, GoF/SOLID, arquitetura, deployment e testes).

## Estrutura
.
├── docs/
│ └── TPE_etapas_13_a_24.md # Conteúdo das etapas 13 a 24 (contratos, GRASP, GoF/SOLID, etc.)
├── diagramas/
│ ├── 01_casos_de_uso.puml
│ ├── 02_atividade_UC03_responder_pesquisa.puml
│ ├── 03_sequencia_UC03_responder_pesquisa.puml
│ ├── 04_comunicacao_UC06_registrar_plano_acao.puml
│ ├── 05_classes_dominio.puml
│ └── 06_deployment.puml
├── .github/ISSUE_TEMPLATE/ # Template para abrir uma Issue por história de usuário
└── TPE_FINAL_SistemaClimaOrganizacional_HRO_ALVF.docx # Documento final de entrega

## Correções aplicadas em relação à primeira avaliação

1. **UC05 ↔ UC06 (`<<extend>>`)** — seta invertida (agora `UC06 ..> UC05`) e ponto de extensão explícito
   ("indicador crítico identificado") — ver `diagramas/01_casos_de_uso.puml`.
2. **Modelo de classes de domínio** — enumerações explícitas (`StatusCampanha`, `TipoPergunta`,
   `StatusPlanoAcao`) e separação entre participação identificada (`Participacao`) e resposta
   anonimizada (`RespostaPesquisa`/`RespostaItem`) — ver `diagramas/05_classes_dominio.puml`.
3. **Evidência de versionamento** — este repositório, com histórico de commits, Issues por história
   de usuário e branches/tags (ver seção 22 do documento e `docs/COMO_CRIAR_REPOSITORIO.md`).

## Declaração de uso de IA

Utilizei apoio de IA na organização do documento e na geração dos diagramas PlantUML, com
revisão e validação final de minha responsabilidade.
