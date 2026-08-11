# A Green CI Run Is an Input, Not a Conclusion

A pull request with a row of green checks is useful evidence. It shows that
some code ran on a known revision against a defined set of assertions. It is
not a proof that a change is correct in the way a user, maintainer, or
downstream integration cares about.

The practical question is not whether to trust CI. It is: **what does this
green result cover, and what does it leave unknown?**

## Start with the claim, not the implementation

Before writing or accepting a change, describe its user-visible claim in one
sentence. For example:

> Invalid values from an external map event must not become application state
> or cause a synthetic camera-change event.

This is stronger than “add a finite-number check.” It describes the boundary:
bad external input must neither contaminate state nor create a new output.

The same approach applies to loaders, migrations, and caches. “Add support
for visual circuit files” is vague. “A caller can load standalone and
multi-circuit files through the public API, selecting the intended circuit when
needed” can be tested and reviewed. Public examples of these two shapes are
linked in the [engineering evidence](../ENGINEERING-EVIDENCE.md) and in the
[QDK integration](https://github.com/microsoft/qdk/pull/3291).

## Use an evidence ladder

For a bounded change, collect evidence at five different layers:

1. **Reproduction**: show the old failure or missing behavior on a relevant
   version.
2. **Contract**: state the input, output, or state transition that changes and
   what must remain unchanged.
3. **Regression test**: make the actual contract fail before the change and
   pass after it.
4. **Counterexample**: test the adjacent case that would make a simplistic fix
   look correct while breaking the boundary.
5. **System check**: build, type-check, run, or exercise the public path that
   consumers use.

CI often covers the last layer well. A unit test may cover the third. The most
costly omissions are usually the first, second, and fourth.

For a numeric input guard, counterexamples include `NaN`, infinities, missing
nested fields, a valid partial snapshot, and unchanged state that must not
emit an event. For a loader change, they include an empty file, multiple named
entries, an invalid override, and the old default path. These are normal edges
of an interface, not exotic tests.

## Read tests as a contract map

“Tests passed” is too compressed for a review note. Translate the verified
behavior into ordinary language:

- valid input updates the intended state;
- invalid input is ignored without an extra notification;
- a named resource is selected when requested;
- the default behavior remains unchanged;
- failure is reported rather than silently converted to success.

This lets a reviewer see whether the suite proves the requested behavior or
merely exercises the new lines. If no test sentence describes a fallback,
ordering rule, or mutation boundary, that path is probably being assumed rather
than tested.

## Separate facts from inference in the PR

Avoid “fully fixed” when the evidence is narrower. A useful PR description has
three compact sections:

| Section | Contents |
| --- | --- |
| Changed | Concrete code path and visible behavior changed. |
| Verified | Commands actually run and cases they cover. |
| Not verified | Environments or integrations not exercised. |

This is not defensive writing. It directs review attention to the uncertainty
that remains and makes a green run more useful because its scope is explicit.

## A small review checklist

Before merging a narrow change, ask:

1. Can the user-visible claim be stated without an implementation detail?
2. Does a regression test encode that claim?
3. What is the closest invalid, missing, reordered, or duplicated input?
4. Could the fix mutate state or emit output when it should only ignore input?
5. Which command validates the public integration, not just the edited file?
6. What remains untested, and is that limitation visible?

A green pipeline is good news. It becomes decision-grade evidence when it sits
alongside a clear contract, an adversarial edge case, and an honest statement
of the environments it did not cover.
