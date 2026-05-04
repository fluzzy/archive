# Apple Support App — Leaked CLAUDE.md

> **Source**: [GeekNews (Hada)](https://news.hada.io/topic?id=29093)
> **Fetched**: 2026-05-04
> **Archived**: 2026-05-04

Apple shipped version 5.13 of its Support iOS app with internal `CLAUDE.md` files (intended for AI coding assistants) accidentally bundled. The files were removed in the emergency 5.13.1 update, but their contents had already leaked publicly. This archive preserves the leaked CLAUDE.md content verbatim, providing a rare real-world look at how Apple structures internal AI coding guidance for a production iOS feature.

---

## Background

- The Apple Support app v5.13 shipped with at least two `CLAUDE.md` files inside the bundle.
- Apple released v5.13.1 shortly after to remove the files.
- Mark Gurman (Bloomberg) subsequently reported that Apple heavily relies on Anthropic's Claude for internal development. Anthropic was reportedly asking for "billions annually, doubling each year for three years," prompting Apple to also explore a Google Gemini partnership.
- Community discussion centered on whether AI instruction files belong in version control (consensus: yes, similar to README), and whether Apple's code review caught such leaks.

---

## Leaked CLAUDE.md #1 — Chat Module

```markdown
# Chat - Conversational Support (Juno AI + Live Agents)

- Uses **AsyncStream** for real-time updates, NOT Combine (unlike rest of app).
  Streams are recreated on each access; old ones are finished.
- Service providers are **actors** (not `@MainActor` classes) for thread-safe
  concurrent message handling.
- **Multi-backend via protocol:** `ChatViewModelServiceProvider` abstracts Juno AI
  (`ChatAssistantAPIProvider`), live agents (`ChatKitChatServiceProvider`), and
  dev mocks. View model doesn't know which backend is active.
- Conditional compilation is heavy: `#if JUNO_ENABLED`, `#if
  canImport(CCChatKit)`, `#if DEV_BUILD`. Some files nest these. Check xcconfig for
  enabled flags.
- **Three participant roles:** `.client` (user), `.agent` (live Apple Support),
  `.assistant` (AI). Route message handling per role.
- Messages are wrapped in `MessageGroup` (UUID container) to avoid SwiftUI ID
  collisions (rdar://164022273). Don't flatten.
- CCChatKit is callback-based; bridged to async/await via `Task` wrappers in
  `ChatFacadeServiceProvider`.
- Session persistence: Keychain for `ChatInfo` (reconnection), file cache in
  `CachesDirectory/TemporaryChatTranscripts/` for transcripts.
```

---

## Leaked CLAUDE.md #2 — SAComponents Module

```markdown
# SAComponents - Shared UI Component Library

- Components are purely UI — no business logic, no service dependencies.
- UIKit components use `UIContentConfiguration` protocol with preset factory methods (e.g.,
  `.cell()`, `.callToActionProminent()`).
- SwiftUI components provide convenience modifiers on `View` (e.g., `platterBackground()`,
  `frame(square:)`).
- Presets live in `Presets/` as static factory methods on enums.
- Platform variants use `#if os(visionOS)` guards. iOS version conditionals use `#available`.
- DocC catalog in `SAComponents.docc/` with contributor guide. Update docs when adding components.
- Always include `#Preview {}` showing multiple states for new components.
```

---

## Notable Takeaways

**Architecture signals:**
- Apple internally code-names an AI assistant "Juno AI" routed through `ChatAssistantAPIProvider`.
- The chat backend is abstracted behind a service provider protocol so multiple backends (Juno AI, live agents via CCChatKit, dev mocks) can coexist.
- Heavy use of `#if` conditional compilation flags suggests staged rollout / kill-switching.
- A real radar bug (`rdar://164022273`) is referenced — SwiftUI ID collisions force a `MessageGroup` UUID wrapper.

**CLAUDE.md style:**
- Each module has its own short, focused CLAUDE.md (not one monolithic file).
- Content is dense bullet points covering invariants, conventions, and gotchas — not tutorials.
- Cross-references concrete types, files, and even radar IDs.
- The SAComponents file shows how Apple documents shared-library expectations: UIKit/SwiftUI parity, preset placement, `#Preview` requirement.

---

## References

- [GeekNews topic — Apple Accidentally Shipped Claude.md File](https://news.hada.io/topic?id=29093)
