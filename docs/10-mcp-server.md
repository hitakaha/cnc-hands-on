# 10. Optional: CNC MCP server

**Time:** 20 minutes. Skip this chapter if the instructor does not start the assistant.

**Objective:** Ask the lab assistant one question that it can answer with a live Crosswork tool. Confirm the answer in the CNC UI. You decide what to do next.

The assistant is a chat page. When you ask a question, it may call tools on an MCP server. That server is what talks to CNC. The tools listed in the assistant sidebar are the only live data the assistant can fetch. Anything else in the reply is explanation until you confirm it in the UI.

## 10.1 Open the assistant

The instructor starts the assistant and the MCP server, then gives you the URL. You do not receive, and you do not type, the CNC password, an API token, or a client secret.

1. Open the assistant URL.
2. Read the backend status in the sidebar. Continue when it is online.
3. Read the tool list. Find the tool whose description is the inventory node count. That is the live tool for this lab.
4. If the sidebar also lists session or ticket tools, leave them unused. This chapter does not delete sessions and does not change the controller.

**Result:** The assistant is online and you can name the inventory tool.

## 10.2 One live question

In the chat, ask:

```text
How many nodes are in the Crosswork inventory?
```

Wait for the reply. Open the tool-call detail for that turn.

Check all of the following:

- The assistant called the inventory count tool.
- The reply contains a number.
- You can find a matching count under **Device Management > Network Devices** or **Network Inventory**. The two screens group equipment differently, so the numbers may differ by what each one counts. Write both numbers and one sentence on the difference. If they match, write that they match.

If the tool call fails, the useful symptom is the error text, not a second guess from the model. A failure here usually means the assistant cannot authenticate to CNC or cannot reach the UI address. That is an instructor fix. Chapters 2 and 4 already showed you the same inventory with your own login.

**Result:** A number from the tool, compared with Device Management.

## 10.3 Operator check, not a write

Ask a second question that does not require a write tool:

```text
The inventory count is <the number you just verified>.
The CML lab should have the routers from 5-nodes_L3VPN 8201-32FH 25.3.1-ipv6.
What should an operator check first in Crosswork?
```

A useful answer names checks you already know how to open:

- Reachability on **Device Management > Network Devices**
- The credential profile on that device
- Data Gateway mapping
- Collection job status
- SR-PCE and NSO provider status
- Active alarms for that hostname

Take one check the assistant named, open that CNC page, and confirm or contradict the suggestion with the screen. The screen wins.

## 10.4 What you leave out of the chat

- Passwords, tokens, ticket ids, and certificate contents
- A request to delete a session, a policy, a VPN, or a device
- Configuration text from another service's dry run

## Done when

- You completed this chapter, or you skipped it because the assistant was not started.
- If you used it: the assistant answered the inventory question with a tool call, and you compared that number with Device Management.
