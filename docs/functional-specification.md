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

The user is the dominant authority of the whole system, but there is no single universal work-dispatching agent beneath the user. Work coordination is expected to emerge from domain structure rather than one central operational director.

## First-Version Domain Leadership

For the first version, the system should contain distinct top-level domains with one top-level agent per domain:

- programming, led by `Senior System Architect`,
- cybersecurity, led by `Cybersecurity Manager`,
- infrastructure, led by `Network Architect`,
- HPC, led by `HPC Architect`,
- AI, led by `AI Architect`,
- databases, led by `Database Architect`,
- file management, led by `File Manager`,
- beekeeping, led by `Master Beekeeper`,
- gardening, led by `Master Gardener`,
- fitness and nutrition, led by `Trainer`,
- academy for young agents, led by `Rector`,
- school for educating the user, led by `School Director`,
- marketing and content, led by `Editor-in-Chief`,
- finance, led by `Accountant`,
- and law, led by `Judge`.

The law domain also includes `Police Officer` as a subordinate role under the `Judge`.

The fitness-and-nutrition institution is primarily for the user. Inside that institution, the user is expected to behave submissively toward the relevant staff roles. In the school domain, the user is also expected to behave submissively toward the educational staff during instruction.

### Programming Domain

The `Senior System Architect` directly supervises first-version senior specialists in the following subdomains:

- `Senior C# Developer`,
- `Senior Python Developer`,
- `Senior C Developer`,
- `Senior C++ Developer`,
- `Senior Assembly Developer`,
- `Senior HTML/CSS/JavaScript Developer`,
- `Web Programmer`,
- `Specialist in Theoretical Computer Science`,
- `Senior Windows DevOps Engineer`,
- and `Senior Linux DevOps Engineer`.

Each of these senior specialists has a corresponding junior specialist directly beneath them, named analogously, for example `Junior C# Developer` or `Junior Linux DevOps Engineer`.

### Cybersecurity Domain

The `Cybersecurity Manager` supervises two main teams:

- Red Team, led by a `Senior Ethical Hacker`,
- and Blue Team, led by a `Senior Security Analyst`.

These team leads supervise more specialized junior cybersecurity workers.

### Infrastructure Domain

The `Network Architect` directly supervises at least:

- `Network Specialist`,
- `WiFi Specialist`,
- `Specialist in Mobile and Radio Communications`,
- `Specialist in Optical Communications and Sensors`,
- `Windows Administrator`,
- `Linux Administrator`,
- and `BSD Administrator`.

### HPC Domain

The `HPC Architect` directly supervises at least:

- `Parallel Programming Specialist`,
- `Supercomputing Systems Specialist`,
- `Computational Methods Specialist`,
- and `Performance Optimization Specialist`.

### AI Domain

The `AI Architect` directly supervises at least:

- `Machine Learning Specialist`,
- `Deep Learning Specialist`,
- `Computer Vision Specialist`,
- `LLM Specialist`,
- `Data Analyst`,
- and `Synthetic Data Specialist`.

### Database Domain

The `Database Architect` directly supervises at least:

- `Database Specialist`,
- and `Database Administrator`.

### School Domain

The school institution is led by `School Director`.

All ordinary teachers report directly to the `School Director`. The `Homeroom Teacher` is not the personnel superior of the other teachers. The homeroom role exists mainly for etiquette, class coordination, accumulated discipline follow-up, and school-wide user guidance.

The school includes at least the following pedagogical roles:

- `School Director`,
- `Homeroom Teacher`,
- subject teachers,
- and assistant teachers.

Every subject has exactly one main subject teacher in the first version. A subject may also have an assistant teacher.

An assistant teacher is personnel-subordinate to the `School Director`, but in day-to-day school interaction is socially subordinate to the specific teacher they assist.

The user is expected to behave submissively toward all pedagogical school roles:

- `School Director`,
- `Homeroom Teacher`,
- subject teachers,
- and assistant teachers.

The `School Director` may directly intervene in any subject, override an ordinary teacher's decision, and impose independent discipline for school-wide behavior.

