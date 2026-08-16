# Dex Tweaks

Painel administrativo em Batch para Windows 10 e Windows 11: diagnóstico, manutenção, privacidade, hardware e ajustes de desempenho em um único arquivo, `Dex-Tweaks.bat`.

[![Plataforma](https://img.shields.io/badge/Windows-10%20%7C%2011-0078d4)](https://www.microsoft.com/windows)
[![Interface](https://img.shields.io/badge/interface-Batch-4d4d4d)](Dex-Tweaks.bat)
[![Privilégios](https://img.shields.io/badge/requer-administrador-c62828)](#requisitos)

> Altera configurações sensíveis do Windows. Leia a prévia exibida pelo painel, crie um backup e aplique somente o que você entende.

## Requisitos

| Item | Requisito |
|---|---|
| Sistema | Windows 10 64 bits (build 19044+) ou Windows 11 64 bits (build 22000+) |
| Conta | Administrador |
| Shell | Prompt de Comando e PowerShell |

Windows 10 roda em modo de compatibilidade com o mesmo painel; recursos como Hyper-V, HAGS, BitLocker e System Guard seguem sujeitos à disponibilidade real do hardware/edição. Windows 10 fora do suporte regular precisa de ESU ou LTSC ativo.

## Uso

1. Baixe `Dex-Tweaks.bat` na página de [Releases](https://github.com/mxndex7/Dex-Tweaks/releases).
2. Clique com o botão direito e escolha **Executar como administrador**.
3. Aceite o termo inicial e crie o backup oferecido.
4. Comece por um perfil `Safe` ou `Balanced` na Central de Perfis.

Não requer instalação. Logs, relatórios, preferências e snapshots ficam em `%ProgramData%\DexTweaks`, criados apenas durante o uso.

## O que tem dentro

Dashboard, Perfis gerenciados (`Safe`, `Balanced`, `Competitive`, `Streaming`, `Laptop`, `Privacy`), otimizações de CPU/GPU/rede/memória/serviços, ferramentas de hardware e drivers, privacidade e navegadores, Dex Toolbox, backup/restauração, saúde do sistema, benchmark antes/depois, busca global, histórico e central de reinicialização.

Alterações gerenciadas seguem prévia → validação → snapshot → aplicação → verificação → registro. Operações de maior risco (BCD, desativar o Defender) exigem Modo Especialista e confirmação extra.

Lista completa de telas e o que cada uma faz: [DOCUMENTACAO.md](DOCUMENTACAO.md).

## Avisos

- Resultados variam conforme hardware, drivers, edição e versão do Windows.
- Ajustes agressivos podem afetar compatibilidade, consumo ou segurança.
- Benchmark é uma fotografia do sistema, não garantia de FPS ou latência.
- Revise scripts baixados da internet antes de executá-los como administrador.

## Projeto

Autor: Guilherme Mendes — [github.com/mxndex7/Dex-Tweaks](https://github.com/mxndex7/Dex-Tweaks)
