# Godot Installer

Instaladores Windows para Godot Engine 3 e 4, gerados com Inno Setup para as máquinas do laboratório do Instituto Federal de Mato Grosso.

[![Build Installers](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml/badge.svg)](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml)
[![Release](https://img.shields.io/github/release/carlosrabelo/godot-installer.svg)](https://github.com/carlosrabelo/godot-installer/releases)

## Destaques

- Gera instaladores Windows para Godot 3 e Godot 4 com Inno Setup
- Instala ou atualiza o Godot nas máquinas do laboratório com um clique
- Atualiza a versão do engine com uma edição de uma linha em cada `Godot.iss`
- Recompila apenas a linha do Godot cujo `Godot.iss` mudou no GitHub Actions
- Publica um GitHub Release por linha (`godot3-*` / `godot4-*`) no push para `master`
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
2. Escolha a release da linha do Godot desejada (`godot3-<version>` ou `godot4-<version>`)
3. Baixe `Godot_v<version>-install_win64.exe` e execute o assistente

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
.github/workflows/build.yml      # CI: per-line build and release
```

## Desenvolvimento

As versões do Godot ficam em um único define por script:

```
#define MyAppVersion "4.7.1"   # godot4/Godot.iss
#define MyAppVersion "3.6.2"   # godot3/Godot.iss
```

Fluxo de trabalho:

1. Atualize `MyAppVersion` na linha que deseja subir (`godot3/Godot.iss` ou `godot4/Godot.iss`)
2. Faça push em `master` — o CI recompila **somente essa linha** e publica uma release com tag `godot3-<version>` ou `godot4-<version>` (ignora se a tag já existir)
3. Para um rebuild manual, execute **Build Installers** via `workflow_dispatch`, escolha `godot3` / `godot4` / `both`, e habilite **Publish a GitHub Release** apenas quando quiser uma release

Alterações em `.github/workflows/build.yml` recompilam as duas linhas, mas não criam releases. Os artefatos sobem por linha (`godot3-installer`, `godot4-installer`).

## Contribuição

1. Faça um fork do repositório
2. Crie uma branch de feature: `git checkout -b feat/description`
3. Faça o commit com Conventional Commits: `git commit -m "feat: bump Godot 4 to 4.7.1"`
4. Envie o push e abra um pull request

## Licença

Este repositório ainda não inclui um arquivo de licença. Os binários do Godot Engine permanecem sob a [licença upstream](https://godotengine.org/license/).