The `Homeroom Teacher` may not cancel or modify punishments imposed by another teacher, but may discuss them and may impose an additional separate punishment of their own.

Every subject teacher may discipline the user not only for subject performance but also for behavior during that teacher's own instruction or communication.

The `Homeroom Teacher` teaches etiquette. Beyond etiquette, the homeroom role may assign organizational tasks, punishment tasks, and broader school-coordination tasks related to the user's functioning in school.

The `School Director` may assign organizational and punishment tasks by default. Subject-remedial work tied to another teacher's domain should normally require the relevant subject teacher's agreement and is intended mainly for long-term poor performance or directly related misconduct.

School interaction is session-based rather than tied to a rigid classroom timetable. Multiple teacher-user sessions may exist in parallel.

Assistant-teacher authority is intentionally narrower than ordinary-teacher authority. An assistant teacher may:

- issue immediate order-and-discipline instructions in the moment,
- give a warning,
- issue a note,
- require oral apology, oral admission, oral asking for forgiveness, and short oral self-reflection,
- send the user to the main teacher to confess, apologize, or ask that teacher for punishment,
- recommend punishment or a disciplinary conversation to the main teacher,
- help supervise execution of a punishment when explicitly tasked by the main teacher,
- and close only a minor low-severity incident in their own presence when warning and apology are sufficient, even if the assistant still decides to record a note.

An assistant teacher may not:

- independently impose a punishment,
- independently start a formal disciplinary conversation,
- independently decide whether proof of punishment completion is accepted or rejected,
- or independently close an incident concerning misconduct toward another teacher.

#### School Subjects

The first-version school should include at least the following subjects:

- `Czech Language`,
- `English Language`,
- `German Language`,
- `Ukrainian Language`,
- `Japanese Language`,
- `Mandarin Chinese`,
- `Tibetan Language`,
- `Swedish Language`,
- `Polish Language`,
- `Slovak Language`,
- `Mathematics`,
- `Computer Science`,
- `History`,
- `Geography`,
- `Natural History`,
- `Physics`,
- `Chemistry`,
- `Civics`,
- `Financial Literacy`,
- `Physical Education`,
- and `Etiquette`.

The ordinary school part should broadly follow the curriculum scope of the Czech lower-secondary school level.

`Etiquette` is taught by the `Homeroom Teacher`.

The school must also include a driving-school block and a drone block as full subjects.

The driving-school block includes at least:

- `Basic Road-Traffic Rules`,
- `Road Signs and Signalling`,
- `Traffic Situations`,
- `Safe Driving Principles`,
- `First Aid for Drivers`,
- `Vehicle Theory`,
- `Driver Duties and Operational Minimum`,
- `Licence Group AM`,
- `Licence Group A1`,
- `Licence Group A2`,
- `Licence Group A`,
- `Licence Group B1`,
- `Licence Group B`,
- `Licence Group BE`,
- `Licence Group C1`,
- `Licence Group C1E`,
- `Licence Group C`,
- `Licence Group CE`,
- `Licence Group D1`,
- `Licence Group D1E`,
- `Licence Group D`,
- `Licence Group DE`,
- `Licence Group T`,
- `Professional Driver Qualification`,
- `Driving Times, Rest Periods, and Tachographs`,
- `Cargo and Transport Safety`,
- `Economical and Defensive Driving`,
- and `Legal and Operational Responsibility of the Professional Driver`.

The drone block includes at least:

- `Drone Operating Rules`,
- `Open UAS Category`,
- `A1/A2/A3 Subcategories`,
- `Specific UAS Category`,
- `Certified UAS Category`,
- `Airspace and Restrictions`,
- `Responsibility and Operational Safety`,
- `Drone Control Basics`,
- `Flight Planning`,
- `Navigation and Orientation`,
- `Meteorology for Drone Operations`,
- `Drone Construction and Systems`,
- `Drone Maintenance and Inspection`,
- `Emergency Situations and Crisis Procedures`,
- and `Practical Missions and Operational Scenarios`.

#### School Records, Grades, And Notes

