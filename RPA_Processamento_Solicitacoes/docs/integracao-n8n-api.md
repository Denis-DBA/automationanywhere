# Integração n8n → Automation Anywhere REST API

## Pré-requisitos

- n8n operacional e workflow publicado.
- Automation Anywhere Control Room Community Edition e Bot Agent Windows conectado.
- Bot `RPA_Processamento_Solicitacoes` disponível no Control Room.
- Sessão Windows apta para execução **assistida**.

## Passo a passo

**1. Schedule Trigger**

Duas regras diárias: às **08h00** e **17h20**. Configurar o timezone do workflow para `America/Sao_Paulo`.

**2. HTTP Request — Autenticação**

- Method: `POST`
- URL: `https://community.cloud.automationanywhere.digital/v2/authentication`
- Authentication: `Generic Credential Type → Custom Auth`
- A credencial Custom Auth envia o corpo JSON com `username` e `password`, armazenados **na área de credenciais do n8n**; não colocar segredos no workflow público.
- A resposta inclui um campo `token`. Não copie esse valor para o GitHub.

**3. AA - Executar Bot**

- Method: `POST`
- URL: `https://community.cloud.automationanywhere.digital/v4/automations/deploy`
- Header `X-Authorization`: expressão `{{ $json.token }}`
- Body JSON:

```json
{
  "botId": 33417795,
  "automationName": "RPA_Processamento_Solicitacoes",
  "automationPriority": "PRIORITY_MEDIUM",
  "attendedRequest": {
    "deploymentDeviceId": 0,
    "queueDeployment": true,
    "launchInChildWindow": false
  }
}
```

O ajuste `launchInChildWindow: false` evitou o erro **Virtual window mode is not supported with user-specific bot agent installation** no ambiente testado.

## Interpretação dos resultados

A API pode retornar `deploymentId` e `automationName`. Esse retorno significa **deploy aceito**, não necessariamente bot concluído. Consulte **Operations → Activity** no Control Room. Confirme também a alteração na planilha.

## Licenciamento

A **Community Edition** utilizada não oferece **Unattended Bot Runner**. O agendamento pelo n8n fornece um disparo temporizado de um processo **assistido** e depende da disponibilidade da estação Windows e do Bot Agent. Para operação 24x7 verdadeiramente não assistida, avalie licença e infraestrutura empresariais adequadas.

## Segurança

- Definir a credencial Custom Auth no n8n do importador.
- Não versionar tokens, senhas, execuções brutas nem planilhas com dados pessoais.
- Manter a planilha fictícia no repositório.
- Evitar exposição de tokens em capturas de tela e logs.
