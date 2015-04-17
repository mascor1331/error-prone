<!--
*** AUTO-GENERATED, DO NOT MODIFY ***
To make changes, edit the @BugPattern annotation or the explanation in docs/bugpattern.
-->

---
title: UnnecessaryTypeArgument
layout: bugpattern
category: JDK
severity: ERROR
maturity: MATURE
---

<div style="float:right;"><table id="metadata">
<tr><td>Category</td><td>JDK</td></tr>
<tr><td>Severity</td><td>ERROR</td></tr>
<tr><td>Maturity</td><td>MATURE</td></tr>
</table></div>

# Bug pattern: UnnecessaryTypeArgument
__Non-generic methods should not be invoked with type arguments__

## The problem
Surprisingly, JLS §15.12.2.1 allows non-generic methods to be invoked with type
arguments:

> a non-generic method may be potentially applicable to an invocation that
> supplies explicit type arguments. Indeed, it may turn out to be applicable.
> In such a case, the type arguments will simply be ignored.

> This rule stems from issues of compatibility and principles of
> substitutability.  Since interfaces or superclasses may be generified
> independently of their subtypes, we may override a generic method with a
> non-generic one. However, the overriding (non-generic) method must be
> applicable to calls to the generic method, including calls that explicitly
> pass type arguments. Otherwise the subtype would not be substitutable for its
> generified supertype.

However, in cases where the method being called is not an override of a generic
method, there is no reason to allow this, and it is potentially confusing.
This check flags only cases where the substitutability principle does not
apply.

## Suppression
Suppress false positives by adding an `@SuppressWarnings("UnnecessaryTypeArgument")` annotation to the enclosing element.