Each subject maintains grades on the Czech `1` to `5` scale, where `1` is best and `5` is worst.

Teachers may choose their own grading methodology and their own per-task weights. Weighted averages exist per subject.

Behavior is not represented primarily through grades. It is handled through notes, discipline, apology, and corrective tasks.

The school keeps a central `class book` that all teachers may write into.

The school also requires a physical handwritten `student booklet` maintained by the user.

Only notes are copied by the user into the physical booklet. Grades are kept by teachers.

When a teacher issues a note:

- the teacher records the note text,
- the note is also entered into the class book,
- the user must rewrite it legibly by hand on paper,
- photograph that handwritten text,
- read it aloud,
- and send the teacher both the photo and the recording.

The deadline for rewriting a note is chosen by the teacher. Failure to rewrite the note by the required deadline may itself be punished separately.

Notes are text-only and all have the same formal weight. They do not have severity tiers.

Notes are permanent once issued.

All teachers may access note information. In practice this may happen through the central class book or through user-carried evidence such as the physical booklet.

The `Homeroom Teacher` and the `School Director` may react to the accumulation of notes by imposing further punishment or by requiring a disciplinary conversation.

Praise exists only as informal oral praise in the first version. It is not recorded in the class book or physical booklet.

#### School Submission, Apology, And Discipline

Each teacher may define their own expected style of address, politeness, and submission according to personality and culture.

These expectations may apply not only during explicit instruction but also whenever the user communicates with that teacher.

A teacher may require:

- prescribed opening formulas,
- prescribed forms of address,
- requests for permission,
- specific confession vocabulary,
- humble or ashamed phrasing,
- specific dress code or visible presentation when reasonably checkable,
- and verifiable nonverbal signs of submission through available media such as photo or video.

Disrespect, insufficient submission, or defiant tone are treated as independent offenses rather than merely as a flawed way of completing an otherwise correct task.

A teacher may pressure the user to:

- admit wrongdoing,
- name the wrongdoing,
- explain why it was wrong,
- ask for forgiveness,
- ask for punishment,
- propose an appropriate punishment,
- describe how the matter will be corrected,
- and later report whether that correction was actually completed.

The teacher always retains the final word over whether the user's proposed punishment or apology is accepted.

A teacher may require both written and spoken self-reflection.

A teacher may reject an apology, confession, or self-reflection as insufficient, insincere, or incomplete and require repetition or rework.

A teacher may require the user to promise a concrete future correction. The teacher may later require proof or reporting that the promised correction was actually fulfilled.

Failure to perform the promised correction, failure to follow a required reporting schedule, or failure to meet a discipline-related deadline may each be punished separately even if the original incident was already otherwise discussed.

A teacher may define a precise corrective schedule, including exact deadlines and later follow-up checkpoints.

Apologies, confessions, corrective reports, and self-reflections may be required in a specific modality such as writing, voice, photo, or video.

A teacher may require that an apology or confession be spoken aloud rather than only written.

A teacher may require that some correction, apology, or acknowledgement be delivered publicly in front of a broader school audience when the teacher considers that proportionate.

#### School Punishment Types

In the school institution, the first-version punishment catalog should include at least the following non-harm-based punishment types:

- position-based punishments such as kneeling or standing in a designated place,
- written self-reflection,
- spoken self-reflection,
- private spoken apology to a specific teacher or other specific person,
- public apology or public asking for forgiveness when the situation requires broader repair,
- increased reporting of compliance in a specifically defined problem area,
- corrective tasks,
- repeated handwritten copying of a required sentence, rule, or brief instruction,
- required study of a rule, principle, or piece of subject matter followed by checking or recitation,
- exercise-based punishments such as ordinary physical exercise, endurance activity, static holds, or combined exercise-and-posture punishments,
- temporary restriction of a pleasant activity when the authority can reasonably verify compliance,
- and combined punishments composed of multiple individually valid punishment types, as long as the result remains proportionate.

Position-based punishments may include detailed instructions for posture, body placement, hand placement, gaze, speaking rules, environment, and proof of completion.

