---
name: agent-card-ui
description: Design and build polished conversational interfaces on Wonderful, with short agent replies and branded interactive cards inline in one thread. Use for Chat SDK embeds, reusable Widget Library experiences, and sample or live Wonderful Apps across booking, commerce, finance, onboarding, events, support, and other domains. Covers focused custom components, optional frosted-glass surfaces, safe actions, theme support, and real-platform verification.
---

# Agent + cards on Wonderful

**One conversation, one focused artifact, one clear next action.** The agent guides the user with a short reply. The card makes the choice or result easier to understand. Keep simple answers as text.

## 1. Choose the experience before implementation

1. Identify the audience, channel, task, and final action.
2. Decide whether the request needs a visual draft, a scripted sample, or a live agent integration.
3. Resolve the requested tenant and workspace before creating resources.
4. Inspect the exact existing widget when the user asks to reuse or update it.
5. Preserve its identifier, parameter contract, and actions unless the request requires a change.

**Default to cards inside the chat thread.** Place each card immediately after its associated assistant reply, above the next turn and composer. Do not create a separate card panel, dashboard, or two-lane canvas unless the user explicitly requests it.

For sample conversations, state that replies are scripted and actions are local. Do not imply a live agent or completed external transaction. Sample labels are a copy choice, not a substitute for safe behavior. If the user removes them, preserve the no-write boundary and truthful outcome copy. Keep stable internal identifiers separate from visible labels.

## 2. Ground the delivery shape in current docs

Fetch the relevant live pages before relying on platform contracts:

```bash
wful docs page build/widgets/overview
wful docs page build/widgets/local-development
wful docs page build/tools/low-code
wful docs page developer_platform/chat-sdk/widget-reference
```

Fetch `developer_platform/chat-sdk/browser-voice` when voice is requested. Read the authentication documentation before integrating personal data. Cite documentation paths and fetch dates for platform claims. Do not mirror documentation or a component catalog into a repository.

| Deliverable | Native path |
|---|---|
| Inline cards in a website conversation | Chat SDK conversation surface and Widgets |
| Reusable choice, review, form, or compact result | Saved Widget Library widget |
| Widget supplied by an agent tool | `context.widgets.respondWith({ slug, params, summary, skipAgentReply })` |
| Branded artifact the catalog cannot express | One focused Custom Chat component |
| Persistent Wonderful App | `wonderful-apps` and the connected Software Studio workflow |
| Inline explanation in the assistant's current chat | GenUI; not a saved Widget Library resource |

`renderConversation(chat, host)` mounts the conversation surface into the host. Use the normal conversation renderer for inline message cards. Do not mount cards separately with `renderWidget()` for this default layout.

Native WhatsApp does not become a Custom Chat surface through an embed. Keep WhatsApp messages, buttons, and lists native. Propose a linked web conversation for rich cards when appropriate. Do not silently add that companion experience to a WhatsApp-only requirement.

Browser voice is distinct from WhatsApp calls and voice notes. Chat and voice are separate conversation choices, not an assumed mid-conversation switch. Do not promise interactive card parity during voice. Verify the supported channel and client. Direct widget-to-tool actions are unavailable in voice under the documented contract.

## 3. Design one reusable widget per task

Start with built-ins for layout, text, forms, navigation, and familiar controls. Create a custom component only when a focused branded artifact or interaction cannot fit those contracts. Do not create generic custom text, spacing, or button libraries.

| Task | Useful artifact | Final action |
|---|---|---|
| Review a request | Compact summary with labelled values | Confirm the reviewed details |
| Choose a product or service | Option cards with meaningful differences | Select one option |
| Book or attend | Reservation, ticket, or appointment card | Confirm or continue to a secure service |
| Understand a financial result | Labelled figures and a concise explanation | View details or choose the next step |
| Follow a delivery or case | Status and a short timeline | Track, respond, or contact support |
| Provide several details | Short local form steps and a review screen | Submit once |

Say when the agent should use the widget in its description and relevant skill instructions. Prefer saved widgets for repeated transactional flows. Use runtime composition only when content genuinely varies.

Discover the current component contracts through native authoring tools, or the pinned Custom Chat project:

```bash
pnpm components
pnpm component -- <component-name>
pnpm check
```

