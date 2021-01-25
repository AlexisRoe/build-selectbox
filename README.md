# build-selectbox

Learn to build a single component in plain html, css, js - a select box by following a tutorial by [Web Dev Simplified](https://www.youtube.com/watch?v=Fc-oyl31mRI&list=WL&index=49&t=150s).

<p>You can find the Web dev Simplified [Github Starter Kit](https://github.com/WebDevSimplified/custom-select-dropdown) here.</p>

## Lessons learned

This approach uses the build in state management of the input select field to store informations. Its actually hidden inside the code. If the Javascript is deactivated, there is still a selectbox available.

The functionality is build with an object and a lot of eventListener.

The Styling is very basic, but it can be everything you like.

<br/>

### Translation

Because i found the naming a little destracting here my personal list

| js-property          | html-tag |                                representation |
| -------------------- | :------: | --------------------------------------------: |
| customElement        |   div    |                 the custom selectfield mockup |
| labelElement         |   span   | the label/ name inside the custom selectfield |
| optionsCustomElement |    ul    |                               the list itself |
| optionElement        |    li    |            option-tag inside the select-field |

<br/>

### Lessons

<br/>

**RECAP: debouncing for searching input (500ms)**

```js
let debounceTimeout;
let searchTerm = '';
...
clearTimeout(debounceTimeout);
searchTerm += event.key;
debounceTimeout = setTimeout(() => {
    searchTerm = '';
}, 500);
```

<br/>

**RECAP: handling key strokes**

```js
select.customElement.addEventListener('keydown', (event) => {
        switch (event.code) {
            case 'Space':
                ...
                break;
            case 'ArrowUp':
                ...
                break;
            case 'ArrowDown':
                ...
                break;

            case 'Enter':
            case 'Escape':
                ...
        }
```

<br/>

**getting the informations stored in option state**

```js
export default class Select {
    constructor(element) {
        ...
        this.options = getFormattedOptions(element.querySelectorAll('option'));
        ...
    }
}
...
function getFormattedOptions(optionElements) {
    return [...optionElements].map((optionElement) => {
        return {
            value: optionElement.value,
            label: optionElement.label,
            selected: optionElement.selected,
            element: optionElement,
        };
    });
}
```

<br/>

**creating the list inside the custom made select field**

```js
select.options.forEach((option) => {
    const optionElement = document.createElement('li');
    optionElement.classList.add('custom-select-option');
    optionElement.classList.toggle('selected', option.selected);
    optionElement.innerText = option.label;
    optionElement.dataset.value = option.value;
    optionElement.addEventListener('click', () => {
        select.selectValue(option.value);
        select.optionsCustomElement.classList.remove('show');
    });
    select.optionsCustomElement.append(optionElement);
});
```

<br/>

**giving a div a focus**

```js
select.customElement.tabIndex = 0;
```

<br/>

**react on loosing focus on an element**

```js
select.customElement.addEventListener('blur', () => {
    select.optionsCustomElement.classList.remove('show');
});
```
