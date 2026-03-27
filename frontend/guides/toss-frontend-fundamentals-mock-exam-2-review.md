# Toss Frontend Fundamentals Mock Exam 2nd Review

> **Source**: [정소현 (alice0751)](https://velog.io/@alice0751/%ED%86%A0%EC%8A%A4-Frontend-Fundamentals-%EB%AA%A8%EC%9D%98%EA%B3%A0%EC%82%AC-2%ED%9A%8C%EC%B0%A8-%EB%A6%AC%EB%B7%B0)
> **Author**: 정소현
> **Fetched**: 2026-03-27
> **Archived**: 2026-03-27

A review of the 2nd Toss Frontend Fundamentals mock exam, focusing on code predictability as the core principle of readable code. Covers anti-patterns in naming, state management, and component design.

---

## Core Principle: Code Predictability

> "The brain doesn't process everything — it predicts."

Readable code depends on **predictability**. When readers can anticipate code behavior from naming and structure, comprehension improves significantly. Code should align with reader expectations based on function names, component structure, and data flow patterns.

---

## Anti-Patterns Discussed

### 1. `setMessage` with Props Drilling

- Generic naming like `setMessage` masks the intended context (success/error feedback vs. chat vs. notifications)
- **Solution**: Keep data near its usage point — avoid drilling state setters through multiple layers

### 2. `setDate` in Components

- Exposing parent state management through props breaks component reusability
- **Better approach**: Use standard interfaces (`value` / `onChange`) to hide the caller's context from the component

### 3. Navigation State Passing

- Hiding data in `navigate('/', { state: { message } })` requires manual history cleanup
- **Problem**: Debugging becomes difficult; the receiving side needs non-obvious logic to retrieve and clear the state

---

## Design Principles

| Principle | Guideline |
| --- | --- |
| Component Boundaries | Each component owns specific responsibilities |
| Abstraction Costs | Hiding details should aid prediction, not obscure it |
| State Sourcing | Global state belongs only to truly cross-page concerns (auth, themes, notifications) — not form values or filters |
| Presentational vs. Smart | Choice depends on domain responsibility, not rigid rules |

---

## Refactoring Example

### Before

A 402-line `RoomBookingPage` with intertwined state, URL sync, and rendering logic all in one component.

### After

- Page acts as an **orchestrator** showing flow clearly
- Extracted hooks manage specific concerns
- **URL as single source of truth**: consolidated `useBookingParams` and `useUpdateBookingParam` to eliminate `useState` / `useEffect` synchronization overhead

---

## Key Takeaways

- Develop the habit of asking: *"Will this code's behavior surprise another developer?"*
- Small naming or placement choices either smooth or interrupt reading flow
- You need **justification for your decisions**, not absolute truth — claims matter, don't blindly follow answers

---

## References

- [토스 Frontend Fundamentals 모의고사 2회차 리뷰 — 정소현](https://velog.io/@alice0751/%ED%86%A0%EC%8A%A4-Frontend-Fundamentals-%EB%AA%A8%EC%9D%98%EA%B3%A0%EC%82%AC-2%ED%9A%8C%EC%B0%A8-%EB%A6%AC%EB%B7%B0)
