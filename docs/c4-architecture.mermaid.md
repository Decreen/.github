# C4 Architecture

Derived from `docs/c4-events.ndjson` (append-only stream). Pass 4 adds no new elements.

## L1 — System context

```mermaid
flowchart TD
  subgraph L1["System context"]
    actor_org_member["actor:org_member<br/>Organization member"]
    actor_public_reader["actor:public_reader<br/>Public reader"]
    ext_github_hosting["ext:github_hosting<br/>GitHub platform (static hosting and rendering)"]
  end
  actor_org_member -->|push / manage content| ext_github_hosting
  actor_public_reader -->|view rendered profile| ext_github_hosting
```

## L2 — Containers

_System boundary: **sys:org_profile_suite** — Organization profile and repository documentation_

```mermaid
flowchart TD
  subgraph L2["sys:org_profile_suite"]
    container_profile_markdown["container:profile_markdown<br/>Profile landing markdown"]
    container_root_markdown["container:root_markdown<br/>Root repository markdown"]
    ext_github_hosting["ext:github_hosting<br/>GitHub platform (static hosting and rendering)"]
    actor_org_member["actor:org_member<br/>Organization member"]
    actor_public_reader["actor:public_reader<br/>Public reader"]
  end
  actor_org_member -->|author / update| container_profile_markdown
  actor_org_member -->|author / update| container_root_markdown
  container_profile_markdown -->|published as static content| ext_github_hosting
  container_root_markdown -->|published as static content| ext_github_hosting
  actor_org_member -->|push / manage content| ext_github_hosting
  actor_public_reader -->|browse| ext_github_hosting
```

## L3 — container:profile_markdown

```mermaid
flowchart TD
  subgraph L3p["%% SCOPE: urn:c4:container:profile_markdown"]
    component_profile_content_store["component:profile_content_store<br/>%% KIND: storage<br/>Authored profile markdown body"]
    component_profile_outbound_links["component:profile_outbound_links<br/>%% KIND: router<br/>Outbound hyperlinks in profile README"]
    ext_github_hosting["ext:github_hosting<br/>GitHub platform (static hosting and rendering)"]
  end
  component_profile_content_store -->|embeds links| component_profile_outbound_links
  component_profile_outbound_links -->|resolve in browser| ext_github_hosting
```

## L3 — container:root_markdown

```mermaid
flowchart TD
  subgraph L3r["%% SCOPE: urn:c4:container:root_markdown"]
    component_root_content_store["component:root_content_store<br/>%% KIND: storage<br/>Authored root README body"]
    component_root_outbound_links["component:root_outbound_links<br/>%% KIND: router<br/>Outbound hyperlinks in root README"]
    ext_github_hosting["ext:github_hosting<br/>GitHub platform (static hosting and rendering)"]
  end
  component_root_content_store -->|embeds links| component_root_outbound_links
  component_root_outbound_links -->|resolve in browser| ext_github_hosting
```

## Containment Map

```json
{
  "parentToChildren": {
    "sys:org_profile_suite": ["container:profile_markdown", "container:root_markdown"],
    "container:profile_markdown": ["component:profile_content_store", "component:profile_outbound_links"],
    "container:root_markdown": ["component:root_content_store", "component:root_outbound_links"]
  },
  "childToParent": {
    "container:profile_markdown": "sys:org_profile_suite",
    "container:root_markdown": "sys:org_profile_suite",
    "component:profile_content_store": "container:profile_markdown",
    "component:profile_outbound_links": "container:profile_markdown",
    "component:root_content_store": "container:root_markdown",
    "component:root_outbound_links": "container:root_markdown"
  }
}
```
