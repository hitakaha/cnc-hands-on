# 9. Proactive assurance and the MCP server

**Time:** 40 minutes.

**Objective:** See where CNC monitors service and device health, then ask the lab assistant one question that it can answer from a live Crosswork tool. You decide what to do with the answer.

## 9.1 Assurance on the controller

You already used device alarms, events, and performance policies in chapter 4. Two more CNC 7.1 applications sit above that, when they are installed on this cluster:

| Application | What an operator uses it for |
| --- | --- |
| Service Health | A service-level view of the L2VPN and L3VPN you provisioned, with symptoms against the rules the operator enabled |
| Health Insights | KPI monitoring and alerts driven by logic the operator attached to devices |

1. From the main menu or the application list, open **Service Health** if it is installed.
2. Find `PTA-L3-SEAT-<seat>` if the service view has reached it. Read the health state and one symptom or rule name. If your service is absent, the collector has not scored it yet. Note the time and continue. Scoring can lag a new provision.
3. Open **Health Insights** only if it is installed. Read one existing KPI profile. Enable no new KPI. A new profile on a shared cluster adds collection for every seat.
4. If either application is missing from this single-VM lab, record "not installed" and continue with section 9.2. The MCP exercise is the required part of this chapter.

**Result:** You know whether service health is scoring your VPN, and you left KPI enablement unchanged.

## 9.2 The assistant and MCP

The lab assistant is a chat page. When you ask a question, the assistant may call tools on an MCP server. That server is what talks to Crosswork. The tools listed in the assistant sidebar are the only live data the assistant can fetch. Anything else in the reply is explanation, and you treat it as explanation until you confirm it in the CNC UI.

The instructor starts the assistant and the MCP server before this chapter. You receive the assistant URL on the whiteboard. You do not receive, and you do not type, the controller password, an API token, or a client secret. Those stay in the instructor's configuration outside this guide.

1. Open the assistant URL.
2. Read the backend status in the sidebar. Continue when it is online.
3. Read the tool list. Find the tool whose description is the inventory node count. That is the live tool for this lab.
4. If the sidebar also lists session or ticket tools, leave them unused. This chapter does not delete sessions and does not change the controller.

**Result:** The assistant is online and you can name the inventory tool.

## 9.3 One live question

In the chat, ask:

```text
How many nodes are in the Crosswork inventory?
```

Wait for the reply. Open the tool-call detail the UI shows for that turn.

Check all of the following:

- The assistant called the inventory count tool.
- The reply contains a number.
- You can find a matching count under **Device Management > Network Devices** or **Network Inventory**. The two screens group equipment differently, so the numbers may differ by what each one counts. Write both numbers and one sentence on the difference you see. If they match, write that they match.

If the tool call fails, the useful symptom is the error text, not a second guess from the model. A failure here usually means the assistant cannot authenticate to CNC or cannot reach the UI address. That is an instructor fix. Your earlier chapters already showed you the same inventory with your own login.

**Result:** A number from the tool, compared with the number you read in Device Management.

## 9.4 Remediation stays with you

Ask a second question that does not require a write tool:

```text
The inventory count is <the number you just verified>.
The CML lab should have <the node count on the whiteboard> routers.
What should an operator check first in Crosswork?
```

A useful answer names checks you already know how to open:

- Reachability on **Device Management > Network Devices**
- The credential profile on that device
- Data Gateway mapping
- SR-PCE and NSO provider status
- Active alarms for that hostname

Take one check the assistant named, open that CNC page, and confirm or contradict the suggestion with the screen. The screen wins. The assistant does not commit, delete, or provision.

That is the remediation pattern for this lab: the assistant reads, the assistant suggests, the operator verifies, the operator changes the network only through the chapters that allow a commit.

## 9.5 What you leave out of the chat

- Passwords, tokens, ticket ids, and certificate contents
- A request to delete a session, a policy, a VPN, or a device
- Configuration text copied from another seat's dry run

## Done when

- You know whether Service Health and Health Insights are installed.
- The assistant answered the inventory question with a tool call.
- You compared that number with Device Management.
- You verified one suggested check on a CNC screen.