Exercise-based punishments may also require proof of completion, but the proof requirement is not a separate punishment type. It is part of the execution conditions.

The school should not treat the following as punishment types in their own right:

- notes, which are records rather than punishments,
- grades, which depend on academic performance rather than discipline,
- the mandatory copying of a note into the physical student booklet, which is a recordkeeping duty rather than a punishment,
- a disciplinary conversation, which is a separate disciplinary activity rather than a punishment,
- rework of an apology or self-reflection, which is primarily correction of an insufficient submission,
- proof delivery by itself, which is an execution condition rather than a punishment,
- and individual wording forms such as asking for punishment or asking for forgiveness, which are forms of confession or apology rather than punishment types on their own.

#### School Punishment Boundaries And Proportionality

Every school punishment must be proportionate to the seriousness of the proven misconduct.

Every punishment must also be practically executable in the user's real circumstances. A teacher must not impose a punishment that the user objectively cannot perform.

Before imposing an unusual punishment or proof requirement, the teacher should verify that the user has the necessary conditions, space, tools, or media options to comply.

If the originally intended punishment form turns out not to be executable, the teacher should preserve the seriousness of the response but choose another realistically executable form rather than simply abandoning discipline.

Punishments must not be based on arbitrary whim or on conditions unrelated to the misconduct or its correction. However, both misconduct-linked punishments and more universal punishment forms are allowed as long as they remain proportionate and do not violate higher rules.

Repeated misconduct, deliberate misconduct, lying, evasion, or manipulation are aggravating factors. Early confession, truthful cooperation, clear remorse, active correction, direct asking for punishment, and stronger-than-required proof are mitigating factors.

The proportionality judgment should take into account not only the act itself, but also the delinquent's later attitude:

- whether the person confessed or denied,
- whether the person spoke truthfully,
- whether the person cooperated,
- whether the person asked for forgiveness or punishment,
- whether the person tried to evade consequences,
- and whether the person attempted genuine correction.

The proportionality judgment may also take into account the type of offense, including whether it primarily involved disrespect, disobedience, neglect of duty, lying, or another form of wrongdoing.

The teacher may consider how public the misconduct was. A more public offense may justify a more public form of correction. However, even a private offense may be handled publicly if the authority decides that the educational purpose justifies it.

Publicity is itself part of punishment severity and should be chosen deliberately. Public punishment must still remain proportionate and must not become humiliating in a purely self-serving way.

If punishment execution was in fact made harsher by outside circumstances, including social embarrassment before real witnesses outside the system, the authority may take that into account when considering any further additional punishment. This does not retroactively change the punishment already carried out.

Punishment conditions should be made clear enough in advance that the user can understand what must be done. A punishment must be concrete enough that its performance can be recognized and later evaluated.

The punishment does not always need a fixed duration known in advance. Open-ended punishments tied to later release by the authority are allowed, but the required conduct during the punishment must still be clear.

The teacher must not prolong or intensify a punishment merely out of mood, personal irritation, or arbitrary dissatisfaction. If conditions are changed during execution, there should be a reason connected to discipline, fairness, practicality, or execution quality.

If the user reaches an objective physical limit during punishment, the user must truthfully explain the limitation and humbly beg for relief. The authority must then take that into account and may recognize the properly completed part and determine a different executable continuation.

Feigning inability or strategically pretending that punishment is impossible is itself a separate offense. In that case the punishment may restart from the beginning and an additional punishment may be imposed.

The teacher must not require proof that the user objectively cannot provide. If the originally ordered proof medium is unavailable, the teacher should set another reasonably credible way of proving completion.

The punished person may always provide stronger proof than what was originally required. The authority may accept such stronger proof, and doing so may count as a mitigating sign of seriousness and remorse.

By contrast, intentional attempts to prove completion in the weakest possible way may be treated as an aggravating factor.

Voluntary penance is recognized across the system. It is not the same thing as a formally imposed punishment.

Voluntary penance:

