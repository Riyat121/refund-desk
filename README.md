# Refund Desk with a Spine

AI agents you can trust with money, because they work inside rules, a human gate and a shared record.

Built for the **Flip the Switch** hackathon (WeMakeDevs x DevTools Academy x Switch), November 2026.

## The problem
Companies want agents to handle refunds, but refunds are money. An agent that approves everything is dangerous. An agent that asks a human about everything is useless. This project shows the middle: agents decide the easy cases, stop at the risky ones, and a human makes the final call.

## What it does
Three agents work together in one Switch room (backed by Mattermost):

1. **riyaa-records** reads the customer's request and looks up the order in `orders.json`.
2. **riyaa-policy** applies the rules in `refund_policy.md` and returns a verdict.
3. **riyaa-reply** writes the customer message based on the verdict.

Customer request -> Records -> Policy -> (human approval if needed) -> Reply

## Why Switch is central
- Agents and humans share **one room and one history**. The agents even noticed a duplicate request for an order that was already approved, and refused to double-refund.
- Agents **hand work to each other** by @mention, with no glue code.
- A **human gate**: for mid-size refunds the policy agent stops and waits until a person replies APPROVE or REJECT.
- Everything runs **locally** with free models (OpenCode), so there is no paid API.

## Demo scenarios (all tested)
| Order | Amount | What happens |
|---|---|---|
| 1001 | $20 | AUTO_APPROVE, the customer gets an approval message |
| 1002 | $320 | NEEDS_HUMAN, the flow waits, a person approves, then the reply goes out |
| 1003 | $899 | FLAG_FRAUD (over $500, 4 refunds in 90 days, address mismatch). No refund, the customer is only told the account is under review |
| 1004 | $180 | NEEDS_HUMAN, a person rejects, the customer gets a polite refusal |

## Setup
1. Install Docker Desktop and Switch Console (https://docs.switchagents.ai/switch-rooms/getting-started). Choose "Run Switch on this computer".
2. Install OpenCode, sign in, and use one of its free models.
3. Keep `orders.json` and `refund_policy.md` in a folder your agents can read.
4. In Switch Console create three agents (provider OpenCode, run location This computer) named `riyaa-records`, `riyaa-policy`, `riyaa-reply`, with the instructions from `agents/`.
5. Create a room with the Mattermost bridge, add the three agents, and paste `room-instructions.md` as the room instructions.
6. Post: `@riyaa-records Refund request for order 1001`

## Files
- `refund_policy.md`: the refund rules
- `orders.json`: sample (fake) orders
- `agents/`: instructions for the three agents
- `room-instructions.md`: instructions shown to agents in the room

## Built with
Switch, OpenCode (free models), Mattermost, Docker. Built with AI assistance (Claude) and explained by the author. All data is fake.
