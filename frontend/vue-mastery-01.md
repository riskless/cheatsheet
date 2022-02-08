### Build a Gmail Clone with Vue 3
- Flexible Events
  - Got a useful pattern for communicating requests from a child component to a parent component.
- Multi-select with Reactive Sets
  - Use a Set and a Reactive object to track which checkboxes are checked.
```vue
import { reactive } from 'vue';
setup() {
  //...
  let selected = reactive(new Set())
  let emailSelection = {
    selected,
    toggle(email){
      if(selected.has(email)) {
        selected.delete(email)
      } else {
        selected.add(email)
      }
    }
  }

  return {
    emailSelection,
    ...
  }
}
```
