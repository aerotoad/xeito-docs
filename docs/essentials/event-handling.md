# Event Handling

## Listening to Events

To listen to an event emitted by a component or DOM element, you can use the standard syntax for event listeners in HTML or the ``@`` syntax in Xeito.

```html
<button onclick=${this.handleClick}>Click me</button>
This is the same as:
<button @click=${this.handleClick}>Click me</button>
```
The ``@`` syntax is just a shorthand for the ``on`` attribute, it is not required but it is recommended to use it for consistency.

By doing this, the ``handleClick`` method will be called every time the button is clicked and the ``event`` object will be passed as a parameter.

## Inline Event Handlers

For simple use cases, you can use inline event handlers by passing an arrow function inside the template interpolation.

```html
<button @click="${()=>this.count++}">Increment count</button>
<p>Count is: ${this.count}</p>
```

## Method Handlers

All the methods of the component are automatically bound to the component instance, this means you can use them directly inside the template interpolation without having to bind them, it also means that you cannot call them with the () syntax by default because it will be called immediately instead of being passed as a reference.

```html
<button @click=${this.handleClick}>Click me</button> // This will work
<button @click=${this.handleClick()}>Click me</button> // This will not work
```

This means we need to use a different solution if we want to pass parameters to the method. This can be achieved by wrapping the method call inside an arrow function:

```html
<button @click="${()=>this.handleClick('Hello World')}">Click me</button>
```
The arrow function will be called when the button is clicked and it will call the method with the parameter. This arrow function will also receive the event object as a parameter, or any other data generated by the event, and we can pass it to the method if we want to use it.

```html
<button @click="${(e: Event)=>this.handleClick('Hello World', e)}">Click me</button>
```

## Form example

```typescript
import { Component, XeitoComponent, html } from '@xeito/core';

@Component({
  selector: 'app-form'
})
export class MyForm extends XeitoComponent {

  public name: string = '';
  public email: string = '';
  public password: string = '';

  handleInput(e: Event) {
    const target = e.target as HTMLInputElement;
    this[target.name] = target.value;
  }

  handleSubmit(e: Event) {
    e.preventDefault();
    console.log(this.name, this.email, this.password);
  }

  render() {
    return html`
      <form @submit=${this.handleSubmit}>
        <input type="text" name="name" @input=${this.handleInput} value=${this.name} />
        <input type="email" name="email" @input=${this.handleInput} value=${this.email} />
        <input type="password" name="password" @input=${this.handleInput} value=${this.password} />
        <button type="submit">Submit</button>
      </form>
    `;
  }

}
```