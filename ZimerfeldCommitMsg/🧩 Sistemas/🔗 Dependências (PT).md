---
tipo: sistema
projeto: GitExtensions.ZimerfeldCommitMsg
lang: pt-BR
atualizado: 2026-07-04
tags: [dependências, assemblies, gitextensions, nuget]
---

# 🔗 Dependências

> 🇺🇸 English → [[🔗 Dependências (EN)]] · 🇪🇸 Español → [[🔗 Dependências (ES)]]

## 🧩 Assemblies do GitExtensions (referências de compilação)

Ambas referenciadas com `<Private>false</Private>` — **não** copiadas para o output (o host já as fornece em runtime).

| Assembly | Caminho | Uso |
|---|---|---|
| `GitExtensions.Extensibility.dll` | `refs\` (versionado no repo) | `IGitPlugin`, `GitPluginBase`, `IGitUICommands`, `ISetting`/`ChoiceSetting`, `CommitTemplateManager` |
| `System.ComponentModel.Composition.dll` | `refs\` (versionado no repo) | MEF — `[Export(typeof(IGitPlugin))]` |

> **Build determinístico (qualquer máquina Windows):** os assemblies de referência ficam **versionados em `refs\`** (apontados por `$(GitExtensionsRefPath)` no `.csproj`), **não** baixados em prebuild. Garante compilação reprodutível e **offline**. Um download anterior podia trazer o asset arm64 (6.0.5.75) incompatível com o x64 instalado (6.0.5.18375) — daí a versionagem em `refs\` (ver `refs/README.md`). O `.csproj` demove o aviso `MSB3277` (conflito benigno entre o ref pack net10 4.0 e o VS.Threading 8.0 do host — resolvido em runtime).

## 📦 Dependência do pacote NuGet (marcador do Plugin Manager)

```xml
<dependency id="GitExtensions.Extensibility" version="[7.0.0, 8.0.0)" />
```

> [!important] Por que a dependência marcadora existe
> O Plugin Manager do GitExtensions filtra o feed do nuget.org por pacotes que **dependem** de
> `GitExtensions.Extensibility`. **Sem** essa dependência, o pacote é publicado mas **nunca aparece**
> no Plugin Manager interno. Além disso, o filtro casa a **faixa de versão** da dependência com a
> versão que o **manager anuncia** para o host em execução (**não** o runtime instalado): o manager
> v7.x do GitExtensions 7.x anuncia `7.0.0`, então a faixa precisa **conter** 7.0.0 → `[7.0.0, 8.0.0)`.
> O plugin é compilado contra a **Extensibility 7.2** (net10): o GE7 mudou as assinaturas de
> `AddCommitTemplate`/`StartCommitDialog`, então uma DLL compilada para GE6 fazia os templates **sumirem**
> no dropdown do host 7.x. Um valor solto como `1.0.0.129` significa `>= 1.0.0.129`, que **não** inclui 7.0.0,
> e o pacote seria **silenciosamente filtrado para fora** do Plugin Manager. Para voltar ao **GitExtensions 6**
> (manager v3.x anuncia `0.4.0`), usar `[0.4.0, 0.5.0)` e a Extensibility 6.x. Alinhado ao [[GitExtensions.ZimerfeldTree]].

## 📦 Empacotamento (nuspec)

- DLL em **`lib\` raiz** (grupo "any" que o Plugin Manager extrai) — gera o aviso **NU5101**, intencional e filtrado no `build.ps1`. Ver [[🏷️ Versionamento (PT)|🏷️ Versionamento]].
- Mesma DLL também em `tools\net10.0-windows\` para o install via **Package Manager Console** (`install.ps1`).
- `LICENSE.txt` (CC BY-NC-ND 4.0, `type="file"`), `README.md`/`README.pt-BR.md`/`README.en-US.md`, e `icon-128.png` (em `images\`) no pacote.

## 🔑 Interfaces-chave usadas

### `IGitPlugin` (via `GitPluginBase`)
- `Register(IGitUICommands)` / `Unregister(IGitUICommands)` — captura/limpa o commands, registra/desregistra o template de commit e assina/desassina `Application.Idle`
- `Execute(GitUIEventArgs)` — menu Plugins → ZimerfeldCommitMsg
- `GetSettings()` — expõe o `ChoiceSetting` de idioma

### `IGitUICommands` / host
- `Module.WorkingDir` — working dir usado para rodar `git diff --cached`
- `CommitTemplateManager` — registro dos itens de template (um por idioma) no dropdown do diálogo de commit
- `FormCommit` (via reflection sobre `Application.OpenForms`) — a caixa de mensagem a preencher

## ✅ Runtime (o que o usuário precisa ter)

| Requisito | Valor |
|---|---|
| GitExtensions | 7.x (.NET 10) |
| .NET | 10.0 (Windows) — fornecido pelo host |
| `git` | no `PATH` (o gerador roda `git diff --cached`) |
| PowerShell | 5.1+ (scripts de build/deploy) |
| .NET 10 SDK + nuget | para compilar e empacotar |

## 🔗 Ligações

- [[🏗️ Arquitetura (PT)|🏗️ Arquitetura]]
- [[🏷️ Versionamento (PT)|🏷️ Versionamento]]
- [[⚙️ CommitMessageGenerator (PT)|⚙️ CommitMessageGenerator]]