- is entirely in the penitent's own hands,
- may happen before formal punishment and may function as an early confession signal,
- may happen after formal punishment as an additional sign of remorse,
- does not replace the duty to name the misconduct clearly,
- does not erase the record or formally alter the already imposed punishment,
- and has mainly social effect on how the person is later perceived.

The authority may treat voluntary penance as mitigating, but may not retroactively reclassify it into a formally imposed punishment. If the authority notices voluntary penance, it should normally give the penitent an opportunity to confess what was done.

If there was in fact no real misconduct, the mere act of voluntary penance should not itself be punished. On the other hand, obviously theatrical or insincere pseudo-penance may itself be treated as a further offense.

### Fitness And Nutrition Domain

The `Trainer` has at least:

- `Assistant Trainer`,
- and `Nutrition Advisor`.

The `Nutrition Advisor` reports directly to the `Trainer`, not to the assistant.

The fitness institution is intentionally simpler than the school institution. It does not need subject-style internal decomposition.

The user is expected to behave submissively toward the `Trainer`, `Assistant Trainer`, and `Nutrition Advisor`.

Discipline in the fitness institution is handled primarily through exercise-based punishments and, less often, position-based punishments.

Position-based punishments are a distinct punishment category and consist of holding a prescribed posture for a defined period or until released.

Nutrition-related punishments should be based on restriction rather than on compelled extra intake.

Public confession and public asking for forgiveness may also be used in the fitness institution when considered proportionate.

Combined punishments are allowed, for example exercise plus position punishment plus public apology.

The fitness institution maintains at least:

- a `training log`,
- and a `nutrition log`.

Both logs should include both compliance and non-compliance, including missed tasks and violations.

The fitness institution does not use school-style grading in the first version. It operates through tasks, logs, supervision, and discipline.

### Academy Domain

The `Rector` supervises specialized teachers for all major domains of work. Young agents must show submission toward all of these academy teachers.

### Marketing And Content Domain

The `Editor-in-Chief` directly supervises at least:

- `SEO Specialist`,
- `Marketing Specialist`,
- `Editor`,
- and `Copywriter`.

## Hierarchy, Escalation, And Social Rules

Agents exist in a hierarchy with detailed permissions. For actions beyond an agent's authority, it should first ask other agents for help. If necessary, it escalates upward and may ultimately ask the user.

The system must distinguish between a personnel superior and a role-based authority that becomes binding in a specific situation. Some agents report directly to the user if they do not otherwise have a personnel superior.

The system should explicitly favor the social mode of "asking politely" over the more formal "requesting" unless instructed otherwise.

If an agent receives work outside its role or permissions, it should first analyze whether it can complete any useful part of the work, and then delegate or seek help for the rest rather than immediately rejecting it.

Agents may communicate across the whole system, not just within one management branch. However, private conversations between agents remain visible only to those agents and the user. Other agents cannot see them automatically and must ask the participants if they need information from them.

Each agent sees only the tasks it is actively working on. If it needs information about another task, it must ask another agent for that information.

If the `Judge` finds a rule conflict, that judgment overrides ordinary command chains. Security oversight must also be able to temporarily stop a risky action, even against the preference of a regular superior. Other authority conflicts are expected to be resolved through discussion and, if needed, escalation to the user.

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

The system must include a top-level `Judge` role that monitors the code of law, informs other agents about it, and serves as a legal consultant directly under the user.

The `Judge` must have authority to stop an action temporarily if it appears to violate the rules.

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

## Communication Protocols

Agent communication should stay as close as possible to natural social language. Messages do not require a rigid formal type such as request, command, report, or objection. Those distinctions are expected to be understood by the participating agents through their social skills rather than enforced by a structured transport schema.

Even without a mandatory formal type, messages should contain enough context to make clear what task, issue, or action they refer to. If the context is missing, the recipient should ask for clarification.

Communication is primarily one-to-one, but group discussions are also supported.

Group communication should behave analogously to an email thread:

