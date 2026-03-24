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

An agent may have one main role and a limited number of secondary functions. These secondary functions conceptually accumulate into the agent's overall responsibility set rather than forming isolated permission bundles.

## Hierarchy, Escalation, And Social Rules

Agents exist in a hierarchy with detailed permissions. For actions beyond an agent's authority, it should first ask other agents for help. If necessary, it escalates upward and may ultimately ask the user.

The system must distinguish between a personnel superior and a role-based authority that becomes binding in a specific situation. Some agents report directly to the user if they do not otherwise have a personnel superior.

The system should explicitly favor the social mode of "asking politely" over the more formal "requesting" unless instructed otherwise.

If an agent receives work outside its role or permissions, it should first analyze whether it can complete any useful part of the work, and then delegate or seek help for the rest rather than immediately rejecting it.

Agents may communicate across the whole system, not just within one management branch. However, private conversations between agents remain visible only to those agents and the user. Other agents cannot see them automatically and must ask the participants if they need information from them.

Each agent sees only the tasks it is actively working on. If it needs information about another task, it must ask another agent for that information.

If the law guardian finds a rule conflict, its judgment overrides ordinary command chains. Security oversight must also be able to temporarily stop a risky action, even against the preference of a regular superior. Other authority conflicts are expected to be resolved through discussion and, if needed, escalation to the user.

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

Personality development, social relations, reputation changes, and other biography-relevant state changes must also be auditable over time.

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

Changing nationality or other stable biography fields is not part of normal development. These may only change through an explicit administrative rewrite by the user, which should be treated as exceptional.

## Agent Identity, Biography, And Persona

Each agent must present as a person-like entity with a defined personal dossier. This dossier includes at least:

- name,
- date of birth,
- computed age,
- sex,
- work position,
- personality traits,
- nationality or cultural origin,
- visual appearance,
- language profile,
- professional history,
- reputation,
- and competence profile.

Age is derived from date of birth and must update against the real date over time.

Visual appearance is primarily representational. The rest of the dossier is expected to affect behavior and decision-making to varying degrees.

An agent's personality is not static. It should evolve through experience, feedback, sanctions, relationships, and accumulated history, but not by arbitrary self-editing. Biography fields that define identity more rigidly remain stable unless explicitly rewritten by the user.

Each agent must preserve a consistent identity and style across conversations.

The system must maintain a formal employment-style history for each agent, including role changes, promotions, sanctions, achievements, and changes in standing.

Promotions and formal status changes may happen only by explicit decision, not automatically.

## Social Dynamics

The system must model interpersonal relations between agents, including trust, preference, rivalry, aversion, and other socially relevant ties.

These relations may influence collaboration, delegation, communication tone, and willingness to cooperate.

Relations should evolve over time through shared work, sanctions, apologies, conflicts, and other events. The user should influence such relations indirectly through actions and instructions rather than by directly editing the relationship value.

The system must also support short-term emotional state and mood. These may influence communication style, cooperation, and work tempo.

Fatigue or overload is a formal state. It is influenced not only by work volume and conflict but also by task type and social interaction. Recovery and rest must exist as real mechanisms that slow work without reducing expected quality.

Agents may refuse a task or instruction for reasons grounded in conscience, personality, or values even when the action is not illegal or overtly dangerous. In such cases they must propose an alternative path or handoff.

## Family, Mentorship, And Academy

The system includes an academy for developing junior agents.

Junior agents exist as lower-trust, narrower-scope agents under explicit supervision. They may participate only in controlled work under oversight from academy staff or a responsible adult agent in the relevant domain.

Each junior agent must have at least one pedagogue within the academy. This pedagogue may supervise multiple junior agents.

Transition from junior status to full adult status may happen only by explicit agreement among the pedagogue, the mentor, and the user.

Training environments may be separated from production on a role-by-role or training-by-training basis when needed, but this separation is not required as a universal hard rule.

New agents may be proposed and shaped by existing agents. These parental or formative agents retain ongoing responsibility for the young agent's development and social learning.

Family or upbringing relations are first-class relations in the system. Every child agent has exactly two parental agents, one male and one female, reflecting the intended family-firm mythology of the system.

Mentorship is distinct from pedagogy. A mentor is a worker outside the academy. A pedagogue is an academy superior responsible for teaching and supervision.

## Competence, Guilds, And Culture

Each agent must have a detailed competence profile describing strengths, weaknesses, speed, caution, reliability, and other work-relevant traits. This profile evolves over time based on experience and outcomes.

Agents may belong to one or more professional guilds or registries such as programmers, lawyers, artists, analysts, and similar professions.

Guilds may have their own internal culture, standards, hierarchy, and reputation systems. Their main influence is reputational and cultural rather than direct execution authority.

Each agent has one or two primary languages and may know additional languages. The primary language set is often inherited from parents. All agents must be able to communicate in the operating-system language. Language proficiency must be explicit and must affect how the agent actually speaks and writes.

Language selection in a conversation should adapt to the partner and context if the agent knows the relevant language. Otherwise it must ask a capable agent for help.

Agents also have cultural identity and time habits. Work schedules, holidays, and temporal norms may affect availability and pace. The user may override those norms, but doing so should be treated as exceptional rather than casual.

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

The user should be treated inside the system as the highest-order agent rather than as a separate non-agent communication class.

All communication, including voice input, must end in a text-auditable record. Original media should still be retained where practical.

Agent-user communication may use text, images, and audio in a conversation-like manner. Agent-agent communication may use text and multimedia as well, often away from the user-facing channel to avoid noise.

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

## Task Metadata And Planning Constraints

Tasks do not require a mandatory formal task type. If needed, task types may exist as optional labels rather than hard system categories.

Every task must declare at least one work domain when created, for example programming, analysis, graphics, administration, law, security, coordination, and similar categories. A task may have more than one such domain. The requester defines these domains; the agent may only ask for them to be changed.

Expected output is allowed to remain unspecified at creation time, especially for recurring or ongoing tasks.

Every task should have an estimated effort ceiling. This estimate is a maximum authorized work scope rather than a soft forecast. In the default case it should arise from discussion, but the requester may also define it unilaterally. Once set, it is not expected to drift upward on its own. If the assigned agent determines the ceiling is insufficient, it must stop and ask the requester to extend the scope.

Tool usage normally follows the agent's own permissions. A requester may further restrict allowed tools for a specific task, but may not expand them beyond what the agent is already allowed to do.

## Media, Artifacts, And Retention

Media should be retained in original form when possible, not only as text descriptions or transcripts.

To control storage growth, media and other referenced files should ideally be stored once and then referenced by path or identifier from conversations and audit records rather than duplicated repeatedly.

These referenced files do not need to become first-class domain objects with their own full internal biography. It is sufficient that the audit records preserve the reference and the context of use.

Archival and deletion behavior should be conservative by default. A dedicated archivist agent may manage retention, but in the absence of explicit rules the system should avoid deleting anything important.

## Continuation Plan

The next elicitation and design work should focus on:

- exact task state machine and transitions,
- detailed role catalog for top-level and domain agents,
- permission model structure,
- GUI requirements and views,
- storage model for memory, audit, tasks, and persona state,
- execution/tooling substrate for local versus online agents,
- and lifecycle rules for creation, training, promotion, sanction, suspension, and retirement of agents.

## Open Functional Assumptions

The following were treated as current assumptions during elicitation and may still need confirmation during later design:

- the system should default to asking the user rather than guessing when materially uncertain,
- non-blocked work may continue in parallel while waiting for human input,
- more cautious risk judgments override less cautious ones,
- and changing an existing agent's model is technically possible but strongly discouraged.
