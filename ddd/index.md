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

# **Domain driven design**
Some fundamentals

---

<!-- paginate: true -->
# Terms that may need some clarification

<div class="columns2" align="center">
<div>

## domain
## subdomain
## domain model

</div>
<div>

## consistency
## invariants
## validation

</div>
</div>

---

# Domains


---

# Subdomains


---

# Consistency, invariants and validation... oh my

> An invariant is a business rule that must always be consistent.
>
> -- [Vaughn Vernon](https://www.dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_1.pdf)

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
