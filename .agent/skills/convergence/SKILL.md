# Convergence Skill

## Purpose

Prove that implemented code matches the approved intent and has no accidental extra scope.

## Inputs

- `constitution.md`
- feature `spec.md`
- feature `plan.md`
- feature `tasks.md`
- current implementation and tests

## Procedure

1. Map every requirement to implementation evidence.
2. Map every acceptance scenario to a test or explicit validation evidence.
3. Map every plan decision to the resulting implementation.
4. Check for missing work.
5. Check for partial work.
6. Check for contradictions.
7. Check for unrequested implementation.
8. Check package dependency boundaries and cycles.
9. Check the declared validation commands and results.
10. Record actionable gaps as convergence tasks.
11. Re-run implementation for the new tasks.
12. Repeat until the result is converged.

## Completion condition

Declare `CONVERGED` only when:

- no requirement gap remains;
- no acceptance scenario is unverified;
- no active task remains incomplete;
- no constitution/architecture violation exists;
- no unrequested feature was added.
