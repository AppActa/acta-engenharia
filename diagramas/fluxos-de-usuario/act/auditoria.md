# Fluxo de auditoria do padrão

## Objetivo

Verificar, em um ciclo, se um padrão publicado está sendo aplicado e registrar as respostas do auditor. O fluxo usa os recursos existentes de formulários e respostas da acta-mongo-api; não representa como implementados o agendamento, o vínculo estruturado entre padrão e auditoria ou o tratamento de não conformidades.

## Atores

- **Gestor:** prepara e publica o checklist, define os destinatários e consulta as respostas.
- **Auditor:** usuário autenticado autorizado a responder ao checklist.

## Fluxo suportado hoje

1. O gestor escolhe o ciclo e identifica o padrão que será verificado. A API atual não possui um campo de relacionamento entre um formulário `CHECKLIST` e um formulário `PADRONIZACAO`; registrar o padrão no título ou na descrição é apenas uma referência textual.
2. O gestor cria um formulário do tipo `CHECKLIST` em rascunho, com título, descrição, perguntas e destinatários:
   - `POST /api/v1/ciclos/{idCiclo}/formularios`
   - Cada pergunta informa `titulo`, `tipo`, `obrigatoria` e `opcoes`.
   - `idsUsuariosDestinatarios` é obrigatório. Lista vazia permite resposta de qualquer usuário da empresa; preenchida restringe aos usuários indicados.
3. O gestor revisa o checklist e publica alterando o status para `ATIVO`:
   - `PATCH /api/v1/formularios/{idFormulario}` com `{"status":"ATIVO"}`.
4. O auditor autorizado consulta o checklist ativo e registra as respostas:
   - `POST /api/v1/formularios/{idFormulario}/respostas` com `{"respostas":[{"idPergunta":"<uuid>","resposta":"<valor>"}]}`.
   - A identidade do auditor vem da autenticação; não é enviada no corpo.
   - Cada usuário pode responder uma vez por formulário. Uma tentativa repetida resulta em HTTP 409.
5. O gestor consulta as respostas registradas:
   - `GET /api/v1/formularios/{idFormulario}/respostas`.
   - A resposta permite verificar quem respondeu e quando. A conclusão da auditoria é avaliada pelo gestor a partir das respostas; a API não calcula um resultado agregado de conformidade.
6. Quando não for mais necessário aceitar respostas, o gestor pode inativar o formulário com `PATCH /api/v1/formularios/{idFormulario}` e `{"status":"INATIVO"}`.

Todas as rotas exigem Firebase ID Token. A API valida o ciclo na empresa do usuário e restringe cada recurso à empresa autenticada.

## Regras para o checklist

- Usar perguntas objetivas e verificáveis, com evidência solicitada quando necessária.
- Usar tipos de resposta aceitos pela API, como `SIM_NAO`, `RADIO`, `CHECKBOX`, `TEXTO`, `DATA` ou `ARQUIVO`. A interpretação de uma resposta como conforme ou não conforme deve ser explícita no texto da pergunta; não existe campo de resultado por item.
- Uma resposta representa o envio do checklist por um auditor. Como há unicidade de resposta por usuário e formulário, auditorias repetidas exigem novos formulários enquanto não existir uma entidade própria de execução de auditoria.

## Lacunas do contrato para completar o fluxo de auditoria

O fluxo acima registra formulários e respostas, mas não implementa gestão completa de auditorias. Para isso, o contrato da API precisa ser ampliado antes de a interface apresentar essas capacidades:

1. **Vínculo:** relacionar uma execução de auditoria ao padrão (`idFormularioPadrao`) e ao ciclo, com validação de empresa e ciclo.
2. **Agendamento e responsável:** persistir frequência, data prevista, auditor responsável e estado da execução.
3. **Resultado:** definir resultado por item e resultado da auditoria (por exemplo, conforme, não conforme ou inconclusiva), além das regras de preenchimento.
4. **Não conformidades:** permitir registrar descrição, evidência, responsável e prazo e relacionar o achado ao plano de ação existente.
5. **Histórico:** permitir nova execução periódica sobre o mesmo padrão sem reutilizar o mesmo checklist/respondente nem sobrescrever respostas anteriores.

Até esses contratos existirem, a interface deve limitar-se a criar/publicar checklists e registrar/consultar respostas pelos endpoints existentes. Não deve exibir agendamento, estado de auditoria ou conformidade consolidada como se fossem persistidos.

## Endpoints consultados

Contrato conferido no README e nos controllers da acta-mongo-api: `FormularioController`, `RespostaFormularioController`, `FormularioRequestDTO` e `RespostaFormularioRequestDTO`. Consulte também o Swagger da API para os esquemas completos.
