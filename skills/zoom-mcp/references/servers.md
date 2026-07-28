# Zoom MCP Server Catalog

Catalog last verified against the source skill library and live endpoint checks on 2026-07-28.
Use the MCP protocol `tools/list` response as the authority when a server's tools or schemas
change.

## Endpoints

| Server | Endpoint | Matching Marketplace template |
|---|---|---|
| Zoom MCP | `https://mcp.zoom.us/mcp/zoom/streamable` | [Default](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-default.json) |
| Meetings MCP | `https://mcp.zoom.us/mcp/meeting/streamable` | [Meetings](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-meetings.json) |
| Docs MCP | `https://mcp.zoom.us/mcp/docs/streamable` | [Docs](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-docs.json) |
| Tasks MCP | `https://mcp.zoom.us/mcp/tasks/streamable` | [Tasks](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-tasks.json) |
| Revenue Accelerator MCP | `https://mcp.zoom.us/mcp/revenue_accelerator/streamable` | [Revenue Accelerator](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-revenue-accelerator.json) |
| Chat MCP | `https://mcp.zoom.us/mcp/chat/streamable` | [Chat](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-team-chat.json) |
| Whiteboard MCP | `https://mcp.zoom.us/mcp/whiteboard/streamable` | [Whiteboard](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-whiteboard.json) |

The legacy Chat alias is `https://mcp.zoom.us/mcp/team_chat/streamable`. New integrations
should use `/mcp/chat/streamable`.

## Current Tool Surfaces

| Server | Tools |
|---|---|
| Zoom MCP | `create_new_file_with_markdown`, `get_file_content`, `get_meeting_assets`, `get_recording_resource`, `hub_create_file_from_content`, `hub_get_file_content`, `recordings_list`, `search_meetings`, `search_zoom` |
| Meetings MCP | `search_meetings`, `get_meeting_assets`, `recordings_list`, `get_recording_resource` |
| Docs MCP | `create_file_with_content`, `get_file_content` |
| Tasks MCP | `get_my_tasks`, `get_task_detail`, `create_task`, `update_task`, `trash_task`, `add_comment`, `delete_task_comment`, `get_a_task_comments`, `add_tasks_assignees`, `get_assignees_of_a_task`, `remove_task_assignee`, `add_tasks_collaborators`, `get_task_collaborators`, `remove_task_collaborator`, `bulk_create_task_steps`, `get_task_step`, `list_task_steps`, `update_task_step`, `reorder_task_steps`, `delete_task_steps` |
| Revenue Accelerator MCP | `search_conversations`, `get_conversation_transcript`, `get_conversation_analysis`, `get_conversation_comments`, `get_scorecard_sessions`, `search_deals`, `get_deal_detail_v2`, `get_deal_analysis`, `get_deal_stages`, `get_deal_activities_v2`, `get_customer_accounts`, `get_customer_contacts`, `search_indicators`, `get_manager_team_and_member`, `search_internal_users` |
| Chat MCP | `zoom_chat_message_send`, `zoom_chat_message_update`, `zoom_chat_contact_add`, `zoom_chat_channel_create`, `zoom_chat_channel_update`, `zoom_chat_channel_members_add`, `zoom_chat_channel_get_by_id`, `zoom_chat_channel_member_role_update`, `zoom_chat_channel_members_list`, `zoom_chat_channels_list`, `zoom_chat_channels_search`, `zoom_chat_contacts_get_by_id`, `zoom_chat_contacts_search`, `zoom_chat_files_search`, `zoom_chat_message_get_by_id`, `zoom_chat_message_replies_list`, `zoom_chat_messages_fetch`, `zoom_chat_messages_filter`, `zoom_chat_messages_search`, `zoom_chat_sessions_recent_list` |
| Whiteboard MCP | `create_a_whiteboard_for_brainstorming`, `list_whiteboards`, `create_a_whiteboard`, `get_a_whiteboard`, `create_a_whiteboard_by_script`, `create_a_whiteboard_for_meeting_summary`, `create_a_whiteboard_for_strategy_analysis`, `add_a_whiteboard_collaborator`, `delete_a_whiteboard_collaborator`, `get_a_whiteboard_collaborator`, `update_a_whiteboard_collaborator` |

## Scope Families

Use the matching Marketplace template for the complete baseline scope set, then remove scopes
for tools the integration will not call.

| Server | Scope families |
|---|---|
| Zoom MCP | `meeting:read:search`, `meeting:read:assets`, `ai_companion:read:search`, `cloud_recording:read:list_user_recordings`, `cloud_recording:read:content`, `docs:write:import`, `docs:read:export`, `hub:write:content`, `hub:read:content` |
| Meetings MCP | `meeting:read:search`, `meeting:read:assets`, `cloud_recording:read:list_user_recordings`, `cloud_recording:read:content` |
| Docs MCP | `docs:write:import`, `docs:read:export` |
| Tasks MCP | `tasks:read:list_tasks`, `tasks:read:task`, `tasks:write:task`, `tasks:update:task`, `tasks:delete:trash_task`, `tasks:write:comment`, `tasks:delete:comment`, `tasks:read:comments`, `tasks:write:assignees`, `tasks:read:assignees`, `tasks:delete:assignees`, `tasks:write:collaborators`, `tasks:read:list_collaborators`, `tasks:delete:collaborator` |
| Revenue Accelerator MCP | `zra:read:list_conversations`, `zra:read:conversations`, `zra:read:conversation_analysis`, `zra:read:list_conversation_comments`, `zra:read:conversation_scorecards`, `zra:read:list_deals`, `zra:read:deal`, `zra:read:list_deal_activities`, `zra:read:crm_customer_contact`, `zra:read:indicator`, `zra:read:team`, `zra:read:user` |
| Chat MCP | `team_chat:*` user scopes plus `chat_channel:read` and `chat_channel:write`; see the [Chat template](../../rest-api/assets/marketplace-apps/marketplace-manifest-template-for-mcp-team-chat.json) for the current explicit set |
| Whiteboard MCP | `whiteboard:read:list_whiteboards`, `whiteboard:read:whiteboard`, `whiteboard:write:whiteboard`, `whiteboard:write:collaborator`, `whiteboard:delete:collaborator`, `whiteboard:update:collaborator`, `whiteboard:read:list_collaborators` |

## Routing Rules

- Use Zoom MCP for cross-product search and combined meeting/Docs/Hub workflows.
- Use Meetings MCP when the task is limited to meeting search, assets, or recordings.
- Use Docs MCP for dedicated document creation or content retrieval.
- Use Tasks MCP for task writes and task collaboration.
- Use Revenue Accelerator MCP only for licensed ZRA data.
- Use Chat MCP for Chat reads and writes; use the REST Team Chat skill for bulk or durable automation.
- Use Whiteboard MCP for Whiteboard operations and collaboration.
- Use the REST API for deterministic meeting CRUD; the current MCP catalog is not a replacement for that surface.