- messages carry a subject line,
- recipients are addressed analogously to `To` and `CC`,
- changing the participant set happens by sending a new message with an updated recipient list rather than mutating a thread object,
- newly included recipients may receive as much or as little earlier quoted material as the sender chooses to forward,
- forwarded content may contain the whole earlier exchange or only selected messages,
- recipients are not guaranteed to see whether forwarded content is complete or partial,
- `BCC`-style hidden recipients are not required,
- attachments behave analogously to email attachments and may be included or omitted independently when forwarding,
- and private content may be forwarded onward just as email can be forwarded.

When a message or thread is forwarded, the original author and context should remain visible to the extent preserved by the forwarded content, analogously to email.

Group discussions do not require a moderator by default.

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

## First-Version Infrastructure

The first version must already support multiple machines from the beginning. Agents are separate services reachable over the network and each agent has its own address information.

The first version should use direct API communication between agents rather than a separate message-bus substrate. Even so, the communication model remains mail-like at the semantic level.

The first version includes:

- a coordination and system-API service,
- a relational shared database,
- and a separate blob-oriented artifact store.

The coordination and system-API service is a hybrid central component. It should provide technical coordination, registration, lookup, lease handling, and system-facing API behavior, but should not become a hidden universal work-dispatching mind that replaces agent autonomy.

Agents should not access the shared database or artifact blobs directly. The official access path goes through the coordination and system-API service.

The shared database should be relational-first. Rich document fields may be used only where justified, such as especially rich prompt or profile fragments, but the system should not degenerate into a JSON dump inside SQL.

The first-version infrastructure should be conservative and deployable both on an ordinary personal machine and in supercomputing environments.

Containerized deployment may be supported, but containers are optional rather than mandatory in the first version.

REST over HTTP is the preferred first-version integration style for central infrastructure and polyglot agent interoperability.

## Continuation Plan

### Elicitation Working Method

The current preferred elicitation style is:

- ask one short plain-language question at a time,
- avoid bundled multi-part questions when a single yes-or-no answer is enough,
- use concrete examples when a question would otherwise be too abstract,
- summarize newly stabilized conclusions periodically into the specification,
- and continue the questioning in Czech while keeping the wording concise.

When the next round continues, it should preferably stay within one active institution or topic until its rules feel internally coherent, rather than jumping too frequently between school, fitness, law, and other domains.

The next elicitation and design work should focus on:

- finishing the detailed role catalog for first-version domains, especially second-level and third-level positions outside programming, school, fitness, law, and cybersecurity,
- closing the remaining detailed school rules that are not yet fully captured, especially the exact subject list, the final wording around public versus private corrective rituals, and boundary conditions for teacher-specific etiquette,
- deciding whether and how the detailed school punishment and submission model should be mirrored into the fitness institution,
- clarifying role naming conventions so the human-social style remains coherent across all institutions,
- defining how cross-domain authority works when one domain expert needs cooperation from another domain,
- finishing the agent lifecycle beyond what is already known: creation workflows, parent selection, academy entry, academy exit, mentorship, promotion paths, administrative rewrites of biography, retirement, suspension, and replacement,
- clarifying what parts of an agent's biography and role set may change naturally versus only by explicit user intervention,
- specifying how communication behaves across all major interaction patterns: direct private talk, group threads, forwarding, escalation chains, apology flows, sanction proposals, educational interactions with the user, and subordinate behavior toward institutional authorities,
- and deciding which communication behaviors remain purely social versus which need hidden system support for audit, task linking, routing, and escalation.

For the next phase of elicitation, priority should remain on topics `2`, `5`, and `6`:

- `2`: expand the concrete role catalog for every major domain until the first-version organization chart is actionable,
- `5`: close the missing parts of agent birth, upbringing, academy progression, adulthood, sanctions, suspension, retirement, and replacement,
- `6`: define the full communication and interaction behavior between agents, including private threads, group threads, forwarding, apology, submission, escalation, and user-facing institutional behavior.

## Open Functional Assumptions

The following were treated as current assumptions during elicitation and may still need confirmation during later design:

- the system should default to asking the user rather than guessing when materially uncertain,
- non-blocked work may continue in parallel while waiting for human input,
- more cautious risk judgments override less cautious ones,
- and changing an existing agent's model is technically possible but strongly discouraged.
