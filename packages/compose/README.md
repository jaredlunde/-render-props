# compose ()
A utility for flattening nested render props (function as child) component calls
safely and in a way that doesn't take a huge performance hit.

### Installation
```yarn add @render-props/compose``` or ```npm i @render-props/compose```

____

## `compose(Components <object {propName: Component}>)`
```js
const Composed = compose({
  toggle: Toggle,
  counter: Counter
})

<Composed toggle={propsPassedToToggle} counter={propsPassedToCounter}>
  {function ({toggle, counter}) {
    // toggle = render props returned by the Toggle component
    // counter = render props returned by the Counter component
  }}
</Composed>
```

____

## Usage
```js
import compose from '@render-props/compose'
import Toggle from '@render-props/toggle'
import Counter from '@render-props/counter'


const ToggleCounter = compose({toggle: Toggle, counter: Counter})

function SomeComponent (props) {
  /**
  This is the same as:

  function SomeComponent (props) {
    <Toggle initialValue={true}>
      {function (toggleContext) {
        return (
          <Counter initialValue={6} initialStep={4}>
            {function (counterContext) {
              const derivedProps = {
                toggle: toggleContext,
                counter: counterContext
              }

              return (
                ...
              )
            }}
          </Counter>
        )
      }}
    </Toggle>
  }
  **/
  return (
    <ToggleCounter
      toggle={{initialValue: true}}
      counter={{initialValue: 6, initialStep: 4}}
    >
      {({toggle, counter}) => (
        <div>
          <div>
            Toggle value: {JSON.stringify(toggle.value)}

            <button onClick={toggle.toggle}>
              Toggle me to {toggle.value === true ? 'false' : 'true'}
            </button>
          </div>

          <div>
            Counter value: {counter.value}

            <button onClick={counter.incr}>
              incr() me by {counter.step}
            </button>

            <button onClick={counter.decr}>
              decr() me by {counter.step}
            </button>
          </div>
        </div>
      )}
    </ToggleCounter>
  )
}
```