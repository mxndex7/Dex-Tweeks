# Dex Tweaks 2.1.0 - Documentacao funcional

Esta documentacao descreve as funcionalidades disponiveis no `Dex-Tweaks.bat`, a intencao de cada uma, os efeitos esperados e os principais cuidados de uso.

O Dex Tweaks e um painel administrativo para Windows 10 e Windows 11 voltado a diagnostico, manutencao, privacidade, configuracao de hardware e ajustes de desempenho. Todo o codigo executavel continua concentrado em um unico arquivo BAT. Este documento e apenas uma referencia externa e nao participa da execucao.

## Sumario

1. [Requisitos e inicializacao](#requisitos-e-inicializacao)
2. [Conceitos de seguranca](#conceitos-de-seguranca)
3. [Painel principal](#painel-principal)
4. [Dashboard](#1-dashboard)
5. [Perfis](#2-perfis)
6. [Otimizacoes](#3-otimizacoes)
7. [Hardware](#4-hardware)
8. [Windows](#5-windows)
9. [Privacidade](#6-privacidade)
10. [Ferramentas avancadas](#7-ferramentas-avancadas)
11. [Central de alteracoes](#8-central-de-alteracoes)
12. [Backup e restauracao](#9-backup-e-restauracao)
13. [Saude do sistema](#a-saude-do-sistema)
14. [Benchmark](#b-benchmark)
15. [Pesquisa, historico e reinicializacao](#c-pesquisa-global)
16. [Dados gerados](#dados-gerados)
17. [Fluxo recomendado](#fluxo-recomendado)
18. [Testes internos](#testes-internos)

## Requisitos e inicializacao

| Item | Comportamento | Intencao |
|---|---|---|
| Windows 10 | Exige sistema cliente de 64 bits e build 19044 ou superior | Oferecer o mesmo painel em modo de compatibilidade |
| Windows 11 | Exige sistema cliente de 64 bits e build 22000 ou superior | Usar o modo nativo e os recursos de interface do Windows 11 |
| Administrador | Verifica privilegios com `net session` | Garantir acesso ao Registro, servicos, BCD, DISM e configuracoes protegidas |
| Termo inicial | Solicita `I agree` na primeira execucao | Deixar claro que ajustes de sistema podem ter resultados diferentes em cada computador |
| Backup inicial | Oferece Registro e Ponto de Restauracao | Criar uma rota de recuperacao antes das primeiras alteracoes |
| Tema | Permite escolher uma cor e salva a preferencia | Manter a identidade visual entre execucoes |
| Atualizador | Consulta a ultima release no GitHub e compara a versao completa | Informar novas versoes sem substituir automaticamente o arquivo atual |

Depois da primeira aceitacao, o termo e a cor escolhida ficam salvos. O usuario ainda pode criar backups a qualquer momento pela opcao `Backup / Restore`.

### Camada de compatibilidade do sistema operacional

Na inicializacao, o painel identifica:

- Familia Windows 10 ou Windows 11;
- Build instalada;
- Tipo cliente, rejeitando Windows Server;
- Arquitetura de 64 bits;
- Capacidades vinculadas a versao, como HAGS e interface do Windows 11.

O Windows 10 build 19044 ou superior usa modo de compatibilidade. O alvo recomendado e o Windows 10 22H2 build 19045; a build 19044 inclui Windows 10 21H2 e LTSC 2021. As mesmas areas do painel permanecem disponiveis. Operacoes exclusivas da interface do Windows 11 sao substituidas por equivalentes do Windows 10 ou ignoradas sem criar chaves exclusivas do Windows 11.

Algumas funcoes dependem da edicao, do driver ou do hardware e nao apenas da build:

- HAGS requer GPU e driver WDDM compativeis;
- Hyper-V e Windows Sandbox nao existem em todas as edicoes;
- BitLocker depende da edicao e configuracao;
- System Guard requer TPM, Secure Boot, UEFI e virtualizacao de firmware;
- Pacotes AppX podem variar conforme a imagem do Windows e o fabricante.

O painel preserva o produto e a versao-alvo configurados no Windows Update. As rotinas de privacidade nao forcam Windows 10 a migrar para Windows 11 e nao fixam o Windows 11 em uma release especifica.

Windows 10 Home e Pro encerraram o suporte regular da Microsoft. Para manter atualizacoes de seguranca, use ESU ou uma edicao LTSC ainda dentro do ciclo aplicavel.

## Conceitos de seguranca

### Niveis usados nesta documentacao

| Nivel | Significado |
|---|---|
| Baixo | Mudanca normalmente reversivel e com impacto limitado |
| Moderado | Pode alterar comportamento, consumo de energia ou compatibilidade |
| Alto | Pode remover recursos, aplicativos ou servicos usados por outros componentes |
| Critico | Pode reduzir protecoes de seguranca ou alterar a inicializacao do Windows |

### Camada gerenciada

As funcoes novas de Perfis e da Central de Alteracoes seguem este fluxo:

1. Montam uma fila de mudancas.
2. Exibem uma previa.
3. Executam uma verificacao de compatibilidade por sistema, build e capacidade conhecida.
4. Criam um snapshot gerenciado.
5. Aplicam uma acao por vez.
6. Consultam o estado resultante.
7. Registram sucesso ou falha.
8. Marcam a necessidade de reinicializacao.

### Modo especialista

Alteracoes de BCD e os modos que desativam ou removem o Microsoft Defender exigem confirmacao dupla. Antes de continuar, o painel tenta criar:

- Snapshot gerenciado;
- Backup do BCD;
- Ponto de Restauracao do Windows;
- Registro da operacao no historico.

Essas protecoes reduzem o risco, mas nao transformam uma operacao critica em uma operacao segura.

## Painel principal

| Opcao | Funcionalidade | Intencao |
|---|---|---|
| `1` | Dashboard | Mostrar o estado geral do computador |
| `2` | Profiles | Aplicar conjuntos coerentes de configuracoes |
| `3` | Optimizations | Acessar as otimizacoes tradicionais |
| `4` | Hardware | Consultar e configurar componentes fisicos |
| `5` | Windows | Manter e personalizar recursos do Windows |
| `6` | Privacy | Reduzir coleta de dados e permissoes |
| `7` | Advanced | Acessar Toolbox, jogos, OBS e ferramentas tecnicas |
| `8` | Change Center | Aplicar e reverter mudancas gerenciadas |
| `9` | Backup / Restore | Criar e restaurar estados do sistema |
| `A` | System Health | Diagnosticar e reparar o Windows |
| `B` | Benchmark | Comparar metricas antes e depois |
| `C` | Global Search | Localizar uma area do painel por palavra |
| `D` | History | Consultar e exportar logs |
| `E` | Restart Center | Administrar reinicializacoes pendentes |
| `X` | Exit | Encerrar o painel |

## 1. Dashboard

O Dashboard e a tela de situacao do Dex Tweaks.

### Informacoes exibidas

- Estado do Microsoft Defender;
- Conectividade de rede;
- Plano de energia ativo;
- Build do Windows;
- Perfil gerenciado mais recente;
- Quantidade de backups gerenciados;
- Existencia de uma reinicializacao pendente.

### Acoes

| Acao | Intencao |
|---|---|
| Refresh | Atualizar as informacoes sem reiniciar o painel |
| Backup manager | Abrir diretamente a central de recuperacao |
| System health | Abrir os diagnosticos |
| Profiles | Abrir os perfis recomendados |
| Settings | Alterar tema, exportar relatorio e administrar estado salvo |

### Configuracoes do Dashboard

- Troca da cor da interface;
- Exportacao de um relatorio textual;
- Abertura da pasta de dados do Dex;
- Limpeza do perfil salvo e do lembrete de reinicializacao.

## 2. Perfis

Perfis sao combinacoes predefinidas de acoes gerenciadas. Eles priorizam consistencia e nao utilizam os modos que desativam protecoes do Windows.

### Safe

**Intencao:** recuperar uma base compativel e protegida.

- Ativa e verifica o Defender;
- Mantem Windows Update e BITS disponiveis;
- Mantem o Windows Search disponivel;
- Ativa o plano Balanceado;
- Deixa o Windows escolher os efeitos visuais.

**Risco:** baixo.

### Balanced

**Intencao:** melhorar a experiencia de jogos sem desativar seguranca ou atualizacoes.

- Inclui Defender e Windows Update;
- Ativa Game Mode;
- Solicita HAGS;
- Usa energia Balanceada;
- Mantem efeitos visuais automaticos.

**Risco:** baixo a moderado. HAGS depende do suporte da GPU e requer reinicializacao.

### Competitive

**Intencao:** priorizar resposta e desempenho para jogos competitivos.

- Mantem o Defender;
- Ativa Game Mode e HAGS;
- Ativa Alto Desempenho;
- Reduz efeitos visuais;
- Desativa distribuicao P2P do Windows Update.

**Risco:** moderado. Pode aumentar consumo, temperatura e ruido.

### Streaming

**Intencao:** equilibrar jogo e captura de video.

- Mantem o Defender;
- Ativa Game Mode e HAGS;
- Usa Alto Desempenho;
- Mantem efeitos visuais automaticos;
- Restringe atividade de aplicativos em segundo plano.

**Risco:** moderado. Aplicativos dependentes de segundo plano podem ter comportamento diferente.

### Laptop

**Intencao:** melhorar jogos sem impor configuracoes agressivas de energia.

- Mantem Defender e Windows Update;
- Ativa Game Mode;
- Usa o plano Balanceado;
- Mantem efeitos visuais automaticos.

**Risco:** baixo.

### Privacy

**Intencao:** reduzir identificadores e publicacao de atividades sem remover protecoes.

- Mantem Defender e Windows Update;
- Configura o menor nivel de diagnostico suportado;
- Desativa o identificador de publicidade;
- Desativa publicacao e upload do historico de atividades;
- Desativa distribuicao P2P de atualizacoes.

**Risco:** baixo.

### Importacao e exportacao

- O ultimo perfil aplicado pode ser exportado para `DexTweaks_Profile.queue` na Area de Trabalho.
- Um arquivo com esse nome pode ser importado novamente.
- Todos os codigos sao validados por uma lista permitida antes da importacao.
- Codigos desconhecidos bloqueiam o perfil inteiro.

## 3. Otimizacoes

Este menu preserva as rotinas tradicionais do projeto. Varias delas sao mais agressivas que os Perfis gerenciados.

### 3.1 Windows Cleaner

**Intencao:** liberar espaco e remover dados temporarios.

Pode limpar:

- Temporarios do Windows;
- Caches de navegadores e aplicativos;
- Logs e relatorios de falha;
- Historicos MRU;
- Caches de drivers e shaders.

**Risco:** moderado. Arquivos temporarios, historicos e caches removidos nao sao recuperados pelo snapshot gerenciado.

### 3.2 BCDEdit Tweaks

**Intencao:** experimentar configuracoes de temporizador, interrupcoes, boot e virtualizacao.

Inclui configuracoes de:

- Dynamic tick, platform tick e TSC;
- APIC, PCI e temporizadores;
- Boot UX e diagnosticos de inicializacao;
- Hypervisor, VBS e virtualizacao;
- Integridade de codigo, ELAM e DEP/NX.

**Risco:** critico. Pode reduzir seguranca, impedir recursos de virtualizacao ou afetar a inicializacao. Usa Modo Especialista, snapshot, backup do BCD e confirmacao dupla.

### 3.3 GPU Optimizations

**Intencao:** aplicar ajustes especificos ao fabricante da GPU.

- Intel: energia, memoria e configuracoes graficas;
- NVIDIA: perfil padrao, modo experimental e opcao de reversao;
- AMD: configuracoes de driver e desempenho.

O modo NVIDIA experimental aplica valores avancados no Registro e nao deve ser usado como configuracao padrao.

**Risco:** moderado a alto.

### 3.4 Network Tweaks

**Intencao:** ajustar parametros conforme o tipo de conexao.

- Wi-Fi;
- Ethernet;
- Ajustes universais TCP/IP, DNS, firewall e resolucao de nomes.

Pode alterar DNS, cache, offloads, adaptadores, firewall, LLMNR, NetBIOS e sincronizacao de horario.

**Risco:** moderado. Redes corporativas, compartilhamentos e VPNs podem depender de configuracoes removidas.

### 3.5 CPU Optimizations

**Intencao:** configurar energia, agendamento e Registro conforme a CPU.

- Intel;
- AMD Ryzen;
- Ajustes universais.

**Risco:** moderado. Resultados variam conforme geracao, firmware e arquitetura hibrida.

### 3.6 Memory Optimizer

**Intencao:** ajustar paginacao, cache e prioridades de memoria.

- Detecta a quantidade de RAM;
- Configura gerenciamento de memoria;
- Ajusta memoria virtual;
- Altera prioridades de alguns componentes.

**Risco:** moderado. Alteracoes de pagefile e memoria devem ser avaliadas em cargas pesadas.

### 3.7 Mouse/Keyboard Tweaks

**Intencao:** reduzir aceleracao e tornar a entrada mais previsivel.

- Remove aceleracao e suavizacao do mouse;
- Ajusta repeticao do teclado;
- Altera filas e resposta de entrada;
- Desativa recursos de acessibilidade que podem interferir em jogos.

**Risco:** baixo a moderado. Usuarios que dependem de acessibilidade devem evitar a remocao desses recursos.

### 3.8 Internet Refresher

**Intencao:** reparar rapidamente problemas de conectividade.

- Limpa DNS e ARP;
- Renova IP;
- Reinicia a pilha de rede;
- Redefine adaptadores e parametros;
- Pode configurar servidores DNS.

**Risco:** moderado. A conexao e interrompida durante o processo.

### 3.9 Service Tweaks

**Intencao:** reduzir servicos considerados opcionais para um determinado uso.

Categorias disponiveis:

- Telemetria do Windows;
- Servicos OEM;
- Rede e compartilhamento;
- Microsoft Store e Xbox;
- Telemetria de terceiros;
- Perifericos de jogos;
- Atualizadores automaticos;
- Search, SysMain e outros servicos de desempenho;
- Seguranca e backup opcionais;
- Bluetooth;
- Impressao e imagem;
- Hyper-V e maquinas virtuais;
- Recursos e aplicativos do Windows.

O modo rapido pergunta por grupos. O modo avancado permite configurar categorias ou desativar todos os servicos opcionais.

**Risco:** alto. Desativar atualizadores, seguranca, impressao, Bluetooth, Store ou virtualizacao remove funcionalidades reais.

### 3.10 Debloater

**Intencao:** remover aplicativos e componentes que o usuario nao utiliza.

Pergunta separadamente sobre:

- Microsoft Store;
- Xbox;
- Edge;
- OneDrive;
- Copilot;
- Cortana/Search;
- Skype e Teams;
- WebView2;
- Notificacoes de seguranca;
- Aplicativos OEM;
- Aplicativos de sistema;
- Photos, Paint, Camera e Snipping Tool;
- App Installer e OpenSSH;
- Servicos e inicializacao.

**Risco:** alto. Edge, WebView2, Store e App Installer podem ser dependencias de outros programas. A remocao de arquivos protegidos do Xbox possui confirmacao adicional.

### 3.11 Custom Power Plan

**Intencao:** criar um plano coerente para o tipo de equipamento.

- Desktop Ultimate Performance;
- Laptop Balanced Performance;
- Consulta do plano atual.

Configura CPU, armazenamento, USB, grafico, rede e suspensao.

**Risco:** moderado. O perfil de desktop pode aumentar consumo e temperatura.

### 3.12 Browser Config

**Intencao:** reduzir rastreamento sem desativar atualizacoes de seguranca.

- Suporte a Edge, Chrome, Firefox e navegadores Chromium;
- Politicas de privacidade;
- Instalacao orientada de extensoes;
- Bloqueio de alguns dominios no arquivo `hosts`;
- Safe Browsing e validacao SSL preservados;
- Servicos e tarefas de update mantidos ativos.

**Risco:** moderado. Politicas forcadas podem impedir que o usuario altere algumas preferencias no navegador.

### 3.13 Colour Presets

**Intencao:** personalizar a cor da interface.

A selecao e persistida para as proximas execucoes.

## 4. Hardware

### 4.1 Hardware Information

**Intencao:** gerar uma visao ampla da maquina.

- CPU, GPU, RAM, placa-mae e BIOS;
- Armazenamento;
- Temperaturas quando expostas pelo hardware;
- Saude e estabilidade;
- Relatorio exportavel.

Nao altera configuracoes.

### 4.2 Storage Acceleration

**Intencao:** ajustar armazenamento conforme SSD, NVMe ou HDD.

- TRIM;
- Cache e prefetch;
- Agendamento de desfragmentacao;
- Write caching;
- Energia do controlador;
- Parametros NTFS e drivers.

**Risco:** moderado. Nem todo ajuste e adequado para todos os controladores ou politicas corporativas.

### 4.3 NVIDIA Driver Optimizer

**Intencao:** reduzir servicos, telemetria e inicializacao de componentes NVIDIA opcionais.

Permite decidir sobre:

- NVIDIA App/GeForce Experience;
- Overlay e ShadowPlay;
- Atualizacao automatica do driver;
- Servicos e tarefas;
- Configuracoes 3D;
- Bloqueio de hosts de telemetria.

**Risco:** moderado a alto. Desativar update exige manutencao manual.

### 4.4 Tweaked NVIDIA Driver

**Intencao:** instalar o pacote NVIDIA modificado apresentado pelo painel.

Protecoes aplicadas:

- Download controlado;
- Extracao em diretorio temporario;
- Rejeicao de caminhos arbitrarios;
- Exigencia de assinatura Authenticode valida da NVIDIA no `setup.exe`.

**Risco:** alto. Substitui o driver atual e exige reinicializacao.

### 4.5 AMD Driver Optimization

**Intencao:** reduzir telemetria e processos opcionais do AMD Software.

- Detecta GPU;
- Permite desativar servicos;
- Permite desativar metricas;
- Permite desativar atualizacao automatica;
- Ajusta tarefas, dominios e configuracoes do driver.

**Risco:** moderado a alto.

### 4.6 Hardware Security Center

**Intencao:** configurar recursos de seguranca baseados em hardware.

| Funcao | Intencao |
|---|---|
| Windows Hello | Ativar PIN e biometria |
| BitLocker | Configurar criptografia de unidade |
| Credential Guard | Isolar credenciais usando virtualizacao |
| Device Guard | Restringir execucao e reforcar integridade |
| Core Isolation | Ativar Memory Integrity |
| System Guard | Proteger inicializacao e firmware |
| HVCI | Aplicar integridade de codigo no hipervisor |
| Smart Card | Configurar autenticacao por certificado |
| Compliance Report | Gerar relatorio de TPM, Secure Boot, UEFI e virtualizacao |
| Automated Setup | Aplicar uma base recomendada de protecoes |

Essas funcoes podem exigir TPM, Secure Boot, virtualizacao e drivers compativeis.

### 4.7 Audio Optimization

**Intencao:** reduzir latencia e evitar economia de energia em dispositivos de audio.

**Risco:** moderado. Pode aumentar consumo ou alterar aprimoramentos do fabricante.

### 4.8 USB Optimization

**Intencao:** reduzir suspensao seletiva e atrasos em perifericos.

**Risco:** moderado. Pode aumentar consumo, especialmente em notebooks.

### 4.9 Monitor / Display Optimization

**Intencao:** configurar resposta visual, energia e parametros de exibicao.

**Risco:** moderado. O resultado depende do monitor, GPU e conexao utilizada.

## 5. Windows

### 5.1 Search Index Optimizer

**Intencao:** reduzir atividade de indexacao.

O modo completo desativa Windows Search, limpa o indice e remove integracoes web/Cortana.

**Risco:** alto. A pesquisa do Menu Iniciar, de arquivos e de conteudo deixa de funcionar. A Central de Alteracoes oferece opcoes gerenciadas para ligar ou desligar somente o servico.

### 5.2 Windows Defender Optimizer

| Modo | Intencao | Risco |
|---|---|---|
| Gaming | Reduzir carga de scan mantendo protecao ativa | Moderado |
| Performance | Desativar protecao em tempo real e nuvem | Critico |
| Complete Removal | Desativar e remover componentes do Defender | Critico |

O modo Gaming mantem Real-Time Protection, Behavior Monitoring e Network Protection e nao cria exclusoes amplas de pastas de jogos.

Performance e Complete Removal usam Modo Especialista. O perfil Safe pode restaurar as protecoes gerenciadas, mas arquivos fisicamente removidos podem exigir DISM, SFC ou Restauracao do Sistema.

### 5.3 Windows Explorer Fixes

**Intencao:** reduzir animacoes, rastreamento de recentes e custo de renderizacao do Explorer.

- Thumbnails e preview;
- Historico de arquivos;
- Taskbar;
- Area de notificacao;
- Extensoes e elementos visuais.

**Risco:** moderado. Afeta a experiencia visual e a navegacao.

### 5.4 System File Checker

**Intencao:** reparar a imagem e os arquivos do Windows.

- SFC;
- DISM;
- Limpeza do Component Store;
- Manutencao do WinSxS;
- Reset da Microsoft Store;
- Limpeza de temporarios.

Pode levar de 15 a 30 minutos ou mais.

### 5.5 Registry Fixes

**Intencao:** reduzir sugestoes, anuncios e efeitos visuais por politicas de Registro.

- Windows Tips;
- Spotlight;
- Timeline e Activity History;
- Explorer;
- Responsividade;
- Entradas relacionadas a bloatware.

**Risco:** moderado.

### 5.6 Windows Features Manager

| Modo | Intencao |
|---|---|
| Gaming | Manter recursos de jogos e remover componentes considerados desnecessarios |
| Performance | Desativar recursos mais pesados |
| Privacy | Remover integracoes e sincronizacao selecionadas |
| Developer | Ativar recursos de desenvolvimento |
| View | Exibir recursos habilitados e desabilitados |
| Custom | Administrar recursos individualmente |

O gerenciador personalizado cobre Hyper-V, WSL, Virtual Machine Platform, Media, .NET, Internet Explorer, Containers, Print e componentes legados.

O modo Privacy preserva os executaveis do SmartScreen. O modo Performance pode reduzir bastante a funcionalidade do Windows.

## 6. Privacidade

### 6.1 Telemetry & Data Blocker

**Intencao:** reduzir telemetria, diagnosticos, tarefas e dominios de coleta.

**Risco:** alto quando aplicado de forma completa. Pode afetar diagnostico, Insider, feedback e suporte.

### 6.2 Cortana & Search Privacy

**Intencao:** remover Cortana, historicos e integracao web da pesquisa.

**Risco:** alto. Pode afetar a pesquisa do Windows.

### 6.3 Account & Cloud Sync

**Intencao:** limitar integracao de Conta Microsoft, sincronizacao e conteudo em nuvem.

**Risco:** moderado a alto. Configuracoes e preferencias podem deixar de sincronizar.

### 6.4 Location & Sensor Privacy

**Intencao:** limitar localizacao, mapas, sensores e historico de posicao.

**Risco:** moderado. Clima, mapas, Find My Device e aplicativos de localizacao podem parar de funcionar.

### 6.5 App Permissions

**Intencao:** restringir acesso de aplicativos a camera, microfone, contatos, notificacoes e segundo plano.

**Risco:** moderado. Aplicativos podem precisar de permissoes reativadas manualmente.

### 6.6 Advertising & Style

**Intencao:** desativar identificadores de publicidade, sugestoes e personalizacao promocional.

**Risco:** baixo.

### 6.7 Privacy Data Cleanup

**Intencao:** apagar caches, historicos, logs e outros rastros locais.

**Risco:** moderado. Dados apagados nao sao restaurados pelo snapshot de configuracoes.

### 6.8, 6.9 e 6.10

As opcoes `Advanced Security`, `DNS & Hosts Protection` e `Complete Privacy Audit` aparecem no menu, mas atualmente direcionam para `Coming soon`. Elas ainda nao executam uma funcionalidade propria.

## 7. Ferramentas avancadas

### 7.1 Dex Toolbox

**Intencao:** centralizar instalacao e manutencao de software via Chocolatey.

O catalogo possui 208 entradas:

- 159 instalacoes automaticas;
- 49 itens web, exclusivos de outro sistema ou que exigem instalacao manual.

Categorias:

- Navegadores, VPN, seguranca e Office;
- Produtividade, mensagens, midia e desenvolvimento;
- Ferramentas de sistema, arquivos e runtimes;
- Design, jogos, e-mail e DevOps.

Funcoes:

- Pesquisa de pacotes;
- Instalacao com validacao de checksum;
- Lista de instalados;
- Consulta de atualizacoes;
- Atualizacao em lote;
- Desinstalacao protegida.

O Dex recusa `--ignore-checksums`. Falhas de integridade interrompem a instalacao.

### 7.2 Game Boosters

**Intencao:** aplicar configuracoes especificas a jogos.

Jogos presentes:

- Valorant;
- Fortnite;
- Counter-Strike 2;
- Warzone;
- Minecraft.

Os ajustes podem envolver prioridade, afinidade, Game Mode, configuracoes de processo e parametros especificos.

### 7.3 Scheduled Tasks

**Intencao:** desativar tarefas de segundo plano consideradas dispensaveis.

**Risco:** alto. Tarefas de manutencao, atualizacao e seguranca nao devem ser removidas sem entender a dependencia.

### 7.4 MSI Mode

**Intencao:** configurar Message Signaled Interrupts em hardware compativel para reduzir latencia de interrupcao.

**Risco:** alto. Um dispositivo ou driver incompativel pode apresentar falha ou instabilidade.

### 7.5 Program Debloat

**Intencao:** reduzir inicializacao, telemetria, cache e recursos opcionais de programas instalados.

Rotinas disponiveis:

- Chrome;
- Firefox;
- Spotify;
- Steam;
- Discord;
- Selecao de programa.

**Risco:** moderado. Preferencias e atualizacoes dos aplicativos podem ser alteradas.

### 7.6 Affinity

**Intencao:** distribuir interrupcoes de dispositivos entre processadores logicos.

A rotina detecta nucleos fisicos e processadores logicos e configura politicas de afinidade para:

- GPU;
- Adaptador de rede;
- Controladores USB.

Quando Hyper-Threading ou SMT e detectado, tambem aplica mascaras especificas de afinidade aos controladores suportados.

**Risco:** moderado a alto. Afinidade fixa pode piorar desempenho em CPUs hibridas ou com quantidade diferente de nucleos.

### 7.7 DirectX Optimization

**Intencao:** ajustar renderizacao, latencia e configuracoes graficas do Windows.

**Risco:** moderado. O impacto depende do jogo, API e driver.

### 7.8 OBS Optimizer

**Intencao:** gerar uma configuracao coerente de encoder e qualidade.

Encoders:

- NVIDIA NVENC;
- AMD AMF/VCE;
- x264 por CPU.

Niveis:

- Performance;
- Balanced;
- Quality.

### 7.9 Theme Presets

Abre a mesma selecao persistente de cores do painel.

### 7.10 Stream Optimizer

**Intencao:** priorizar o jogo e reduzir o custo de ferramentas de streaming.

Pode configurar prioridade, afinidade, Game Bar, HAGS, pagefile, aplicativos em segundo plano, cache e parametros especificos de streaming/jogo.

**Risco:** alto. A rotina tradicional possui suposicoes sobre processos e nucleos de CPU. Para uma configuracao mais conservadora, use o perfil Streaming gerenciado.

## 8. Central de alteracoes

A Central de Alteracoes mostra o estado atual de:

- Game Mode;
- HAGS;
- Windows Search;
- Plano de energia;
- Microsoft Defender.

### Acoes individuais

| Acao | Intencao | Reinicializacao |
|---|---|---|
| Game Mode on | Priorizar jogos e desativar captura em segundo plano | Normalmente nao |
| HAGS on/off | Controlar agendamento de GPU por hardware | Sim |
| Balanced power | Voltar ao plano equilibrado | Nao |
| High Performance | Priorizar desempenho | Nao |
| Search on/off | Controlar indexacao e pesquisa | Pode ser recomendada |
| Defender protection | Restaurar protecoes monitoradas | Pode ser recomendada |
| Restore snapshot | Voltar ao ultimo estado gerenciado | Sim |

### Fila personalizada

Permite adicionar varias acoes, revisar a lista e aplicar tudo em uma unica sessao. Um snapshot e criado antes do lote.

### Codigos permitidos

`DEFENDER_ON`, `UPDATES_ON`, `SEARCH_ON`, `SEARCH_OFF`, `GM_ON`, `HAGS_ON`, `HAGS_OFF`, `PWR_BAL`, `PWR_HIGH`, `VFX_AUTO`, `VFX_PERF`, `TELEMETRY_SAFE`, `ADS_OFF`, `ACTIVITY_OFF`, `DELIVERY_OFF` e `BGAPPS_OFF`.

## 9. Backup e restauracao

### Snapshot gerenciado

**Intencao:** guardar somente os estados administrados pela camada nova.

Pode conter:

- Chaves de Game Mode e Game DVR;
- HAGS e Graphics Drivers;
- Efeitos visuais;
- Politicas do Windows utilizadas pelo Dex;
- Advertising ID;
- Plano de energia;
- Startup de Windows Update, BITS e Search;
- Preferencias relevantes do Defender;
- Backup do BCD;
- Metadados.

### Restauracao do snapshot

Pode restaurar todo o snapshot mais recente ou apenas:

- Registro gerenciado;
- Servicos;
- Plano de energia;
- Defender;
- BCD.

Tambem e possivel escolher um snapshot gerenciado mais antigo.

### Windows Restore Point

**Intencao:** permitir que o proprio Windows restaure configuracoes, drivers e componentes monitorados pelo System Restore.

Depende de System Protection estar habilitado na unidade do sistema.

### Full Registry Backup

Salva hives como `SOFTWARE`, `SYSTEM`, `DEFAULT`, `SECURITY`, `SAM` e `NTUSER`.

**Intencao:** fornecer material de recuperacao avancada.

Esses arquivos nao devem ser importados durante uma sessao normal do Windows. A restauracao integral deve ser feita pelo Ambiente de Recuperacao ou com suporte tecnico.

### Windows System Restore

Abre `rstrui.exe` para escolher um Ponto de Restauracao existente.

### Protecao dos backups

A pasta de dados remove heranca de permissoes e concede controle a Administradores e SYSTEM. Isso protege especialmente copias de `SAM` e `SECURITY`.

## A. Saude do sistema

### Quick health check

- `DISM /CheckHealth`;
- `sfc /verifyonly`;
- Salva codigos de resultado e relatorio.

**Intencao:** diagnosticar sem iniciar reparo completo.

### Full Windows repair

- `DISM /RestoreHealth`;
- `sfc /scannow`;
- Marca reinicializacao pendente.

**Intencao:** reparar Component Store e arquivos protegidos.

### Storage health report

Lista discos fisicos e volumes, incluindo tipo de midia, saude, estado operacional, tamanho e espaco disponivel.

### Recent critical event report

Coleta ate 100 eventos criticos ou de erro do log System nos ultimos sete dias.

### Security status audit

Registra:

- Defender;
- Real-Time Protection;
- Behavior Monitoring;
- IOAV;
- Firewall;
- Secure Boot;
- TPM.

## B. Benchmark

O Benchmark e uma comparacao administrativa, nao um medidor de FPS.

### Metricas

- Quantidade de processos;
- Servicos em execucao;
- Memoria livre e total;
- Carga de CPU;
- Tempo ligado;
- Espaco livre na unidade do sistema.

### Fluxo

1. Capture a baseline antes das alteracoes.
2. Aplique o perfil ou ajuste.
3. Capture o estado atual.
4. Compare os valores e deltas.

Uma unica amostra pode variar por atualizacoes, programas abertos e atividade em segundo plano.

## C. Pesquisa global

**Intencao:** localizar rapidamente uma secao do painel.

Termos como `GPU`, `restore`, `Defender`, `streaming`, `disk` e `services` retornam codigos de navegacao.

A busca cobre as areas principais, nao cada comando interno.

## D. Historico

Funcoes:

- Exibir o log da sessao atual;
- Listar logs anteriores;
- Exportar o log atual;
- Apagar logs antigos preservando a sessao atual.

O historico registra a camada gerenciada e operacoes de seguranca integradas. Nem todo comando das rotinas legadas possui um evento individual.

## E. Central de reinicializacao

Funcoes:

- Mostrar se o Dex ou o Windows detectaram reinicializacao pendente;
- Reiniciar imediatamente;
- Agendar reinicializacao em 60 segundos;
- Cancelar desligamento/reinicializacao agendada;
- Limpar apenas o lembrete interno do Dex.

Limpar o lembrete nao elimina uma reinicializacao exigida pelo Windows.

## Dados gerados

Diretorio principal:

```text
%ProgramData%\DexTweaks
```

Estrutura:

```text
DexTweaks\
|-- Backups\
|   |-- Managed_YYYYMMDD_HHMMSS\
|   `-- FullRegistry_YYYYMMDD_HHMMSS\
|-- Logs\
|-- Reports\
`-- State\
```

### Logs

`Logs\Session_YYYYMMDD_HHMMSS.log`

### Relatorios

- Dashboard;
- Health;
- Repair;
- Storage;
- Critical Events;
- Security;
- Benchmark baseline e atual.

### Estado persistente

- Aceitacao inicial;
- Cor;
- Ultimo perfil;
- Ultimo snapshot;
- Fila;
- Lembrete de reinicializacao;
- Catalogo de pesquisa.

## Fluxo recomendado

Para um primeiro uso:

1. Execute como Administrador.
2. Crie um Ponto de Restauracao.
3. Abra `System Health` e execute o diagnostico rapido.
4. Capture uma baseline no Benchmark.
5. Use primeiro o perfil `Safe` ou `Balanced`.
6. Reinicie quando solicitado.
7. Capture o estado atual e compare.
8. So depois avalie otimizacoes tradicionais individualmente.

Para desfazer:

1. Use `Backup / Restore`.
2. Selecione restauracao integral ou por modulo.
3. Reinicie.
4. Se o problema envolver arquivos removidos ou drivers, use Windows System Restore.
5. Para corrupcao do Windows, use DISM e SFC.

## Testes internos

### Auto-teste estrutural

```bat
Dex-Tweaks.bat --self-test
```

Valida:

- Labels duplicados;
- Destinos ausentes de `goto` e `call`;
- Modulos obrigatorios;
- Estrutura de navegacao.

### Smoke test

```bat
Dex-Tweaks.bat --smoke-test
```

Valida sem aplicar tweaks:

- Deteccao simulada de Windows 10 e Windows 11;
- Bloqueio de builds abaixo do minimo;
- Inicializacao do runtime;
- Consultas do Dashboard;
- Montagem e validacao de perfil;
- Criacao de snapshot em pasta temporaria;
- Limpeza dos dados de teste.

## Observacoes finais

- Nenhum tweak garante aumento de FPS.
- Desempenho deve ser medido na mesma carga e nas mesmas condicoes.
- Remocao de arquivos e aplicativos nao e equivalente a alterar uma configuracao.
- Backups gerenciados restauram o que foi capturado, nao dados temporarios apagados.
- Drivers e firmware devem permanecer compativeis com o hardware.
- Em computadores corporativos, politicas de dominio podem sobrescrever o Dex ou ser sobrescritas por ele.
- Perfis gerenciados sao a opcao recomendada para uso comum.
- BCDEdit, Defender Removal, MSI Mode e remocao de componentes devem ser tratados como operacoes especializadas.
