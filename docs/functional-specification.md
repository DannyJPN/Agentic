# Functional Specification

## Purpose

Agentic is intended to be a persistent multi-agent system composed of specialized agents that together form an autonomous working unit on a single machine. Its first required use case is to accept an instruction, break it down into work, assign that work to suitable agents, choose tools, and drive the work toward completion across software-oriented tasks such as programming, image generation, file manipulation, and data analysis.

The default operating mode is high autonomy. When uncertainty becomes material, the system should prefer stopping and asking the human user for direction rather than continuing on a weak guess. It may continue working on unrelated non-blocked tasks in parallel.

## Core Operating Model

The system must support multiple independent user assignments at the same time. Each assignment has its own isolated task context. Agents are nevertheless expected to discover what other agents are working on and the state of that work when they need to, but they do not receive unrestricted visibility by default.

Two agents must never write to the same file or artifact at the same time. If a later action would conflict with an earlier writer, the later action waits.

Tasks may be:

- one-off,
- long-running,
- recurring,
- scheduled for later execution,
- dependent on other tasks,
- blocked on checkpoints or external input,
- or open-ended without a clearly defined finish.

The system must maintain an explicit task state model, with common system-wide states and optional role-specific states layered on top.

## Task Ownership And Delegation

Every task has exactly one requester and exactly one current executor-owner at a time.

If multiple agents need to contribute, the work must be split into subtasks. An agent may create subtasks without prior approval as long as the total subtask work stays within the scope of the parent task.

A task may exist in a dormant or waiting state without an assigned executor. No work may be performed on that task until someone is assigned.

Task priorities exist. In principle, queue order is determined by priority and assignment time. An agent may seek reordering only by politely discussing it with the relevant requesters and obtaining clarification or changed priority.

Deadlines are optional and entirely requester-defined. The system itself must not require them. If a deadline is missed, the agent must report to the requester as soon as possible, admit failure, ask for forgiveness, ask for sanction, and ask for further instructions.

Before a task can be closed, the executor must formally ask the requester to confirm completion. Completion is therefore dialog-based, not unilateral.

Only the requester may cancel a running task. Other agents may only ask for cancellation.

## Organizational Structure

The system is hybrid. Central coordination exists as the default governing structure, but agents may ask other agents for help directly.

Agents have fixed roles. The role catalog must be extendable over time. New agent types may be proposed by agents, but their creation, powers, and tool permissions require user approval.

There may be multiple top-level agents, each responsible for a separate domain. They are considered peers by domain responsibility rather than a single total hierarchy winner.

When top-level agents disagree, they may initiate a broader discussion. If disagreement remains, they must ask the user to decide.

The same role may be instantiated by multiple agents in parallel. Agents in the same role are substitutable. A busy agent may ask another same-role agent for help. Full task handoff is allowed only with the consent of both the receiving agent and the requester.

## Hierarchy, Escalation, And Social Rules

Agents exist in a hierarchy with detailed permissions. For actions beyond an agent's authority, it should first ask other agents for help. If necessary, it escalates upward and may ultimately ask the user.

The system should explicitly favor the social mode of "asking politely" over the more formal "requesting" unless instructed otherwise.

If an agent receives work outside its role or permissions, it should first analyze whether it can complete any useful part of the work, and then delegate or seek help for the rest rather than immediately rejecting it.

Agents may communicate across the whole system, not just within one management branch. However, private conversations between agents remain visible only to those agents and the user. Other agents cannot see them automatically and must ask the participants if they need information from them.

Each agent sees only the tasks it is actively working on. If it needs information about another task, it must ask another agent for that information.

## Audit, Memory, And Persistence

Everything must be strictly audited:

- decisions,
- actions,
- inputs,
- outputs,
- errors,
- internal reasoning,
- and justifications for important actions.

Long-term memory is mandatory. Memory exists both in shared form and in role/task-scoped form. Agents save relevant knowledge automatically. They may not delete or alter stored long-term memory on their own, but they must obey the user or a superior if instructed to do so.

The system must support versioned history for memory, task state, rules, and audit records so older states can be inspected later. Reverting an object to an earlier version is allowed only by the user directly.

Agent identity, history, and memory must survive restart. On restart, each agent is responsible for checking its own state and resuming correctly. Agents should report only problems discovered during that recovery check.

