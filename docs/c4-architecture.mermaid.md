# C4 Architecture

_Derived by replaying `docs/c4-events.ndjson` through Pass 4 freeze (`final_render`)._

## L1 — System context

```mermaid
flowchart LR
  actor_developer["actor:developer<br/>Developer or repository maintainer"]
  ext_github["ext:github<br/>GitHub (git remote hosting)"]
  subgraph sys_decreen["sys:decreen-dot-github — Decreen `.github` profile repository (GitHub-hosted)"]
    container_repo["container:repository-tree<br/>Version-controlled repository tree (files and directories)"]
  end
  actor_developer -->|"edge:developer-github<br/>Uses for clone, push, and pull"| ext_github
  actor_developer -->|"edge:developer-repo-tree<br/>Edits and maintains files"| container_repo
  container_repo -->|"edge:repo-sync-github<br/>Git remote sync (push/pull)"| ext_github
```

## L2 — Containers

```mermaid
flowchart TB
  subgraph sys_decreen["sys:decreen-dot-github — Decreen `.github` profile repository (GitHub-hosted)"]
    container_repo["container:repository-tree<br/>Version-controlled repository tree (files and directories)"]
  end
  actor_developer["actor:developer<br/>Developer or repository maintainer"]
  ext_github["ext:github<br/>GitHub (git remote hosting)"]
  actor_developer -->|"edge:developer-github"| ext_github
  actor_developer -->|"edge:developer-repo-tree"| container_repo
  container_repo -->|"edge:repo-sync-github"| ext_github
```

## L3 — Components (selected container)

```mermaid
flowchart TB
  subgraph sys_decreen["sys:decreen-dot-github"]
    subgraph container_repo["%% SCOPE: urn:c4:container:repository-tree"]
      comp_files["%% KIND: storage<br/>component:workspace-files<br/>Working tree files (e.g. root and profile paths)"]
      comp_git["%% KIND: integration<br/>component:git-configuration<br/>Git metadata and remote configuration (.git)"]
      comp_files -->|"edge:files-under-vc<br/>Paths tracked or ignored per repository rules"| comp_git
    end
  end
  ext_github["ext:github<br/>GitHub (git remote hosting)"]
  comp_git -->|"edge:git-config-github<br/>Declares origin remote and sync semantics"| ext_github
```

## Containment map

| ID | Kind | Contained in |
|----|------|----------------|
| `sys:decreen-dot-github` | system | — |
| `container:repository-tree` | container | `sys:decreen-dot-github` |
| `component:workspace-files` | component (`storage`) | `container:repository-tree` |
| `component:git-configuration` | component (`integration`) | `container:repository-tree` |
