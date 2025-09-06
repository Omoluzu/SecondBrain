---
tags:
  - "#js"
---


```js
<script>
	document.addEventListener('wheel', function(event) {
		if (event.ctrlKey) {
			event.preventDefault();
		}
	}, { passive: false });
</script>
```