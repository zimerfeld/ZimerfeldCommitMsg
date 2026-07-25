---
tipo: conhecimento
projeto: GitExtensions.ZimerfeldCommitMsg
lang: es-ES
atualizado: 2026-07-04
tags: [conhecimento, readme, instalacao, build, uso, conventional-commits, i18n]
fonte: README.md
versao: 1.0.97
---

# 📖 README — Instalação, Uso e Build

> 🇧🇷 Portugués → [[📖 README — Instalação, Uso e Build (PT)|📖 README — Instalação, Uso e Build]] · 🇺🇸 English → [[📖 README — Instalação, Uso e Build (EN)]]

> Espejo del `README.md` de la raíz del repositorio (multilingüe EN/PT/ES), reconciliado con el código el 2026-07-04.
> Nota de proyecto: [[📦 GitExtensions.ZimerfeldCommitMsg (ES)|GitExtensions.ZimerfeldCommitMsg]]. Conceptos en [[📜 Conventional Commits - Conceitos (ES)|Conventional Commits - Conceptos]].
> El `build.ps1` sella versión + fecha en los READMEs **y** en las notas del cofre que reflejan la versión actual — ver [[🏷️ Versionamento (ES)|Versionado]].

Plugin para **[GitExtensions](https://gitextensions.github.io/)** que **genera mensajes de commit automáticamente** analizando el contenido real de los cambios en stage. Los cambios se clasifican por los tipos de **Conventional Commits** para elegir el **verbo**; el mensaje es un **subject iniciado por verbo** (sin el prefijo `tipo:`) más un **cuerpo en viñetas**. Salida **multilingüe** (pt-BR / inglés / español), detectada por el SO con override manual.

## ✨ Funcionalidades de alto nivel
- **Generación automática** a partir del contenido del diff en stage — no solo de los nombres de archivo.
- **Verbo guiado por Conventional Commits** (`feat`/`fix`/`docs`/`test`/`chore`/`build`/`refactor`) — el tipo **no** aparece en el mensaje. Ver [[✍️ Subject iniciado por verbo sem prefixo de tipo (ES)|Subject iniciado por verbo]].
- **Dos estrategias** — comentarios del diff (principal) + nombres de archivo (fallback). Ver [[🔀 Duas estratégias - comentários e nomes de arquivo (ES)|Dos estrategias: comentarios y nombres de archivo]].
- **Vocabulario por repositorio** — `.zimerfeldcommitmsg.json` extiende vocabulario/conceptos sin recompilar. Ver [[📓 RepoVocabularyConfig (ES)|RepoVocabularyConfig]].
- **Multilingüe (PT-BR / EN / ES)** — automático por el SO + override (dropdown de 4 elementos y setting).
- **Autorrelleno** al abrir el diálogo y al stage/unstage; **no destructivo**.

## 🧩 Cómo funciona
Al abrir el diálogo de commit, el plugin lee el `git diff --cached`, clasifica los cambios y rellena la caja de mensaje. Detalles en [[⚙️ Geração de mensagem a partir do diff (ES)|Generación de mensaje a partir del diff]] y [[🔍 1 - Detecção do diálogo de commit (ES)|1 - Detección del diálogo de commit]].

## 🗣️ Idioma
Dos formas de elegir (rótulos multilingües):
1. **Dropdown de templates** del diálogo de commit — cuatro elementos (Automático / Portugués / Inglés / Español), elección por commit.
2. **Configuración → Plugins → ZimerfeldCommitMsg** — selector "Idioma da mensagem / Message language" (predeterminado del auto-refresh y del menú Plugins).

| Opción | Comportamiento |
|---|---|
| `Automático/Automatic` | **Por defecto.** Detecta por el SO (`pt-*` → portugués; `es-*` → español; otro → inglés) |
| `Português/Portuguese` | Fuerza pt-BR |
| `Inglês/English` | Fuerza inglés |
| `Espanhol/Español` | Fuerza es-ES |

Ejemplo lado a lado:
| Português-BR | English | Español |
|---|---|---|
| `Implementa autenticação` | `Implement authentication` | `Implementa autenticación` |
| `- Adiciona autenticação` | `- Add authentication` | `- Añade autenticación` |
| `- Adiciona processamento de pagamento` | `- Add payment processing` | `- Añade procesamiento de pagos` |

## 📦 Instalación
**Vía el Plugin Manager de GitExtensions:** busca *ZimerfeldCommitMsg* (Plugins → Plugin Manager), instala y reinicia.

**Manual:** ejecuta `build.ps1` (como Administrador para deploy automático), o copia `GitExtensions.Plugins.ZimerfeldCommitMsg.dll` a `C:\Program Files\GitExtensions\Plugins\`, o ejecuta `tools\install.ps1` como Administrador.

> El nodo **ZimerfeldCommitMsg** solo aparece en **Configuración → Plugins** después de que la DLL con el selector de idioma se instala y GitExtensions se reinicia.

## ✅ Requisitos
- GitExtensions 6.x (.NET 9)
- `git` en el `PATH` (el generador ejecuta `git diff --cached`)

## 🛠️ Build
```powershell
pwsh .\build.ps1          # incrementa versión, build Release, empaqueta el .nupkg
pwsh .\build.ps1 -Force   # siempre recompila/reempaqueta
```
Ver [[🏷️ Versionamento (ES)|Versionado]] y [[🛠️ build.ps1 (ES)|build.ps1]].

## 💜 Apoya el proyecto
**GitHub Sponsors:** [github.com/sponsors/zimerfeld](https://github.com/sponsors/zimerfeld) · **Ko-fi:** [ko-fi.com/C0D621FCGD](https://ko-fi.com/C0D621FCGD). Badges en la parte superior del README (versión + descargas de NuGet).

## 📄 Licencia
Copyright © 2026 Zimerfeld — **CC BY-NC-ND 4.0** (`LICENSE.txt`).

## 🔗 Relacionado
- [[📦 GitExtensions.ZimerfeldCommitMsg (ES)|GitExtensions.ZimerfeldCommitMsg]]
- [[📜 Conventional Commits - Conceitos (ES)|Conventional Commits - Conceptos]]
- [[⚙️ Geração de mensagem a partir do diff (ES)|Generación de mensaje a partir del diff]]
- [[🔑 Fatos-Chave (ES)|Hechos Clave]]
