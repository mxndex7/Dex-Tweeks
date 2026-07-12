# Dex Tweaks

**Dex Tweaks** é um script em Batch para Windows que reúne um conjunto de otimizações de desempenho, privacidade e estabilidade voltadas para usuários avançados e jogadores. O script possui interface interativa, verificações de pré-requisito e diversas opções de configuração para melhorar o comportamento do sistema e dos navegadores.

## Visão geral

- Nome do arquivo: `Dex-Tweaks.bat`
- Versão interna: `1.0`
- Suporte: Windows 11 ou superior (build >= 22000)
- Requer execução como Administrador
- Desenvolvido por: Mendes (`mxndex7`)
- Repositório/contato: https://github.com/mxndex7

## O que o script faz

1. Verifica se está rodando com privilégios de administrador.
2. Verifica se o Windows é compatível (Windows 11 ou acima).
3. Busca atualizações da release mais recente no GitHub.
4. Mostra aviso de aprovação do usuário antes de continuar.
5. Oferece criação de backup de registro e ponto de restauração.
6. Exibe menu interativo com categorias de tweaks:
   - Otimizações gerais do Windows
   - Ajustes de hardware e GPU
   - Ajustes de privacidade
   - Configurações de rede
   - Backup e restore point
   - Opções avançadas

## Seções principais do menu

- `Optimizações`:
  - Limpeza do Windows e remoção de arquivos temporários
  - Ajustes BCDEdit para boot, timers e compatibilidade
  - Otimização de GPU, rede, CPU e memória
  - Ajuste de plano de energia customizado
  - Configurações de navegador e privacidade

- `Hardware`:
  - Otimizações específicas para Intel, NVIDIA e AMD
  - Perfis de GPU com valores de registro e serviços
  - Modo experimental NVIDIA para máximo desempenho

- `Windows`:
  - Ajustes de configuração do sistema e serviços
  - Otimizações de boot e timers do kernel

- `Privacidade`:
  - Hardening de navegador para Edge, Chrome, Firefox, Opera GX, Brave, Vivaldi e Chromium
  - Instalação forçada de extensões como uBlock Origin, Privacy Badger e Decentraleyes
  - Configurações de hosts e DNS para bloqueio de anúncios
  - Desabilitação de telemetria e coleta de dados

- `Backup`:
  - Criação de backup de hives de registro: SOFTWARE, SYSTEM, DEFAULT, SECURITY, SAM e NTUSER
  - Ativação ou criação de ponto de restauração do Windows
  - Remoção de pontos antigos mediante escolha do usuário

- `Avançado`:
  - Configurações de BCDEdit de alto risco
  - Configuração de Timer Resolution via download de ferramenta externa
  - Outros ajustes para desempenho extremo

## Principais recursos de navegador

- Detecta quais navegadores estão instalados e aplica políticas apenas quando presentes.
- Aplica políticas de privacidade para Microsoft Edge, Google Chrome, Mozilla Firefox, Opera GX, Brave e Vivaldi.
- Configura políticas de instalação forçada de extensões de bloqueio de anúncios.
- Atualiza o `hosts` com entradas de bloqueio de anúncios.
- Configura DNS para bloqueio de malware/adware via Cloudflare.

## Planos de energia e performance

- `Desktop Ultimate Performance`:
  - Plano de energia customizado para máxima performance
  - CPU em 100% sem economias
  - Sleep e hibernação desativados
  - USB/PCI e NVMe configurados para desempenho

- `Laptop Balanced Performance`:
  - Performance alta em AC e economia em bateria
  - Ajustes inteligentes de tela e suspensão
  - Otimizações adaptadas para notebooks

## Otimizações GPU

- Intel:
  - Ajustes de DWM, driver e timer
  - Desativação de recursos de economia e pré-rendering

- NVIDIA:
  - Downloader e aplicação de NVIDIA Profile Inspector
  - Ajustes de prioridade de thread, DPC e latência
  - Desabilitação de telemetria e recursos adicionais
  - Modo experimental para máximo desempenho extremo
  - Reversão de ajustes experimenais disponível

- AMD:
  - Ativa ReBAR para GPUs compatíveis
  - Desabilita recursos de economia como Chill e DeLag
  - Ajustes de anti-aliasing, textura e DXVA
  - Desativa serviços e telemetria AMD

## Avisos importantes

- Este script altera o registro do Windows e configurações de boot.
- Algumas otimizações podem reduzir estabilidade ou reduzir recursos de segurança.
- A seção `BCDEdit` inclui opções de alto risco, incluindo desativação de DEP, hypervisor e verificação de integridade.
- É altamente recomendado criar um backup completo e ponto de restauração antes de aplicar ajustes.
- Reinicialização do sistema é obrigatória após várias mudanças.
- Use por sua conta e risco.

## Como usar

1. Clique com o botão direito em `Dex-Tweaks.bat`.
2. Selecione `Executar como administrador`.
3. Confirme com `I agree` quando solicitado.
4. Escolha as categorias e opções desejadas no menu.
5. Reinicie o Windows após aplicar os ajustes.

## Requisitos

- Windows 11 build 22000 ou superior
- Privilégios de administrador
- Conexão com a internet para verificar atualizações e baixar ferramentas

## Observações

- O script utiliza `PowerShell`, `curl` e `reg.exe` para aplicar ajustes.
- Ele pode abrir páginas de atualização e criar arquivos temporários no `%TEMP%`.
- Algumas regras são aplicadas por política de grupo local no registro.
- As alterações podem ser revertidas manualmente com `BCDEdit` ou restauração do registro.
