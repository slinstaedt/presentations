---
theme: default
_class: [invert, lead]
class: []
color: #000000
#backgroundColor: #aaaaaa
backgroundImage: url('https://marp.app/assets/hero-background.svg')
marp: true
#header: 'Header content'
#footer: 'Footer content'
style: |
  .columns2 {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

---

<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true, theme: 'default', look: 'handDrawn' });
  window.addEventListener('vscode.markdown.updateContent', function() { mermaid.init() });
</script>

![bg left:40% 80%](https://marp.app/assets/marp.svg)

# **Domain values**

---

<!-- paginate: true -->
# What's not the concern of a domain value?

- **Persistence**: They shouldn't be concerned with how data is stored, e.g. in databases or files
- **UI**: They should not deal with how information is presented or interacted with on the frontend
- **Infrastructure**: They are independent of frameworks, external services, or system integration layers
- **Behavior**: They typically represent concepts or measurements, not processes or workflows (those belong to domain services or entities)
- **Context-specific validation**  – Basic invariants are okay, but business-rule-heavy logic often belongs elsewhere in the domain model

---

# NB: Domain value vs value object

<div class="columns2">
<div>

## Domain value
Refers to a meaningful unit of information used in the ubiquitous language.
Not always tied to a specific implementation, e.g. could be a value object, enum, literal, etc.

:information_source: Is a **concept** from DDD

</div>
<div>

## Value object
Describe one way to implement domain values as a concept.

:information_source: Is a **design pattern** from DDD 

Both are used mostly interchangable.
</div>
</div>

---

# What characteristics to DV have then?

- VOs **represent domain concepts** that have meaning in the business domain.
- VOs are **immutable** - once created, their state cannot change. This ensures consistency, predictability, and easier reasoning about the domain.
- Unlike entities or aggregates DVs are **compared by their contents**, not by some unique ID. They are *equal*, not *identical*.
- VOs ensure being **always valid**. These are syntactical and universal validations.
- DVs are **highly reusable**. Since they’re self-contained, always valid and context-free, they can be composed into more complex domain objects or even used across different bounded contexts.

---

# Validation

<div class="columns2">
<div>

## Syntactical
- Often checked when parsing some textual representation into a DV
- Textual representation have to follow some known pattern (e.g. IBAN)
- Independent of any business logic
- Usually always required and universally applicable

:information_source: Part of the domain value
</div>
<div>

## Semantical
- Often done *after* syntactical validation
- Further narrows the validity of a DV according to some context-specific business logic
- Can vary across different use cases

:information_source: Part of an aggregate or domain service
</div>
</div>

---

Domain value checks: some examples

**Percentage**
&#10004; Ensures it's value being between 0 and 100
&#10060; *Discounts over 10% granted by managers*

**Date**
&#10004; Ensures a given string represents an valid date
&#10060; *ShippingDate must be at least 2 days after the OrderDate*
&#10060; *Birthday must be in the past*

**EmailAddress**
&#10004; Ensures a given string  represents an valid email address
&#10060; *We can send an email to this address*
&#10060; *The email address was not previously used to register an account*

---

# Show me ~~the~~ some code

<div class="columns2">
<div>

```java
class EmailAddress {

  private String value;

  String getValue() {
    return value;
  }

  void setValue(String value) {
    this.value = value;
  }
}
```
</div>
<div>

```java
class EmailAddressTest {

  @Test
  void shouldAlwaysBeTheSame() {
    var unit = new EmailAddress();
    unit.setValue("bob@hotmail.com");

    assertEquals("bob@hotmail.com", unit.getValue());
    
    unit.setValue("bob@gmail.com");

    assertEquals("bob@hotmail.com", unit.getValue());
  }
}
```
</div>
</div>

---

# Another attempt

<div class="columns2">
<div>

```java
class EmailAddress {

  private final String value;

  EmailAddress(String value) {
    this.value = value;
  }
}
```
</div>
<div>

```java
class EmailAddressTest {

  @Test
  void shouldBeEqual() {
    var unit1 = new EmailAddress("bob@hotmail.com");
    var unit2 = new EmailAddress("bob@hotmail.com");

    assertTrue(unit1.equals(unit2));
    assertTrue(unit1.hashCode() == unit2.hashCode());
  }
}
```
</div>
</div>

---

# Okay, okay... another attempt

<div class="columns2">
<div>

```java
record EmailAddress(String value) {
}
```
</div>
<div>

```java
class EmailAddressTest {

  @Test(excepted = Exception.class)
  void shouldBeAnInvalidEmailAddress() {
    new EmailAddress("Himmelspforte 666");
  }
}
```
</div>
</div>

---

# Let's reuse some standard functionality

<div class="columns2">
<div>

```java
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NonNull;

record EmailAddress(
  @NonNull @Email String value
) {}
```
</div>
<div>

```java
class EmailAddressTest {

  void shouldBeAnInvalidEmailAddress() {
    new EmailAddress(null);
  }
}
```
</div>
</div>

---

# We are getting close...

<div class="columns2">
<div>

```java
record EmailAddress(String value) {

  EmailAddress {
    assertValid(value);
  }

  static void assertValid(String value) {
    // assert a value pattern like foo@example.com
    // or throw
  }
}
```
</div>
<div>

```java
class EmailAddressTest {

  void shouldBeTheSame() {
    var unit1 = new EmailAddress("bob@hotmail.com");
    var unit2 = new EmailAddress("Bob@hotmail.com");

    assertEquals(unit1, unit2);
  }
}
```
</div>
</div>

---

# Finally

```java
record EmailAddress(String value) {

  EmailAddress {
    value = assertAndNormalize(value);
  }

  static String assertAndNormalize(String value) {
    // steps to do:
    // 1. assert text for being non-null
    // 2. normalize value to lower case
    // 3. assert a value pattern like foo@example.com
    // 3a. and return normalized value
    // 3b. or throw
  }
}
```

---

### Is LocalDateTime a value object?

Absolutely. 

Looking at the required properties of value objects, we can tell, that
1. As *time* is a mandatory concept in many sub-domains, it seems to be obvious, that ``java.time`` may provide required value objects for the domain value *time*.
2. ``java.time`` value objects are immutable, but provide additional methods for creating other value objects, e.g. ``Instant.plus(Duration): Instant``
3. They implement ``equals`` and ``hashCode`` correctly.
4. They are always valid by guarding against illegal VO creation.
5. They are highly reusable, as they only impose globally applicable invariants.

---

### Is my VO allowed to have additional methods or is it just about structs?

Of course.

Just ensure no local state is modified and all methods are *pure* from a functional perspective.

---

### Can I use JPA for mapping my VOs?

Sure.

Just ensure your model is not deteriorated by any requirements, a JPA provider may apply to your VO. Mapping it as an ``@Embeddable`` is recommended. Default constructors can be package private. Though fields can not be final, you should not introduce setters. That why access type fields is recommended. Also hibernate support mapping of java records.

---

# More questions?
