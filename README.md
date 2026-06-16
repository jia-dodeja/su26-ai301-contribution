# Contribution #1: Extract "Completed" and "Partially Completed" into i18n messages

**Contribution Number:** 1  
**Student:** Jia Dodeja  
**Issue:** https://github.com/openedx/frontend-app-learner-record/issues/457  
**Status:** Phase II Complete

---

## Why I Chose This Issue

I chose this issue because it is well-defined and directly tied to a real user experience problem. Non-English speakers using the app still see "Completed" and "Partially Completed" in English even when the rest of the interface is translated, which is the kind of inconsistency that quietly affects usability. The fix involves extracting hardcoded strings into i18n messages in a React component, which aligns with my JavaScript background and gives me a chance to learn how internationalization patterns work in a production codebase.

The maintainer also linked the exact file and line number, which gave me enough context to feel confident this was a contribution I could meaningfully complete.

---

## Understanding the Issue

### Problem Description

In `ProgramRecordsList.jsx`, the strings `'Completed'` and `'Partially Completed'` are hardcoded directly in JavaScript. This means they are always displayed in English regardless of the user's language setting.

### Expected Behavior

The strings should be wrapped in the project's translation function so they can be translated into the user's language.

### Current Behavior

Even when the app is displayed in another language, the program status still shows as "Completed" or "Partially Completed" in English.

### Affected Components

- `src/components/ProgramRecordsList/ProgramRecordsList.jsx` (lines 30 and 32)

---

## Reproduction Process

### Environment Setup

Cloned the fork locally, ran `npm install` (instead of `npm ci` due to a lock file sync error), and started the dev server with `npm start`. The app compiled successfully and ran at `http://localhost:1990/`. The page renders blank locally since it requires a real OpenEdX backend for data, but the hardcoded strings are visible directly in the source code at the referenced file and lines.

### Steps to Reproduce

1. Open `src/components/ProgramRecordsList/ProgramRecordsList.jsx`
2. Navigate to lines 29 to 32
3. Observe that `program.status = 'Completed'` and `program.status = 'Partially Completed'` are hardcoded English strings with no translation wrapper
4. **Expected:** strings should use the project's `intl.formatMessage()` pattern with defined message descriptors
5. **Actual:** strings are hardcoded and will always render in English regardless of user locale

### Reproduction Evidence

- **Working branch:** https://github.com/jia-dodeja/frontend-app-learner-record/tree/fix-issue-457

---

## Solution Approach

### Implementation Plan

**Understand:** The program status strings "Completed" and "Partially Completed" are hardcoded in `ProgramRecordsList.jsx`. They need to be extracted into i18n message descriptors so the app can translate them for non-English users.

**Match:** Other components in this codebase use `defineMessages` from `@edx/frontend-platform/i18n` and `useIntl` to define and format translatable strings. For example, looking at surrounding components, messages are defined at the top of the file using `defineMessages` and then referenced via `intl.formatMessage(messages.someKey)`.

**Plan:**
1. Import `defineMessages` and `useIntl` from `@edx/frontend-platform/i18n` at the top of `ProgramRecordsList.jsx`
2. Define message descriptors for `'Completed'` and `'Partially Completed'` using `defineMessages`
3. Replace the hardcoded strings on lines 30 and 32 with `intl.formatMessage(messages.completed)` and `intl.formatMessage(messages.partiallyCompleted)`
4. Run existing tests with `npm test` to confirm nothing is broken

**Implement:** https://github.com/jia-dodeja/frontend-app-learner-record/tree/fix-issue-457

**Review:** Will follow the project's CONTRIBUTING.md and check commit message conventions before opening a PR.

**Evaluate:** Run `npm test` to verify existing tests pass. Manually verify the strings are no longer hardcoded by inspecting the component output.

---

## Testing Strategy

[To be filled in during Phase III]

---

## Implementation Notes

[To be filled in during Phase III]

---

## Pull Request

[To be filled in during Phase IV]

---

## Learnings & Reflections

[To be filled in after completion]
