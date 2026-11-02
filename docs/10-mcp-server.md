# . Optional: CNC MCP server

**Time:** 20 minutes

**Objective:** Ask the lab assistant one question that it can answer with a live Crosswork tool. Confirm the answer in the CNC UI.The assistant is a chat page. When you ask a question, it may call tools on an MCP server. That server is what talks to CNC. 

## 10.1 Open the assistant

Access **Ubuntu2504-Desktop**, open Terminal and execute following:
```
$ cd aiserver
$ ./main
```
Close the browser tab.

Access **Ubuntu2204-1T-Desktop**, open terminal and execute following:
```
$ cd cnc_mcp
$ source venv/bin/activate
$ python3 cnc_mcp_server2.py
```

Open another terminal in **Ubuntu2204-1T-Desktop** and execute following:
```
$ cd webui
$ source venv/bin/actiate
$ streamlit run frontend.py
```
![dcloud](img/10/01-ai-assistant.png)

**Result:** The assistant is online and you can name the Tools.

## 10.2 One live question

In the chat, ask:

```text
How many nodes are in the CNC?
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


## Done when

- The assistant answered the inventory question with a tool call, and you compared that number with Device Management.