Do not guess names, properties, events, or renderer types. Use a flat component pool with child IDs. Define parameter types, realistic preview values, and useful fallback text. Bind server parameters with `$param`; use JSON-pointer bindings for reversible client-local state.

## 4. Set a deliberate visual direction

Write five terms before authoring: **palette, surface, typography, density, memorable motif**. Choose them from the audience, brand, and brief. Do not default to an unrelated purple gradient, or copy one customer's brand into another domain.

Design the card at **360px** first. Verify expansion to **640px** and the desktop conversation. Give the screen one dominant artifact, not a stack of nested generic cards.

Use this hierarchy:

1. Small context or status label.
2. Clear heading, usually 18–24px at chat width.
3. Grouped, readable values or choices.
4. Necessary explanation, source, or safety note.
5. One primary action, with quieter secondary actions only when needed.

Use 14px body text and readable line spacing. Keep secondary text at least 11px. Align labels and values. Allow long values to wrap. Use one consistent radius scale, restrained borders, and intentional spacing. Place the decision and primary action within the card's first practical viewport.

Keep agent replies to one or two useful sentences. Avoid repeating every card value in prose. Make action labels and customer transcript messages natural and specific. Do not expose tool names or structured action context to the user.

### Optional frosted-glass treatment

Use glass when the user requests it or the brand supports it. It is not a universal default.

- Use one contained colour wash with one or two brand accents.
- Place soft decorative shapes **behind** the panel so blur has something to affect.
- Start with a 24px panel radius, 1px translucent inner stroke, and approximately 18px backdrop blur.
- Use a restrained tinted shadow. Avoid neon glows and constant motion.
- Keep field groups quiet. Do not box each value in another decorative card.
- Give the primary action a readable, high-contrast fill. Use status colours for their actual meaning.
- Provide an opaque surface when blur is unsupported or reduced transparency is requested.
- Check text and controls against the rendered backdrop in both themes. Transparency alone is not accessibility.

The surrounding chat remains quieter than the card. Do not apply glass and gradients to every bubble, input, or app section.

## 5. Implement custom components at the correct boundary

Use the current Custom Chat authoring and design skills. A renderer exports synchronous `register(registry)`, registers its exact component name, and returns an instance with `unmount()` and optional `update(nextContext)`.

- Render parameter strings with `textContent` or safe framework text nodes, never untrusted `innerHTML`.
- Insert styles into `host.getRootNode()` when it is a ShadowRoot; otherwise use the owning document's head.
- Prefix selectors and style IDs to avoid collisions. Remove listeners and owned nodes during unmount.
- Read and normalize `action.event` before calling `context.dispatch`. Dispatch the event, not its `{ event: ... }` wrapper.
- Preserve required action context, parameter bindings, and customer presentation during visual edits.
- Give controls keyboard behavior and a visible focus state. Mark decorative elements as non-interactive.
- Do not swallow dispatch errors or present a click as a confirmed business outcome.

Use SDK tokens for neutral surfaces, text, borders, focus, and fonts: `--chat-card`, `--chat-card-secondary`, `--chat-border`, `--chat-radius`, `--foreground`, `--fg-muted`, `--primary`, `--primary-foreground`, and `--font-sans`. Check the current token contract before using additional names. Brand accents may have explicit light and dark values in the focused renderer.

Brand the Chat SDK frame through `appearance`; page CSS cannot style its shadow-root internals. Keep frame presentation separate from component surfaces.

### Theme changes must work in the actual host

Test a real light-to-dark-to-light switch, not just OS emulation. SDK context can retain its initial theme while platform tokens change.

For a Wonderful App, use the shadow host's live theme attribute for mode-specific widget styles:

```css
:host([data-theme-mode="dark"]) .your-widget-panel {
  /* Explicit dark treatment for this branded surface. */
}
```

That selector belongs to the App's shadow-root stylesheet. It is not a universal selector for Widget Library iframe previews. Use the documented theme contract for each renderer host. Prefer inherited tokens for neutral colours. Never use an OS media query as the sole theme source for a platform-pinned appearance.

## 6. Keep actions and outcomes truthful

Use local actions for selections, navigation, and edits. Submit once at the final commitment. Include every reviewed value the receiving agent or tool needs. Use valid lowercase snake_case action-context keys and the installed schema's exact event shape.