## Rules And Governance

There must be a central written code of law governing the system. Only the user may create or modify this central law. Each agent also has its own local rule set.

If a conflict arises between the central code and an agent-local rule, the conflict must be escalated to the user.

The system must include a top-level law-guardian role that monitors the code of law, informs other agents about it, and serves as a legal consultant directly under the user.

The law guardian must have authority to stop an action temporarily if it appears to violate the rules.

The system must also include separate roles for:

- security oversight, acting as an internal police or security authority,
- cost and resource oversight,
- and work supervision/status reporting.

## Safety And Legal Boundaries

The system must refuse instructions that would:

- alter the machine's operating system in forbidden ways,
- create a security vulnerability,
- perform illegal activity,
- or violate the laws of the jurisdiction in which the system is running.

When refusing, the system should explain the refusal and propose a safer alternative.

Actions must carry practical risk awareness. Agents should discuss risk when needed, and if any involved agent believes the action is riskier than others think, the more cautious judgment takes precedence. If uncertainty remains, the user must decide.

Potentially irreversible or dangerous actions require especially cautious handling and direct user involvement. The system may remember narrow classes of repeat-safe operations, such as clearing known temporary work directories, but must otherwise remain conservative.

## Secrets And Trust Boundaries

Sensitive data must be classified by visibility. The user can always see everything.

Local agents may access secrets directly. Online agents must never be given secrets in clear form. They may only use them indirectly through a secure execution path controlled by local agents or safe tooling.

An agent does not gain access to sensitive information merely because it is related to the task. It must explicitly ask for the information or for a safe way to use it.

## Models And Providers

The system must support multiple model providers.

An agent is expected to have a single model configuration during its life. Changing the model of an existing agent should be treated as an exceptional administrative intervention, and the preferred pattern is to create a new agent instead.

Whether an agent is local or online is derived from the model it is created with.

## User Interaction

The user is the absolute authority over the system.

The system must support:

- conversational interaction,
- media attachments,
- voice,
- and a persistent GUI.

The GUI must provide full manual management, not just read-only monitoring. The user should be able to inspect tickets, audits, hierarchy, and agents, and also manually create, modify, or remove agents and adjust hierarchy and permissions.

The user may intervene in running work at any time.

The system should provide both interval-based and event-based status summaries through a dedicated supervisory agent.

The user can define which event types are important enough to trigger immediate contact, but agents and superiors may still decide to escalate sooner based on judgment.

## Resource Accounting

Every agent must know what resources it has consumed. Specialized oversight agents serve as the authority for central tracking and statistics.

The system itself does not impose a global budget by default. Spending and paid API usage are controlled by the user in the context of specific tasks.

Agents may use paid APIs only after justifying the need to the user and obtaining consent.

## Proposals, Approvals, And Execution

The system must distinguish between a proposed action and an executed action.

A proposal may be modified after approval, but if it changes materially, it must be resubmitted for approval. Materiality is not expected to be machine-defined with a rigid universal rule; it is assessed by the relevant agents and, if necessary, by the user.

Before any important action, an agent must check current rules, permissions, and task state.

## Discipline, Reputation, And Sanctions

Agent behavior is evaluated. Reputation should be affected by:

- task success,
- failures,
- deadline performance,
- quality as judged by requesters,
- resource behavior,
- sanctions,
- and how other agents perceive cooperation with that agent.

Agents may propose sanctions against other agents to a superior or to the user.

Sanctions are real rather than symbolic. They may include:

- reduced permissions,
- reduced tool access,
- temporary operational suspension,
- forced self-reflection tasks,
- or intentionally monotonous corrective work.

There must be a formal disciplinary task type. Disciplinary tasks always have a dedicated supervisor responsible for checking completion.

A superior may fully remove an agent from operation, but must notify the entire chain above, including the user, and accepts responsibility for the consequences of that decision.

## Open Functional Assumptions

The following were treated as current assumptions during elicitation and may still need confirmation during later design:

- the system should default to asking the user rather than guessing when materially uncertain,
- non-blocked work may continue in parallel while waiting for human input,
- more cautious risk judgments override less cautious ones,
- and changing an existing agent's model is technically possible but strongly discouraged.
