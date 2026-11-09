# RPA_Processamento_Solicitacoes

Automação criada no **Automation Anywhere 360 (Community Edition)** e acionada pelo n8n via **API REST**.

## Regras do robô

Conforme captura da implementação fornecida:

1. **Excel advanced: Open** — abre a pasta de trabalho de solicitações.
2. **Excel advanced: Get multiple cells** — carrega os registros numa tabela.
3. **Loop** — percorre as linhas da tabela.
4. **Number: To string** — converte o número da linha para texto.
5. **String: Assign** — prepara o endereço da célula de status.
6. **If** — verifica se o status é **Pendente**.
7. **Message box** — sinaliza a solicitação pendente.
8. **Excel advanced: Set cell** — altera o status para **Processado**.
9. **Number: Increment** — incrementa o contador de linha.
10. **Excel advanced: Save workbook** — grava a pasta de trabalho.
11. **Excel advanced: Close** — fecha a planilha.
12. **Message box** — indica que o processamento terminou.

Essas ações refletem a captura de tela do bot; **não constituem um pacote importável**.

## Execução agendada

A cópia exportada do workflow n8n possui duas regras diárias:

- **08h00** — manhã.
- **17h20** — tarde.

Timezone do ambiente: **America/Sao_Paulo**. O workflow precisa estar publicado e o Windows/Bot Agent disponíveis.

## Validação

1. No n8n, execute o fluxo completo ou consulte uma execução real em **Executions**.
2. Confirme sucesso na autenticação e no nó de deploy.
3. Em Automation Anywhere, acesse **Operations → Activity** e confirme **Completed**.
4. Abra a planilha de teste e confirme a atualização das linhas com status **Pendente**.

## Arquivos

- `workflow-n8n.json` — configuração importável sem ID de credencial do ambiente original.
- `bot/README.md` — limitação de exportação na Community Edition.
- `excel/README.md` — como utilizar uma planilha de demonstração.
- `imagens/README.md` — evidências gráficas previstas.

**Atenção:** a ID do bot `33417795` pertence ao ambiente do autor. Após importar, cada pessoa deverá usar um bot próprio e ajustar o `botId`, credenciais e dispositivo.
