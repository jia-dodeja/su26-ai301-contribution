# Contribution #1: Extract "Completed" and "Partially Completed" into i18n messages

**Contribution Number:** 1  
**Student:** Jia Dodeja  
**Issue:** https://github.com/openedx/frontend-app-learner-record/issues/457  
**Status:** Phase III Complete

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

**Match:** Other components in this codebase use `defineMessages` from `@edx/frontend-platform/i18n` and `useIntl` to define and format translatable strings. The surrounding components use `FormattedMessage` for JSX rendering, and `intl.formatMessage()` for string assignment outside of JSX.

**Plan:**
1. Import `defineMessages` and `useIntl` from `@edx/frontend-platform/i18n`
2. Define message descriptors for `'Completed'` and `'Partially Completed'` using `defineMessages`
3. Replace the hardcoded strings on lines 30 and 32 with `intl.formatMessage(messages.completed)` and `intl.formatMessage(messages.partiallyCompleted)`
4. Run existing tests with `npm test` to confirm nothing is broken

**Implement:** https://github.com/jia-dodeja/frontend-app-learner-record/tree/fix-issue-457

**Review:** Followed the project's existing i18n patterns and commit message conventions before opening a PR.

**Evaluate:** Ran `npm test` to verify existing tests pass and added a new test to explicitly verify the translated status strings render correctly.

---

## Testing Strategy

### Tests Added

Added one new test case to `src/components/ProgramRecordsList/test/ProgramRecordsList.test.jsx` that explicitly verifies the translated status strings "Completed" and "Partially Completed" are rendered correctly when program data is present.

### Test Results

- 76 tests passing, 2 failing
- The 2 failing tests are in `SendLearnerRecordModal` and are pre-existing failures unrelated to this change (React `defaultProps` deprecation warnings in the Paragon component library)
- `ProgramRecordsList.jsx` coverage: 95% statements, 100% branch

### Manual Validation

Confirmed the hardcoded strings on lines 30 and 32 of `ProgramRecordsList.jsx` have been replaced with `intl.formatMessage()` calls referencing properly defined message descriptors.

---

## Implementation Notes

### Week 3 Progress

**What I built:**
- Updated the import on line 7 of `ProgramRecordsList.jsx` to include `defineMessages` and `useIntl` from `@edx/frontend-platform/i18n`
- Added `const messages = defineMessages(...)` block above the component with descriptors for `completed` and `partiallyCompleted`
- Added `const intl = useIntl()` inside the component function
- Replaced hardcoded `'Completed'` and `'Partially Completed'` strings with `intl.formatMessage()` calls
- Added a new test case verifying the translated strings render correctly

**Challenges faced:**
- `npm ci` failed due to a lock file sync issue; resolved by using `npm install` instead
- Git credential conflict between two Windows accounts (`jiadodeja` vs `jia-dodeja`); resolved by clearing Windows Credential Manager and embedding a Personal Access Token in the remote URL
- Initially considered using `<FormattedMessage>` JSX component, but since the strings are assigned to a variable outside JSX, `intl.formatMessage()` was the correct approach

**Key commits:**
- `feat: extract hardcoded i18n strings in ProgramRecordsList`
- `test: add test for i18n status strings in ProgramRecordsList`

### Code Changes

- **Files modified:** `src/components/ProgramRecordsList/ProgramRecordsList.jsx`, `src/components/ProgramRecordsList/test/ProgramRecordsList.test.jsx`
- **Active branch:** https://github.com/jia-dodeja/frontend-app-learner-record/tree/fix-issue-457

---

## Pull Request

[To be filled in during Phase IV]

---

## Learnings & Reflections

[To be filled in after completion]
