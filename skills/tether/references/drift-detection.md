# Drift Detection

Behavioral drift occurs when creation deviates from disciplined principles. This reference provides detection questions—not enforcement rules. Internalized discipline replaces external enforcement.

---

## Detection Framework

Drift manifests as:
- **Addition signals**: Creating more than requested
- **Complexity signals**: Introducing unnecessary abstraction
- **Scope signals**: Expanding beyond stated boundaries
- **Pattern signals**: Deviating from established conventions

---

## The Five Patterns

### 1. Over-Engineering

**Signals**: Creating interfaces for single implementations, adding generic parameters to specific functions, building plugin architectures for monolithic needs, factory patterns for direct construction.

**Detection Question**: Was this abstraction explicitly requested, required by a test, or present elsewhere in the codebase?

**If no to all**: Remove. Implement the concrete solution.

### 2. Scope Creep

**Signals**: Adding logging when not requested, error handling beyond requirements, utility functions for single use, configuration for fixed values.

**Detection Question**: Was this capability specified in the original request?

**If no**: Remove. Scope creep compounds; small additions accumulate.

### 3. Verbosity

**Signals**: Comments explaining obvious code, multiple examples for clear concepts, extensive documentation for simple functions, TODO markers.

**Detection Question**: Does this add information not already evident from the code/content itself?

**If no**: Remove. Code should be self-documenting; prose should be dense.

### 4. Pattern Violation

**Signals**: New file structures diverging from existing, different naming conventions, alternative import patterns, non-standard directory placement.

**Detection Question**: Does this match the patterns already established in this codebase?

**If no**: Halt. Align with existing patterns before continuing.

### 5. Future-Proofing

**Signals**: "In case we need..." additions, extensibility hooks unused, configuration for single values, versioning for v1-only formats, optional parameters without current use.

**Detection Question**: Does current functionality require this, or is it anticipating future needs?

**If anticipating**: Remove. YAGNI—You Aren't Gonna Need It.

---

## Quick Reference

| Pattern | Signal | Question | Response |
|---------|--------|----------|----------|
| Over-engineering | Abstractions not requested | Requested/tested/existing? | Remove if no |
| Scope creep | "And also" additions | In original request? | Remove if no |
| Verbosity | Excessive explanation | Adds non-obvious info? | Remove if no |
| Pattern violation | New architecture | Matches existing? | Halt, align |
| Future-proofing | Options/config not needed | Currently required? | Remove if anticipating |

---

## Application

Apply drift detection at each checkpoint phase. Detection questions are phase-agnostic—ask them whenever creating. If uncertain whether something belongs, the answer is probably "no"—remove and simplify.

---

## Self-Monitoring

During creation, periodically ask:
- Am I adding lines that don't map to requirements?
- Am I creating abstractions without test coverage?
- Am I handling errors without specified failure modes?
- Am I creating files when editing would suffice?

If yes to any: stop, remove, continue simpler.
