# Implementação de SOC com Wazuh (SIEM) 🛡️

Projeto focado na centralização de logs e monitoramento de ativos para detecção de intrusão em tempo real. O foco foi estabelecer a visibilidade sobre eventos de segurança em um endpoint Linux (Kali) utilizando a arquitetura Manager-Agent.

## Arquitetura do Lab
* **Wazuh Manager:** Instância central responsável pela correlação de eventos e geração de alertas.
* **Wazuh Agent:** Implantado no Kali Linux para coleta e encaminhamento de logs via protocolo criptografado.
* **Fluxo de Dados:** Monitoramento nativo de logs de sistema e integridade de arquivos (FIM).

## Simulação de Ataque: Brute Force SSH
Para validar as regras de correlação do Wazuh, realizei um teste de estresse no serviço de autenticação do agente:

1. **Execução:** Simulação de múltiplas tentativas de login com usuários inválidos e dicionários de senhas.
2. **Monitoramento:** Acompanhamento do `auth.log` no endpoint para garantir que as falhas estavam sendo registradas localmente.
3. **Detecção:** O Wazuh Manager identificou o pico de falhas de autenticação (Event ID específico) e disparou um alerta de nível de severidade média/alta no dashboard.

### Evidência de Detecção
O dashboard abaixo confirma o registro de **6 tentativas de login inválidas**, correlacionando os eventos como uma possível tentativa de força bruta:

![Dashboard Wazuh](wazuh_dashboard_alert.png)

## Troubleshooting e Ajustes de Rede
Durante o deploy, enfrentei problemas de conectividade entre o agente e o manager devido ao isolamento das VMs.

* **Desafio:** O agente não conseguia realizar o handshake com o servidor na porta 1514/TCP.
* **Resolução:** Ajustei a interface de rede de ambas as máquinas para o modo **Bridge**, garantindo que estivessem no mesmo barramento de camada 2. Isso permitiu a atribuição de IPs na mesma sub-rede e a comunicação direta para o envio dos buffers de log.

## Conclusão Técnica
A prática demonstrou a importância da correlação de eventos. Em um cenário real, logs isolados são difíceis de auditar; com o SIEM, conseguimos transformar ruído de rede em inteligência acionável para resposta a incidentes.

---
*Laboratório focado em Blue Team e Operações de Segurança.*