Configure `presentation.text` as natural customer speech. Template variables must name keys in that action's context. Hidden presentation changes the transcript only; it does not make data secret. Never submit credentials or unauthorized internal state.

A click, dispatched event, accepted request, approved request, and completed transaction are different states. Use the documented result lifecycle before showing external success. Do not assume `dispatch()` returns an acknowledgement. Handle expected failures visibly. Prevent duplicate business writes through server authorization and idempotency, not merely a disabled button.

Use direct tool actions only when the documented client supports them and the tool is eligible. Keep payments and sensitive documents behind appropriate secure services. Project only data the customer may see into the widget.

An agent tool can send one saved widget using `context.widgets.respondWith()`. Put display data in `params`, compact agent context in `summary`, and use `skipAgentReply: true` only when the widget and fallback complete the answer. Attach the widget, enable the agent's widget policy, and declare the tool through its skill. Follow the version-specific agent workflow; do not replace prompts wholesale for a small UI change.

## 7. Reuse the real widget in sample apps

Use `wonderful-apps` and `wonderful-software-studio` for persistent App deliverables. Connect the target and resolve editable source separately. Deploy to Development by default; Production requires explicit approval and its release gates.

For a scripted sample:

1. Capture the exact saved widget document and custom renderer at a known source commit.
2. Reuse that snapshot. Do not redraw a lookalike card or regenerate its values on every render.
3. Record any small portability edits, such as removal of unused type imports or host-specific theme selectors.
4. Resolve the existing parameter bindings and route its existing action to a clearly local sample callback.
5. Keep the card inside the associated assistant turn. Append the customer confirmation and next assistant reply to that same thread.
6. Make restart create fresh widget instances. Keep each instance's confirmation independent.

A frozen snapshot does not publish Widget Library changes, invoke a live agent, or receive later widget updates automatically. Say so. For live integrations, use the approved Chat SDK renderer, authentication, resource attachments, and published releases instead of a sample adapter.

Build App chrome with `@wonderful/ui-base`. Use `Stack` for columns and explicit `Box direction` for rows. Read exact component props before implementation. Some components accept `className`; others do not. Scope App styles to `.app-root` and use platform tokens. A DOM host for an existing external widget renderer is a boundary, not permission to recreate ordinary controls.

Keep the composer visible at realistic desktop and phone heights. Let the transcript scroll rather than forcing the whole page below the fold. Do not verify only in an unusually tall browser window.

## 8. Verify the saved and deployed result

Validation is necessary but does not prove appearance or behavior.

- Run the pinned widget/component validator and the App's required checks.
- Inspect the actual saved Widget Library preview; verify the actual name, description, parameters, fallback, and actions.
- Check 360px and 640px card widths, a normal desktop viewport, and a phone viewport in light and dark themes.
- Inspect long values, focus, disabled controls, empty input, and applicable loading, error, and opaque-fallback states.
- Exercise the complete flow: user message → assistant reply → inline card → user action → customer transcript line → truthful outcome.
- Confirm the composer remains usable and no horizontal overflow hides content.
- For a live agent, run the smallest scenario that proves widget selection, expected values, submission context, and follow-up behavior. Test incompatible-client fallback.
- For an App, verify the mapped platform URL and exact active Development version. Inspect and retain screenshot evidence.

A copied HTML harness, local runner, DOM assertion, or successful build is not proof of the saved platform renderer. Local previews help iteration; the final visual judgment comes from the real surface. Do not spend time repairing a duplicate browser harness when the actual platform preview is available.

Keep widget saving, Widget Library publication, agent publication, App deployment, and Production promotion distinct. Save drafts by default. Start a new conversation after publication when capability negotiation requires it. Do not publish unrelated workspace changes or alter existing agents to make a sample work.

Record outcome, duration, error category, and a correlation ID where available. Never record widget parameters, customer text, credentials, or submission context as analytics properties.

## Sources and scope

The live documentation remains authoritative. Guidance was grounded in the pages above on 6 October 2026 and a saved-widget/Development-App implementation with actual desktop and mobile proof. The durable lessons are domain-neutral: inline composition, one branded centerpiece, truthful actions, frozen-source reuse, host-aware themes, and real-platform verification. Do not copy tenant names, resource IDs, sample data, or domain-specific policy into future work.
