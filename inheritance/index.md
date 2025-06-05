---
theme: default
_class: [invert, lead]
class: []
color: #000000
backgroundColor: #aaaaaa
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

# **Inheritance**
The good, the bad, the ugly

---

<!-- paginate: true -->
# Concepts

### *Super* class
- Also known as *base* or *parent* class
- Refers to a class from which other classes can inherit attributes and methods

### *Sub* class
- Also called *derived* or *child* class 
- Inherits from a super class, meaning it reuses the functionality of the base class and can add or modify it

---

<!-- paginate: true -->
# Inheritance
One of the fundamental building block of object-oriented programming. 

Often names together with
- encapsulation
- polymorphism
- abstraction

---

Use cases:
1. [External reuse](#external-reuse-concept)
2. Internal reuse
3. Abstract base implementation
4. Plug-in functionality
5. Specialization

---

# External reuse: concept

<div class="columns2">
<div>

Adds additional methods and/or properties without actually using parent class implementation itself.

Any client of the child class is supposed to use functionality of both parent and child class.

</div>
<div>
<pre class="mermaid" align="center">
classDiagram
    direction LR
    Child --|> Parent
    Client ..> Child
    Client ..> Parent
</pre>
</div>
</div>

---

## External reuse

```java
@MappedSuperclass
class BaseEntity {

    @Id
    @GeneratedValue
    private long id;

    @Version
    private Instant lastChanged;
}

@Entity
class PersonEntity extends BaseEntity {

    private String name;
}
```
--- 

## External reuse

```java
@Entity
class CustomerEntity extends PersonEntity {

    @Embedded
    private Address shippingAddress;
}

@Embeddable
record Address(/* properties */) {}
```
--- 

## External reuse

```java
@Entity
class OrderEntity extends CustomerEntity {

    @ElementCollection
    private List<OrderItem> items;
}

@Embeddable
record OrderItem(/* properties */) {}
```
--- 

## External reuse

```java
@Entity
class EmployerEntity extends PersonEntity {

    @Embedded
    private Company employedAt;
}

@Embeddable
record Company(long id) {}
```
--- 

## External reuse

```java
@Entity
class StudentEntity extends PersonEntity {

    @Embedded
    private University matriculatedAt;
}

@Embeddable
record University(long id) {}
```
--- 

## External reuse: questions

1. When to stop inheriting properties/methods from other classes?
2. How to inheriting properties/methods from multiple other classes?
3. How to prevent assuming an 'is-a' relationship vs an 'uses-a' one?

---

# Internal reuse: concept

<div class="columns2">
<div>

Adds additional methods and/or properties which are only used by the parent class implementation itself.

Any client of the child class is supposed to only use functionality of the child class.

</div>
<div>
<pre class="mermaid" align="center">
classDiagram
    direction LR
    Child --|> Parent
    Child ..> Parent
    Client ..> Child
</pre>
</div>
</div>

---

# Abstract base implementation: concept

<div class="columns2">
<div>

Abstract parent class, that provides default implementations to concrete child classes.

Any client of the child class is supposed to only use functionality of the parent class.

Both parent and child class have the same level of abstraction.

</div>
<div>
<pre class="mermaid" align="center">
classDiagram
    direction LR
    Child --|> Parent
    Client ..> Parent
class Parent {
    &lt;&lt;abstract&gt;&gt;
    *method1()
    method2()
}
class Child {
    method1()
}
</pre>
</div>
</div>

---

# Plug-in functionality: concept

<div class="columns2">
<div>

Abstract parent class, which provices a concrete implementation and calls back any downstream subdomains.

Any client of the child class is supposed to only use functionality of the parent class.

Parent and child class have the different level of abstraction.

</div>
<div>
<pre class="mermaid" align="center">
classDiagram
    direction LR
    Child --|> Parent
    Client ..> Parent
class Parent {
    &lt;&lt;abstract&gt;&gt;
    *method1()
    method2()
}
class Child {
    method1()
}
</pre>
</div>
</div>

---

# Specialization: concept

<div class="columns2">
<div>

Concrete parent class, which functionality needs to be specilized without the parent class have intended to do so.

Any client of the child class is supposed to only use functionality of the parent class.

</div>
<div>
<pre class="mermaid" align="center">
classDiagram
    direction LR
    Child --|> Parent
    Client ..> Parent
</pre>
</div>
</div>
