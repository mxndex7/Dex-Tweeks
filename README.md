# Dex Tweaks

Painel administrativo em Batch para Windows 10 e Windows 11 que reúne diagnóstico, manutenção, privacidade, configuração de hardware e ajustes de desempenho em um único arquivo executável: `Dex-Tweaks.bat`.

[![Versão](https://img.shields.io/badge/vers%C3%A3o-2.1.0-1677ff)](https://github.com/mxndex7/Dex-Tweaks/releases)
[![Plataforma](https://img.shields.io/badge/Windows-10%20%7C%2011-0078d4)](https://www.microsoft.com/windows)
[![Interface](https://img.shields.io/badge/interface-Batch-4d4d4d)](Dex-Tweaks.bat)
[![Privilégios](https://img.shields.io/badge/requer-administrador-c62828)](#requisitos)

> O Dex Tweaks altera configurações sensíveis do Windows. Leia a prévia exibida pelo painel, crie um backup e aplique somente as funções que você compreende.

## Destaques da versão 2.1

- Compatibilidade oficial com Windows 10 21H2, 22H2 e LTSC 2021 de 64 bits, a partir da build 19044.
- Detecção centralizada do Windows 10 e Windows 11, com capacidades específicas por versão.
- Interface, relatórios e preflight identificam o sistema e o modo de compatibilidade ativos.
- Rotinas de Copilot, Widgets, Notícias e Interesses e barra de tarefas usam caminhos próprios para cada versão.
- System Guard disponível no Windows 10 e 11 quando TPM, Secure Boot, UEFI e virtualização de firmware estão presentes.
- Windows Update não é mais fixado em outro produto ou versão pelas rotinas de privacidade.
- Teste de fumaça cobre builds simuladas do Windows 10 e Windows 11.

- Dashboard com visão do Defender, rede, plano de energia, build do Windows, perfil aplicado, backups e reinicialização pendente.
- Perfis gerenciados `Safe`, `Balanced`, `Competitive`, `Streaming`, `Laptop` e `Privacy`.
- Central de Alterações com prévia, validação, fila personalizada, snapshot e verificação do resultado.
- Backup e restauração com snapshot gerenciado, restauração seletiva, backup do Registro e Ponto de Restauração.
- Central de Saúde com DISM, SFC, integridade de armazenamento, eventos críticos e auditoria de segurança.
- Benchmark antes/depois para processos, serviços, memória, CPU e espaço livre.
- Pesquisa global, histórico de operações, relatórios e Central de Reinicialização.
- Dex Toolbox com catálogo de aplicativos, instalação, consulta e atualização.
- Modo Especialista para operações críticas, com confirmações e proteções adicionais.
- Testes internos estrutural e de fumaça.

## Requisitos

| Item | Requisito |
|---|---|
| Sistema | Windows cliente de 64 bits: Windows 10 build 19044 ou superior, ou Windows 11 build 22000 ou superior |
| Conta | Usuário com privilégios de administrador |
| Shell | Prompt de Comando e PowerShell disponíveis |
| Internet | Necessária para atualização, downloads, DISM e algumas ferramentas |
| Restauração | Recomendável manter a Proteção do Sistema habilitada |

O painel verifica a versão do Windows e os privilégios administrativos durante a inicialização.

### Compatibilidade com Windows 10

O Windows 10 é executado em modo de compatibilidade, mantendo o mesmo painel e praticamente o mesmo catálogo do Windows 11. Ações ligadas exclusivamente à interface do Windows 11 são ignoradas ou substituídas pelos equivalentes do Windows 10. Recursos dependentes de edição ou hardware, como Hyper-V, Windows Sandbox, HAGS, BitLocker e System Guard, continuam sujeitos à disponibilidade real do computador.

O alvo recomendado é Windows 10 22H2 build 19045. A build 19044 é aceita para incluir Windows 10 21H2 e LTSC 2021. O Windows 10 Home e Pro encerrou o suporte regular da Microsoft; mantenha ESU ativo ou utilize uma edição LTSC ainda elegível para atualizações de segurança.

## Download e execução

1. Abra a página de [Releases](https://github.com/mxndex7/Dex-Tweaks/releases).
2. Baixe o arquivo `Dex-Tweaks.bat` da versão mais recente.
3. Clique com o botão direito no arquivo e selecione **Executar como administrador**.
4. Leia e aceite o termo inicial.
5. Crie o backup oferecido antes da primeira alteração.
6. Comece pelo perfil `Safe` ou `Balanced`.

Não é necessário instalar o Dex Tweaks. O código executável permanece concentrado em um único arquivo BAT. Logs, relatórios, preferências e snapshots são criados separadamente apenas durante o uso.

## Painel principal

| Opção | Área | Finalidade |
|---|---|---|
| `1` | Dashboard | Consultar rapidamente o estado geral do computador |
| `2` | Profiles | Aplicar conjuntos coerentes de configurações |
| `3` | Optimizations | Acessar limpeza e ajustes de CPU, GPU, rede, memória e serviços |
| `4` | Hardware | Consultar e configurar armazenamento, vídeo, áudio, USB e monitor |
| `5` | Windows | Manter Explorer, Registro, recursos, pesquisa, Defender e arquivos do sistema |
| `6` | Privacy | Administrar telemetria, permissões, localização, publicidade e dados locais |
| `7` | Advanced | Acessar Toolbox, jogos, tarefas, MSI Mode, afinidade, DirectX e OBS |
| `8` | Change Center | Visualizar, aplicar e reverter mudanças gerenciadas |
| `9` | Backup / Restore | Criar e restaurar estados do sistema |
| `A` | System Health | Diagnosticar integridade, segurança e armazenamento |
| `B` | Benchmark | Registrar e comparar métricas antes e depois |
| `C` | Global Search | Localizar uma área do painel por palavra-chave |
| `D` | History | Consultar, exportar e limpar logs antigos |
| `E` | Restart Center | Administrar reinicializações pendentes |
| `X` | Exit | Encerrar o painel |

## Perfis gerenciados

| Perfil | Intenção |
|---|---|
| `Safe` | Restaurar uma base compatível, protegida e fácil de diagnosticar |
| `Balanced` | Melhorar a experiência de jogos sem desativar segurança ou atualizações |
| `Competitive` | Priorizar resposta e desempenho, com maior consumo de energia |
| `Streaming` | Equilibrar jogo, captura e atividade em segundo plano |
| `Laptop` | Preservar compatibilidade e consumo em computadores portáteis |
| `Privacy` | Reduzir coleta de dados e identificadores sem remover proteções |

Antes de aplicar um perfil, o painel apresenta a fila de ações, verifica compatibilidade e cria um snapshot gerenciado. Perfis podem ser exportados e importados por meio de `DexTweaks_Profile.queue`; códigos desconhecidos são rejeitados.

## Funcionalidades

### Desempenho e manutenção

- Limpeza de arquivos temporários, caches e resíduos do Windows.
- Ajustes de CPU, memória, GPU, rede, serviços e dispositivos de entrada.
- Atualização de componentes de rede e solução de problemas de conectividade.
- Planos de energia personalizados e configurações para jogos.
- Correções do Explorer, Registro, Windows Search e arquivos do sistema.
- Administração de recursos opcionais, aplicativos e serviços do Windows.

### Hardware

- Inventário do computador e informações dos principais componentes.
- Relatórios de saúde de discos e volumes.
- Otimizações para armazenamento, monitor, áudio e USB.
- Ferramentas para drivers NVIDIA e AMD.
- Central de Segurança de Hardware para TPM, Secure Boot e recursos relacionados.

### Navegadores e privacidade

- Configurações para Edge, Chrome, Firefox, Opera GX, Brave e Vivaldi.
- Ajustes de privacidade, permissões, telemetria, publicidade, localização e sincronização.
- Configuração de DNS e arquivo `hosts` para bloqueio de anúncios e rastreadores.
- Preservação das atualizações do navegador, Safe Browsing, SSL e SmartScreen nos fluxos protegidos.

### Ferramentas avançadas

- Dex Toolbox para instalar, localizar e atualizar aplicativos.
- Game Boosters, MSI Mode, afinidade de processos e otimizações do DirectX.
- Otimizador do OBS e perfil para streaming.
- Administração de tarefas agendadas e remoção orientada de programas.
- Temas e predefinições visuais do painel.

## Segurança e recuperação

As funções gerenciadas seguem um fluxo de prévia, validação, snapshot, aplicação, verificação e registro. Operações de maior risco, como alterações de BCD ou modos críticos do Microsoft Defender, ficam atrás do Modo Especialista e exigem confirmação adicional.

O painel oferece:

- Snapshot gerenciado antes de alterações compatíveis.
- Restauração completa ou seletiva por módulo.
- Backup completo do Registro.
- Criação e abertura da Restauração do Sistema do Windows.
- Backup do BCD antes de alterações críticas.
- Verificação de assinatura digital em instaladores NVIDIA obtidos pelo painel.
- Registro das ações e aviso de reinicialização pendente.

Backups reduzem riscos, mas não garantem recuperação em todos os cenários. Não interrompa o computador durante operações de DISM, SFC, Registro, drivers ou restauração.

## Dados gerados

Os dados persistentes ficam em:

```text
%ProgramData%\DexTweaks
```

Essa pasta pode conter configurações, estado gerenciado, snapshots, logs, relatórios e medições de benchmark. Exportações solicitadas pelo usuário são gravadas na Área de Trabalho.

## Testes internos

Execute os comandos abaixo em um Prompt de Comando:

```bat
Dex-Tweaks.bat --self-test
Dex-Tweaks.bat --smoke-test
```

O teste estrutural verifica elementos obrigatórios do script. O smoke test usa dados temporários para validar o fluxo gerenciado sem aplicar as otimizações do painel.

## Fluxo recomendado

1. Execute como administrador.
2. Crie um snapshot ou Ponto de Restauração.
3. Consulte o Dashboard e a Central de Saúde.
4. Registre uma linha de base no Benchmark.
5. Aplique primeiro um perfil `Safe` ou `Balanced`.
6. Reinicie quando o painel solicitar.
7. Verifique o histórico e capture o resultado atual do Benchmark.
8. Use as áreas tradicionais e o Modo Especialista somente quando necessário.

## Documentação

A descrição detalhada de cada função, sua intenção, efeitos e cuidados está em [DOCUMENTACAO.md](DOCUMENTACAO.md).

## Avisos

- Resultados variam conforme hardware, drivers, edição e versão do Windows e programas instalados.
- Ajustes agressivos podem afetar compatibilidade, consumo, temperatura, recursos do Windows ou segurança.
- Métricas do Benchmark são fotografias do sistema e não representam garantia de FPS ou latência.
- Revise scripts baixados da internet antes de executá-los como administrador.

## Projeto

- Autor: Mendes
- Repositório: [github.com/mxndex7/Dex-Tweaks](https://github.com/mxndex7/Dex-Tweaks)
- Versão atual: `2.1.0`
