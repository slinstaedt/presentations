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

# **DDD: Aggregates**
**What's the fuzz about?**

---

<!-- paginate: true -->

--- 

# Aggregates: don't be fooled!

It's not about:
- an UML concept, that implies a relationship between [objects which can exist indepedently of each other](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-aggregation-vs-composition/)
- a structured hierarchy of [data](https://medium.com/tuanhdotnet/differences-between-dto-and-aggregate-in-domain-driven-design-best-practices-explained-ebdccd359378), that can be shared accross systems
- a collection of [entities](https://www.jamesmichaelhickey.com/domain-driven-design-aggregates/), that you can dump into your database tables
- [relationships](https://codeopinion.com/aggregate-ddd-isnt-hierarchy-relationships/)   between entities, for which you can paint nice E/R diagrams

---

# So what else it is about?

---

# Aggregates

> A DDD aggregate is a cluster of domain objects that can be treated as a *single unit*. [...]
> An aggregate will have one of its component objects be the *aggregate root*. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the *integrity* of the aggregate as a whole. [...]
> Aggregates are the basic element of transfer of *data storage* - you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.
>
> -- [Martin Fowler](https://martinfowler.com/bliki/DDD_Aggregate.html)

---

# Consistency, invariants and validation... oh my

> An invariant is a business rule that must always be consistent.
>
> -- [Vaughn Vernon](https://www.dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_1.pdf)

---

```java
void boolean hasBirthday(String timestamp) {
  /* magic domain logic here */
}
```
