# 📐 ACTA Engenharia

Repositório de engenharia de software do ACTA, criado para organizar requisitos, fluxos, casos de uso e decisões de arquitetura que orientam a plataforma durante todo o ciclo **PDCA** (*Plan, Do, Check, Act*).

## 📌 Visão geral

O `acta-engenharia` reúne os artefatos usados para descrever o comportamento esperado do ACTA e alinhar produto, desenvolvimento e infraestrutura. O conteúdo cobre desde a autenticação e a administração de usuários até a identificação de problemas, a análise de causas, a execução de tarefas, a verificação dos resultados e o registro de lições aprendidas.

## ✨ Conteúdo do repositório

- Requisitos funcionais e não funcionais consolidados em planilha.
- Casos de uso resumidos e detalhados para cada etapa do PDCA.
- Fluxos de usuário organizados em `Plan`, `Do`, `Check` e `Act`.
- Diagramas de atividades para autenticação, análise de causas e execução de tarefas.
- Decisões de arquitetura sobre autenticação, persistência de dados e GitOps.
- Documento com a dinâmica de trabalho da equipe.
- Modelo comum para abertura e revisão de Pull Requests.

## 🧾 Requisitos

A planilha [`requisitos/requisitos.xlsx`](requisitos/requisitos.xlsx) contém o catálogo atual do produto:

| Tipo | Identificadores | Quantidade | Informações registradas |
| --- | --- | ---: | --- |
| Funcionais | `RF001` a `RF041` | 41 | Área, requisito, canal e repositórios-base |
| Não funcionais | `RNF001` a `RNF017` | 17 | Categoria, requisito e repositórios-base |

Os requisitos descrevem capacidades e qualidades esperadas do ecossistema ACTA. A confirmação do estado de implementação deve ser feita nos repositórios responsáveis por cada serviço.

## 🗺️ Diagramas

Os 28 diagramas são mantidos como arquivos-fonte PlantUML (`.puml`) e estão divididos em quatro grupos:

| Grupo | Quantidade | Finalidade |
| --- | ---: | --- |
| [Casos de uso](diagramas/casos-de-uso/) | 8 | Visões resumidas e detalhadas das quatro etapas do PDCA |
| [Fluxos de usuário](diagramas/fluxos-de-usuario/) | 12 | Sequência das principais jornadas de `Plan`, `Do`, `Check` e `Act` |
| [Atividades](diagramas/atividades/) | 5 | Processos transversais e fluxos com maior detalhamento operacional |
| [Decisões de arquitetura](diagramas/decisoes-de-arquitetura/) | 3 | Autenticação, persistência e fluxo GitOps |

### Cobertura do PDCA

| Etapa | Fluxos documentados |
| --- | --- |
| **Plan** | Criação do ciclo, identificação do problema, coleta de dados, definição de meta, análise de causas e criação do plano de ação |
| **Do** | Execução de tarefas e controle de prazo |
| **Check** | Verificação dos resultados e análise do atingimento da meta |
| **Act** | Padronização, lições aprendidas e geração de relatório |

Os casos de uso possuem uma versão resumida, adequada para visão geral, e uma versão detalhada, com atores, relações e desdobramentos do processo.

## 🛠️ Formatos e ferramentas

| Formato ou ferramenta | Uso |
| --- | --- |
| PlantUML | Modelagem dos casos de uso, atividades, sequências e decisões de arquitetura |
| Excel (`.xlsx`) | Catálogo de requisitos funcionais e não funcionais |
| PDF | Registro da dinâmica de trabalho da equipe |
| Markdown | Documentação principal e modelo de Pull Request |
| Git e GitHub | Versionamento, revisão e histórico dos artefatos |

## ✅ Pré-requisitos

Para consultar o conteúdo, basta ter o Git e um leitor compatível com PDF e XLSX.

Para editar ou renderizar os diagramas, utilize uma instalação do PlantUML ou uma extensão de editor compatível com arquivos `.puml`. O repositório não fixa uma versão específica da ferramenta.

## 🚀 Uso local

Clone o repositório:

```powershell
git clone https://github.com/AppActa/acta-engenharia.git
Set-Location .\acta-engenharia
```

Com a CLI do PlantUML disponível no `PATH`, gere versões SVG de todos os diagramas:

```powershell
Get-ChildItem .\diagramas -Recurse -Filter *.puml |
    ForEach-Object { plantuml -tsvg $_.FullName }
```

Os arquivos SVG serão gerados junto aos respectivos arquivos `.puml`. No estado atual, o repositório não contém aplicação executável, gerenciador de dependências ou pipeline próprio de renderização.

## 🏗️ Organização dos artefatos

```text
.
├── diagramas/
│   ├── atividades/
│   │   └── autenticacao/
│   ├── casos-de-uso/
│   │   ├── detalhados/
│   │   └── resumidos/
│   ├── decisoes-de-arquitetura/
│   └── fluxos-de-usuario/
│       ├── plan/
│       ├── do/
│       ├── check/
│       └── act/
├── requisitos/
│   └── requisitos.xlsx
├── Dinâmica de Trabalho da Equipe Acta.pdf
├── PULL_REQUEST_TEMPLATE.md
└── README.md
```

Cada diagrama deve permanecer no grupo correspondente ao seu propósito. Os fluxos de usuário seguem a etapa do PDCA, enquanto processos transversais, como autenticação, ficam em `diagramas/atividades/` ou `diagramas/decisoes-de-arquitetura/`, conforme o nível de abstração.

## 📚 Documentos relacionados

- [Planilha de requisitos](requisitos/requisitos.xlsx)
- [Dinâmica de Trabalho da Equipe Acta](<Dinâmica de Trabalho da Equipe Acta.pdf>)
- [Modelo de Pull Request](PULL_REQUEST_TEMPLATE.md)

## 🤝 Links e autoria

- [Repositório](https://github.com/AppActa/acta-engenharia) · [Licença MIT](LICENSE) · `acta.institutojef@gmail.com`
- Contribuições: use *issues* e *pull requests*; há um [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md).
- Autoria: Equipe ACTA.