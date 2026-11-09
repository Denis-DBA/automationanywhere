# Automation Anywhere + n8n + Excel — Portfólio RPA

Projeto prático de **Robotic Process Automation (RPA)**: o **n8n agenda e autentica** uma requisição REST para o **Automation Anywhere 360**, que executa o bot `RPA_Processamento_Solicitacoes` no Windows para processar uma planilha Excel.

## Resultado demonstrado

Em **09/11/2026, às 17h20 (America/Sao_Paulo)**, o n8n executou com sucesso os três nós (`Schedule Trigger`, `HTTP Request — Autenticação`, `AA - Executar Bot`). O Automation Anywhere também registrou execução do robô com status **Completed** em teste anterior. A execução do endpoint de deploy, isoladamente, apenas confirma que a solicitação foi aceita; para validar o processamento dos dados, confira **Operations → Activity** e o Excel de saída.

## Agenda

O workflow exportado está configurado para **todos os dias às 08h00 e 17h20**, no fuso **America/Sao_Paulo**, mediante publicação/ativação do workflow e disponibilidade do ambiente.

## Por que usamos o n8n para agendar?

A **Automation Anywhere Community Edition** utilizada neste projeto permite desenvolvimento e execução **assistida**, mas **não oferece a licença de Bot Runner não assistido (Unattended)** necessária para o agendamento autônomo empresarial via Control Room. Por isso, utilizamos o **Schedule Trigger do n8n** como orquestrador, chamando a API REST do Control Room para iniciar o bot assistido.

**Limitação importante:** agendar o disparo no n8n **não transforma** a licença em Unattended. O computador Windows deve estar ligado, a sessão compatível deve estar disponível, o Bot Agent precisa estar conectado e o n8n em execução nos horários previstos. Esta é uma demonstração de integração, não uma solução de execução desassistida garantida 24x7.

## Arquitetura

```text
Schedule Trigger (08h00 / 17h20)
    ↓
HTTP Request — Autenticação (POST /v2/authentication)
    ↓ token retornado
AA - Executar Bot (POST /v4/automations/deploy)
    ↓
Automation Anywhere Bot Agent (Windows, execução assistida)
    ↓
RPA_Processamento_Solicitacoes → Excel
```

## Conteúdo

- [Projeto: RPA_Processamento_Solicitacoes](RPA_Processamento_Solicitacoes/README.md)
- [Workflow n8n sanitizado](RPA_Processamento_Solicitacoes/workflow-n8n.json)
- [Integração REST e instruções](RPA_Processamento_Solicitacoes/docs/integracao-n8n-api.md)
- [Bot — documentação da exportação](RPA_Processamento_Solicitacoes/bot/README.md)
- [Evidências — instruções](RPA_Processamento_Solicitacoes/imagens/README.md)
- [Excel — instruções](RPA_Processamento_Solicitacoes/excel/README.md)

> **Segurança:** não publique senhas, tokens de execução, credenciais n8n, arquivos `.env` ou planilhas com dados pessoais. Reconfigure a credencial **Custom Auth** após importar o workflow.
