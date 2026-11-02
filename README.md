# Multica Workspace Backup

Public, versioned backups of Multica workspace General Rules, agent system prompts, and explicitly selected workspace settings for [multica-seo-repo](https://github.com/multica-seo-repo). Published content is publicly readable; include only material approved for public sharing.

The [Multica skills catalog](https://github.com/multica-seo-repo/multica-skills-management) links independent skill repositories. Workspace configuration is backed up here.

## Layout

```text
config-map.json
schemas/config-map.schema.json
workspaces/
  <workspace-id>/
    general-rules.md
    settings.json
    agents/
      <agent-id>/
        system-prompt.md
        metadata.json
```

The workspace and agent directories are created when real IDs, source snapshots, and destination bindings have been confirmed. The initial repository contains no exported workspace settings or agent prompts.

## Bindings

[config-map.json](config-map.json) is the authoritative source-to-path mapping. Its format is defined by [schemas/config-map.schema.json](schemas/config-map.schema.json).

- Identify the source by its real `workspace_id`, `kind`, and `agent_id` where applicable. Names alone do not establish a binding.
- Supported kinds: `agent_prompt`, `workspace_rules`, and `workspace_settings`.
- Paths are relative to this repository. Never store personal machine paths or allow traversal outside the repository.
- Missing, invalid, or ambiguous bindings require human confirmation before publishing. Do not fabricate IDs or automatically bind matching names.
- `bindings` starts empty. Repository ID `1442434679` identifies this GitHub repository; it is not a Multica ID.
- JSON Schema checks structure. The publishing implementation must also reject duplicate source bindings, conflicting destination paths, and paths that escape the checkout after resolution.

## Publishing

1. The Governor or authorized editor applies and verifies an approved change in Multica, then preserves its exact final snapshot.
2. Use an automatic handover only after that mechanism has been verified in the actual workspace. Otherwise, preserve the handover details and ask the user to trigger the GitHub Agent manually. The Governor does not wait for publishing.
3. The GitHub Agent verifies runtime authentication, repository access, the confirmed binding, and the snapshot. If human login or access setup is needed, it gives precise instructions and preserves the pending sync.
4. Compare the snapshot with the remote baseline. Validate the approved file scope and exclude secrets. Resolve conflicts with the human or authorized editor; do not overwrite concurrent changes.
5. Commit, push, verify the published files, and record the repository, commit SHA, source snapshot reference, and sync status on the original or linked sync issue. An already-synced snapshot needs no new commit.

This repository does not create or deploy a GitHub Agent, schedule an auto-check, or automatically import GitHub edits into Multica.

## Backup scope

- `general-rules.md`: the approved workspace General Rules snapshot.
- `system-prompt.md`: the full verified prompt of the identified agent.
- `metadata.json`: non-secret identity and selected portable metadata needed to interpret the prompt.
- `settings.json`: only explicitly selected, readable settings. Record the export scope and exclusions with the backup; a partial settings export is not a complete workspace backup.

Do not commit tokens, passwords, SSH private keys, runtime credentials, or secret-bearing environment/MCP configuration. Credentials remain in the runtime's credential storage.

## Retry and restore

- Preserve the exact snapshot and completed-stage records after failure. Inspect the remote before retrying and resume only incomplete steps.
- Missing mapping, authentication, or human approval is a pending state, not successful backup.
- Retrieve an historical version for review, obtain the required human approval, and restore through the Governor or existing authorized editor. Publish the verified restored content as a new commit.
- Governor self-prompt changes and General Rules changes retain their existing approval and editing restrictions.
- Do not delete repositories, force-push, rewrite history, or perform a destructive reset to recover a failed sync.
