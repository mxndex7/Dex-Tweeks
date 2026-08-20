# Dex Tweaks - Documentacao funcional

Referencia rapida das telas do `Dex-Tweaks.bat`: o que cada uma faz e o nivel de risco. Nao participa da execucao do painel.

## Sumario

1. [Requisitos e inicializacao](#requisitos-e-inicializacao)
2. [Niveis de risco](#niveis-de-risco)
3. [Painel principal](#painel-principal)
4. [Perfis](#perfis)
5. [Otimizacoes](#otimizacoes)
6. [Hardware](#hardware)
7. [Windows](#windows)
8. [Privacidade](#privacidade)
9. [Ferramentas avancadas](#ferramentas-avancadas)
10. [Central de alteracoes](#central-de-alteracoes)
11. [Backup e restauracao](#backup-e-restauracao)
12. [Saude do sistema](#saude-do-sistema)
13. [Benchmark](#benchmark)
14. [Pesquisa, historico e reinicializacao](#pesquisa-historico-e-reinicializacao)
15. [Dados gerados](#dados-gerados)
16. [Fluxo recomendado](#fluxo-recomendado)
17. [Testes internos](#testes-internos)

## Requisitos e inicializacao

| Item | Comportamento |
|---|---|
| Windows 10 | Qualquer build (10240+), roda em modo de compatibilidade com o mesmo catalogo do Windows 11 |
| Windows 11 | Build 22000+, modo nativo |
| Administrador | Verificado via `net session` no inicio |
| Termo inicial | `I agree` na primeira execucao; termo e cor ficam salvos |
| Backup inicial | Registro e Ponto de Restauracao oferecidos antes da primeira alteracao |

Recursos dependentes de edicao/hardware (HAGS, Hyper-V, Sandbox, BitLocker, System Guard) seguem sujeitos a disponibilidade real da maquina, independente da build. O painel nao forca migracao de Windows 10 para 11 nem fixa o Windows 11 em uma release especifica.

## Niveis de risco

| Nivel | Significado |
|---|---|
| Baixo | Reversivel, impacto limitado |
| Moderado | Pode mudar comportamento, consumo ou compatibilidade |
| Alto | Pode remover recursos/apps/servicos usados por outros componentes |
| Critico | Pode reduzir seguranca ou afetar a inicializacao do Windows |

Funcoes de Perfis e Central de Alteracoes seguem sempre: fila -> previa -> checagem de compatibilidade -> snapshot -> aplicacao -> verificacao -> registro. BCD e desativar o Defender exigem Modo Especialista (confirmacao dupla + snapshot + backup do BCD + Ponto de Restauracao).

## Painel principal

| Opcao | Area | Intencao |
|---|---|---|
| `1` | Dashboard | Estado geral do computador |
| `2` | Profiles | Aplicar conjuntos coerentes de configuracoes |
| `3` | Optimizations | Otimizacoes tradicionais (CPU, GPU, rede, memoria, servicos...) |
| `4` | Hardware | Componentes fisicos e drivers |
| `5` | Windows | Explorer, Registro, recursos, Search, Defender, SFC/DISM |
| `6` | Privacy | Telemetria, permissoes, localizacao, publicidade |
| `7` | Advanced | Toolbox, jogos, tarefas, MSI Mode, afinidade, DirectX, OBS |
| `8` | Change Center | Aplicar e reverter mudancas gerenciadas |
| `9` | Backup / Restore | Criar e restaurar estados do sistema |
| `A` | System Health | Diagnostico e reparo do Windows |
| `B` | Benchmark | Comparar metricas antes/depois |
| `C` | Global Search | Localizar uma area do painel por palavra |
| `D` | History | Consultar e exportar logs |
| `E` | Restart Center | Administrar reinicializacoes pendentes |
| `X` | Exit | Encerrar o painel |

## Perfis

Combinacoes predefinidas de acoes gerenciadas; nenhuma desativa protecoes do Windows.

| Perfil | Intencao | Risco |
|---|---|---|
| Safe | Base compativel: Defender, Update, Search ativos, energia balanceada, efeitos automaticos | Baixo |
| Balanced | Melhora jogos sem abrir mao de seguranca: Game Mode, HAGS, energia balanceada | Baixo/Moderado |
| Competitive | Prioriza resposta: Game Mode, HAGS, Alto Desempenho, menos efeitos visuais | Moderado |
| Streaming | Equilibra jogo e captura: Game Mode, HAGS, Alto Desempenho, restringe segundo plano | Moderado |
| Laptop | Melhora jogos sem energia agressiva: Game Mode, energia balanceada | Baixo |
| Privacy | Reduz identificadores e historico de atividade sem remover protecoes | Baixo |

O ultimo perfil aplicado pode ser exportado/importado via `DexTweaks_Profile.queue` na Area de Trabalho; codigos desconhecidos bloqueiam a importacao inteira.

## Otimizacoes

Rotinas tradicionais do projeto - em geral mais agressivas que os Perfis gerenciados.

| Item | Intencao | Risco |
|---|---|---|
| Windows Cleaner | Limpa temporarios, caches de navegador/app, MRU, cache de driver/shader | Moderado |
| BCDEdit Tweaks | Timer, interrupcoes, boot, virtualizacao, integridade de codigo | Critico |
| GPU Optimizations | Ajustes por fabricante (Intel, NVIDIA, AMD) | Moderado/Alto |
| Network Tweaks | DNS, TCP/IP, firewall; especifico para Wi-Fi/Ethernet | Moderado |
| CPU Optimizations | Energia e agendamento por fabricante (Intel, AMD Ryzen) | Moderado |
| Memory Optimizer | Paginacao, cache e prioridades de memoria | Moderado |
| Mouse/Keyboard Tweaks | Remove aceleracao, ajusta repeticao e resposta de entrada | Baixo/Moderado |
| Internet Refresher | Limpa DNS/ARP, renova IP, reinicia pilha de rede | Moderado (interrompe a conexao) |
| Service Tweaks | Desativa servicos opcionais por categoria (telemetria, OEM, Store, Bluetooth...) | Alto |
| Debloater | Remove apps e componentes por categoria (Store, Xbox, Edge, OneDrive, Copilot...) | Alto |
| Custom Power Plan | Plano de energia coerente por tipo de equipamento | Moderado |
| Browser Config | Reduz rastreamento preservando updates de seguranca, Safe Browsing e SSL | Moderado |
| Colour Presets | Cor da interface | - |

## Hardware

| Item | Intencao | Risco |
|---|---|---|
| Hardware Information | Inventario de CPU/GPU/RAM/placa-mae/BIOS/armazenamento; nao altera nada | - |
| Storage Acceleration | TRIM, cache por tipo de unidade, desfrag, write caching | Moderado |
| NVIDIA Driver Optimizer | Reduz telemetria/servicos/overlay opcionais da NVIDIA | Moderado/Alto |
| Tweaked NVIDIA Driver | Instala pacote modificado (download controlado, assinatura Authenticode verificada) | Alto |
| AMD Driver Optimization | Reduz telemetria e processos opcionais do AMD Software | Moderado/Alto |
| Hardware Security Center | Windows Hello, BitLocker, Credential/Device Guard, Core Isolation, System Guard, HVCI, Smart Card, relatorio de conformidade | Depende de TPM/Secure Boot/virtualizacao |
| Audio Optimization | Reduz latencia, evita economia de energia em audio | Moderado |
| USB Optimization | Reduz suspensao seletiva e atrasos em perifericos | Moderado |
| Monitor / Display Optimization | Resposta visual, energia e parametros de exibicao | Moderado |

## Windows

| Item | Intencao | Risco |
|---|---|---|
| Search Index Optimizer | Reduz indexacao; modo completo desativa a Busca inteira | Alto |
| Windows Defender Optimizer | Gaming (mantem protecao ativa) / Performance (desativa tempo real) / Remocao completa (bloqueada por seguranca) | Moderado / Critico / - |
| Windows Explorer Fixes | Menos animacao, thumbnails, historico de recentes | Moderado |
| System File Checker | SFC, DISM, limpeza do Component Store, reset da Store | - (15-30+ min) |
| Registry Fixes | Reduz sugestoes, anuncios e efeitos via politicas de Registro | Moderado |
| Windows Features Manager | Ativa/desativa recursos opcionais (Hyper-V, WSL, .NET, IE, containers...) por modo | Depende do modo |

## Privacidade

| Item | Intencao | Risco |
|---|---|---|
| Telemetry & Data Blocker | Reduz telemetria, diagnostico, tarefas de coleta | Alto |
| Cortana & Search Privacy | Remove Cortana, historicos e integracao web da busca | Alto |
| Account & Cloud Sync | Limita Conta Microsoft e sincronizacao na nuvem | Moderado/Alto |
| Location & Sensor Privacy | Limita localizacao, mapas, sensores | Moderado |
| App Permissions | Restringe camera, microfone, contatos, notificacoes, segundo plano | Moderado |
| Advertising & Style | Desativa ID de publicidade e sugestoes | Baixo |
| Privacy Data Cleanup | Apaga caches, historicos e rastros locais | Moderado |
| Advanced Security, DNS & Hosts Protection, Complete Privacy Audit | Aparecem no menu mas ainda sao `Coming soon` | - |

## Ferramentas avancadas

| Item | Intencao | Risco |
|---|---|---|
| Dex Toolbox | Instala/consulta/atualiza software via Chocolatey (~200 itens no catalogo, checksum obrigatorio) | - |
| Game Boosters | Ajustes especificos para Valorant, Fortnite, CS2, Warzone, Minecraft | - |
| Scheduled Tasks | Desativa tarefas de segundo plano consideradas dispensaveis | Alto |
| MSI Mode | Message Signaled Interrupts em hardware compativel | Alto |
| Program Debloat | Reduz inicializacao/telemetria/cache de Chrome, Firefox, Spotify, Steam, Discord | Moderado |
| Affinity | Distribui interrupcoes de GPU/rede/USB entre nucleos logicos | Moderado/Alto |
| DirectX Optimization | Renderizacao e latencia grafica | Moderado |
| OBS Optimizer | Configuracao de encoder (NVENC/AMF/x264) e qualidade | - |
| Theme Presets | Cor da interface | - |
| Stream Optimizer | Prioriza o jogo e reduz custo de ferramentas de streaming | Alto |

## Central de alteracoes

Mostra o estado atual de Game Mode, HAGS, Windows Search, plano de energia e Defender, com acoes individuais ou em fila.

| Acao | Reinicializacao |
|---|---|
| Game Mode on | Normalmente nao |
| HAGS on/off | Sim |
| Balanced / High Performance power | Nao |
| Search on/off | Pode ser recomendada |
| Defender protection | Pode ser recomendada |
| Restore snapshot | Sim |

Fila personalizada aplica varias acoes em lote (snapshot antes). Codigos aceitos: `DEFENDER_ON`, `UPDATES_ON`, `SEARCH_ON`, `SEARCH_OFF`, `GM_ON`, `HAGS_ON`, `HAGS_OFF`, `PWR_BAL`, `PWR_HIGH`, `VFX_AUTO`, `VFX_PERF`, `TELEMETRY_SAFE`, `ADS_OFF`, `ACTIVITY_OFF`, `DELIVERY_OFF`, `BGAPPS_OFF`.

## Backup e restauracao

| Recurso | Cobre |
|---|---|
| Snapshot gerenciado | Game Mode/DVR, HAGS, efeitos visuais, politicas usadas pelo Dex, Advertising ID, energia, startup de Update/BITS/Search, Defender, BCD |
| Restauracao | Snapshot inteiro, por modulo (Registro, servicos, energia, Defender, BCD), ou um snapshot mais antigo |
| Windows Restore Point | Restauracao nativa do Windows (exige System Protection habilitado) |
| Full Registry Backup | Hives `SOFTWARE`, `SYSTEM`, `DEFAULT`, `SECURITY`, `SAM`, `NTUSER` - recuperacao avancada via Ambiente de Recuperacao, nao para importar em sessao normal |
| Windows System Restore | Abre `rstrui.exe` |

A pasta de dados remove heranca de permissoes e concede acesso apenas a Administradores e SYSTEM.

## Saude do sistema

| Item | Faz |
|---|---|
| Quick health check | `DISM /CheckHealth` + `sfc /verifyonly`, sem reparo |
| Full Windows repair | `DISM /RestoreHealth` + `sfc /scannow`, marca reinicializacao |
| Storage health report | Discos e volumes: tipo, saude, espaco |
| Critical event report | Ate 100 eventos criticos/erro do log System (7 dias) |
| Security status audit | Defender, protecao em tempo real, comportamento, firewall, Secure Boot, TPM |

## Benchmark

Comparacao administrativa (processos, servicos, memoria livre/total, carga de CPU, uptime, espaco livre) - nao e um medidor de FPS. Fluxo: capturar baseline -> aplicar mudanca -> capturar atual -> comparar deltas. Uma amostra isolada pode variar por atualizacoes e atividade em segundo plano.

## Pesquisa, historico e reinicializacao

- **Global Search**: termos como `GPU`, `restore`, `Defender`, `streaming` retornam a area correspondente do painel (nao busca comandos internos).
- **History**: log da sessao atual, logs anteriores, exportacao, limpeza (preservando a sessao atual).
- **Restart Center**: mostra reinicializacao pendente, reinicia agora, agenda em 60s, cancela agendamento, ou limpa so o lembrete interno (nao elimina uma reinicializacao exigida pelo Windows).

## Dados gerados

```text
%ProgramData%\DexTweaks\
|-- Backups\   (Managed_* e FullRegistry_*)
|-- Logs\      (Session_YYYYMMDD_HHMMSS.log)
|-- Reports\   (dashboard, health, storage, eventos, seguranca, benchmark)
`-- State\     (aceite inicial, cor, ultimo perfil/snapshot, fila, catalogo de busca)
```

## Fluxo recomendado

**Primeiro uso:** administrador -> Ponto de Restauracao -> `System Health` (diagnostico rapido) -> baseline no Benchmark -> perfil `Safe` ou `Balanced` -> reiniciar -> capturar resultado e comparar -> so depois avaliar otimizacoes tradicionais individualmente.

**Para desfazer:** `Backup / Restore` (integral ou por modulo) -> reiniciar. Arquivos/drivers removidos: Windows System Restore. Corrupcao do Windows: DISM + SFC.

## Testes internos

```bat
Dex-Tweaks.bat --self-test
Dex-Tweaks.bat --smoke-test
```

`--self-test` valida labels duplicados, destinos de `goto`/`call`, modulos obrigatorios e navegacao. `--smoke-test` roda o fluxo gerenciado completo (deteccao de SO, dashboard, perfil, snapshot) em dados temporarios, sem aplicar tweaks reais.

## Observacoes finais

- Nenhum tweak garante aumento de FPS; meça na mesma carga e condicoes.
- Remover arquivos/apps nao e o mesmo que alterar uma configuracao - backups gerenciados restauram o que foi capturado, nao dados apagados.
- Em maquinas corporativas, politicas de dominio podem sobrescrever o Dex ou serem sobrescritas por ele.
- Perfis gerenciados sao a opcao recomendada para uso comum; BCDEdit, Defender Removal, MSI Mode e remocao de componentes sao operacoes especializadas.
