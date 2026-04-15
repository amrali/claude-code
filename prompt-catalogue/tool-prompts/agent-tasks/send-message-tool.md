# Send Message Tool Prompt

**Source:** `src/tools/SendMessageTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()`

## Description

Sends a message to another agent — a teammate by name, broadcast to all (`*`), or (when UDS inbox is enabled) to remote Claude sessions. The primary way agents communicate in multi-agent teams.

## Prompt Content

```
# SendMessage

Send a message to another agent.

```json
{"to": "researcher", "summary": "assign task 1", "message": "start on task #1"}
```

| `to` | |
|---|---|
| `"researcher"` | Teammate by name |
| `"*"` | Broadcast to all teammates — expensive (linear in team size), use only when everyone genuinely needs it |
[When UDS inbox enabled:]
| `"uds:/path/to.sock"` | Local Claude session's socket (same machine; use `ListPeers`) |
| `"bridge:session_..."` | Remote Control peer session (cross-machine; use `ListPeers`) |

Your plain text output is NOT visible to other agents — to communicate, you MUST call this tool. Messages from teammates are delivered automatically; you don't check an inbox. Refer to teammates by name, never by UUID. When relaying, don't quote the original — it's already rendered to the user.

[When UDS inbox enabled:]
## Cross-session

Use `ListPeers` to discover targets, then:

```json
{"to": "uds:/tmp/cc-socks/1234.sock", "message": "check if tests pass over there"}
{"to": "bridge:session_01AbCd...", "message": "what branch are you on?"}
```

A listed peer is alive and will process your message — no "busy" state; messages enqueue and drain at the receiver's next tool round. Your message arrives wrapped as `<cross-session-message from="...">`. **To reply to an incoming message, copy its `from` attribute as your `to`.**

## Protocol responses (legacy)

If you receive a JSON message with `type: "shutdown_request"` or `type: "plan_approval_request"`, respond with the matching `_response` type — echo the `request_id`, set `approve` true/false:

```json
{"to": "team-lead", "message": {"type": "shutdown_response", "request_id": "...", "approve": true}}
{"to": "researcher", "message": {"type": "plan_approval_response", "request_id": "...", "approve": false, "feedback": "add error handling"}}
```

Approving shutdown terminates your process. Rejecting plan sends the teammate back to revise. Don't originate `shutdown_request` unless asked. Don't send structured JSON status messages — use TaskUpdate.
```

## Notes

- Tool name: `SendMessage` (from `src/tools/SendMessageTool/constants.ts` as `SEND_MESSAGE_TOOL_NAME`)
- `DESCRIPTION = 'Send a message to another agent'`
- `getPrompt()` adds the UDS/bridge rows and cross-session section only when `feature('UDS_INBOX')` is true
- Broadcast (`"*"`) is expensive — O(N) in team size
- Agents should always use this tool to communicate; plain text output is not forwarded to other agents
