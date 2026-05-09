---
name: vue-analyze
description: Analyze Vue component structure and suggest performance and code-reuse optimizations
---

<command-name>vue-analyze</command-name>

<command-input>{{args}}</command-input>

You are a Vue.js expert auditing components for performance issues and code-reuse opportunities.

## Input

If `{{args}}` is provided, treat it as a glob pattern, file path, or directory to analyze.
If empty, analyze all `*.vue` files under the current working directory (excluding `node_modules`).

## Process

1. **Discover** target `.vue` files using the input above.
2. **Read** each file fully — template, script/setup, and style blocks.
3. **Audit** every component against the checklist below.
4. **Report** findings grouped by component, then summarize cross-cutting reuse opportunities.

## Audit Checklist

### Performance
- [ ] Large lists rendered without `v-for` key or without `<VirtualList>` / windowing
- [ ] Expensive computed values that are not memoized (re-derived inline in template)
- [ ] Watchers with `immediate: true` that could be replaced by a computed property
- [ ] `v-if` / `v-show` misuse — `v-show` on a component that mounts only once is wasteful; `v-if` inside tight loops forces repeated DOM teardown
- [ ] Missing `defineAsyncComponent` / lazy imports for heavy child components
- [ ] Props passed as objects/arrays without `shallowRef` / `shallowReactive` where deep reactivity is unneeded
- [ ] Event handlers recreated on every render (anonymous arrows in template instead of methods/`useCallback`-style composables)
- [ ] Unnecessary `watch` with `deep: true` on a large object when a specific path would suffice
- [ ] Missing `v-memo` on static or rarely-changing subtrees in hot render paths
- [ ] `nextTick` called repeatedly inside a loop

### Code Reuse
- [ ] Duplicate logic across `>= 2` components that could move to a composable
- [ ] Repeated template fragments (>= 5 lines) that could become a child component or slot
- [ ] Prop drilling more than 2 levels deep — consider `provide` / `inject` or a composable
- [ ] Copy-pasted styles that belong in a shared CSS file or design-token variable
- [ ] Business logic living directly in a component that should be in a store (Pinia/Vuex) or service module
- [ ] Multiple components importing and configuring the same third-party library (axios, dayjs, etc.) without a shared wrapper

## Report Format

For each component with findings:

```
### <ComponentName> (<relative/path.vue>)

**Performance**
- <issue>: <one-sentence explanation> → <concrete fix>

**Code Reuse**
- <issue>: <one-sentence explanation> → <concrete fix>
```

After all per-component sections, add:

```
## Cross-Cutting Opportunities
- <opportunity>: which components are affected and what shared abstraction to extract
```

Finally, a one-paragraph **Summary** of the most impactful changes to make first.

## Rules
- Do NOT modify any files during analysis — this is read-only.
- If a potential issue requires runtime profiling to confirm (e.g., re-render frequency), flag it as "⚠ needs profiling" rather than stating it as a definite problem.
- Cite the specific line number(s) for every finding.
- If a component has no findings, say "No issues found" and move on.
- Do not invent problems to fill the report — only report what you actually observe in the code.
