# Godot Installer

Instaladores Windows para Godot Engine 3 e 4, gerados com Inno Setup para as máquinas do laboratório do Instituto Federal de Mato Grosso.

[![Build Installers](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml/badge.svg)](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml)
[![Release](https://img.shields.io/github/release/carlosrabelo/godot-installer.svg)](https://github.com/carlosrabelo/godot-installer/releases)

## Destaques

- Gera instaladores Windows para Godot 3 e Godot 4 com Inno Setup
- Instala ou atualiza o Godot nas máquinas do laboratório com um clique
- Atualiza a versão do engine com uma edição de uma linha em cada `Godot.iss`
- Recompila os instaladores automaticamente no GitHub Actions quando os scripts mudam
- Publica os instaladores como GitHub Releases a partir de uma execução manual do workflow
- Mantém as instalações de Godot consistentes em todas as máquinas do laboratório

## Visão Geral

Este projeto empacota os binários oficiais do Godot Engine para Windows em instaladores Inno Setup, para que alunos e equipe do laboratório possam instalar ou atualizar o Godot sem extrair arquivos ZIP manualmente. As versões acompanham as releases estáveis mais recentes em [godotengine.org](https://godotengine.org/).

## Pré-requisitos

- **Windows 64-bit** — necessário para executar os instaladores gerados
- **Inno Setup 6** — necessário apenas se você compilar os instaladores localmente; [download](https://jrsoftware.org/isinfo.php)
- **GitHub Actions** — usado por padrão nos builds de CI (Inno Setup local não é obrigatório)

## Instalação

### Baixar uma release

1. Abra a página de [Releases](https://github.com/carlosrabelo/godot-installer/releases)
2. Baixe o instalador da linha do Godot desejada:
   - `Godot_v3.x.x-install_win64.exe`
   - `Godot_v4.x.x-install_win64.exe`
3. Execute o `.exe` e siga o assistente

### Compilar a partir do código-fonte

```bash
git clone https://github.com/carlosrabelo/godot-installer.git
cd godot-installer
```

No Windows, baixe o ZIP `win64` correspondente nas [releases do Godot](https://github.com/godotengine/godot/releases), extraia o `.exe` ao lado do `Godot.iss` correspondente e compile com o Inno Setup:

```bash
# After placing Godot_v<version>-stable_win64.exe in godot3/ or godot4/
ISCC.exe godot3\Godot.iss
ISCC.exe godot4\Godot.iss
```

Os instaladores são gerados em `godot3/Output/` e `godot4/Output/`.

## Uso

### Instalar o Godot em uma máquina do laboratório

```powershell
# Run the downloaded installer (example for Godot 4)
.\Godot_v4.7.1-install_win64.exe
```

Aceite os padrões (ou escolha um diretório customizado), opcionalmente crie um atalho na área de trabalho e finalize o assistente. Depois disso, o Godot abre pelo menu Iniciar.

### Desinstalar

Use **Apps & features** (ou o desinstalador no diretório de instalação). Quando solicitado, escolha se deseja apagar os arquivos de dados restantes na pasta de instalação.

## Estrutura do Projeto

```
godot3/Godot.iss                 # Inno Setup script for Godot 3
godot4/Godot.iss                 # Inno Setup script for Godot 4
.github/workflows/build.yml      # CI: download binaries, compile, optional release
```

## Desenvolvimento

As versões do Godot ficam em um único define por script:

```
#define MyAppVersion "4.7.1"   # godot4/Godot.iss
#define MyAppVersion "3.6.2"   # godot3/Godot.iss
```

Fluxo de trabalho:

1. Atualize `MyAppVersion` em `godot3/Godot.iss` e/ou `godot4/Godot.iss`
2. Faça push em `master` — o CI baixa o binário Windows `*-stable` correspondente e gera os dois instaladores
3. Para publicar um GitHub Release, execute **Build Installers** via `workflow_dispatch` com **Publish a GitHub Release** habilitado

Os artefatos de cada execução bem-sucedida também ficam disponíveis na página do run em Actions (`godot-installers`).

## Contribuição

1. Faça um fork do repositório
2. Crie uma branch de feature: `git checkout -b feat/description`
3. Faça o commit com Conventional Commits: `git commit -m "feat: bump Godot 4 to 4.7.1"`
4. Envie o push e abra um pull request

## Licença

Este repositório ainda não inclui um arquivo de licença. Os binários do Godot Engine permanecem sob a [licença upstream](https://godotengine.org/license/).
