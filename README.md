## Modal Open/close
* To show/hide model apply the below css and js logics.
* Modal.css
```css 
.ModalOpen {
    display: block;
}

.ModalClosed {
    display: none;
}
```
* Modal.js
```jsx
const modal = props => {
    const cssClasses = [
        "Modal",
        props.show ? "ModalOpen" : "ModalClosed"
    ];

    return (
        <div className={cssClasses.join(' ')}>
            <h1>A Modal</h1>
            <button className="Button" onClick={props.closed}>
                Dismiss
            </button>
        </div>
    );
};
```
* We should do the samething in backdrop also.
```css
.BackdropOpen {
    display: block;
}

.BackdropsClosed {
    display: none;
}
```
* Backdrop.js
```jsx
    const cssClasses = ['Backdrop', props.show ? 'BackdropOpen' : 'BackdropClosed'];

    return <div className={cssClasses.join(' ')}></div>;
```
* Now we have to apply props and handler functions to Modal and backdrop in app.js
```jsx
state = {
  modalIsOpen: false
}

showModal = () => {
  this.setState({modalIsOpen: true});
}

closeModal = () => {
  this.setState({modalIsOpen: false});
}

// In render

<Modal show={this.state.modalIsOpen} closed={this.closeModal}/>
<Backdrop show={this.state.modalIsOpen} />
```
* Now modal open close working fine but we don't have any nice animations lets do that now.

## Using Transitions 
* modal.css
```jsx
.Modal {
    position: fixed;
    z-index: 200;
    border: 1px solid #eee;
    box-shadow: 0 2px 2px #ccc;
    background-color: white;
    padding: 10px;
    text-align: center;
    box-sizing: border-box;
    top: 30%;
    left: 25%;
    width: 50%;
    transition: all 0.3s ease-out;
}

.ModalOpen {
    opacity: 1;
    transform: translateY(0);
}

.ModalClosed {
    opacity: 0;
    transform: translateY(-100%);
} 
```
## Using Animation
```css
.ModalOpen {
    animation: openModal 0.4s ease-out forwards;
}
//forwards keeps the final animation.. else it will back to its original..

.ModalClosed {
    animation: closeModal 1s ease-out forwards;
}

@keyframes openModal {
    0% {
        opacity: 0;
        transform: translateY(-100%);
    }
    50% {
        opacity: 1;
        transform: translateY(90%);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes closeModal {
    0% {
        opacity: 1;
        transform: translateY(0);
    }
    50% {
        opacity: 0.8;
        transform: translateY(60%);
    }
    100% {
        opacity: 0;
        transform: translateY(-100%);
    }
}
``` 
## Limitations 
* If we inspect we could find the backdrop and modal DOM , the reason is its phycially present but not visible because we actually animating this. That means all our html code is always in the dom but not dispaly because the opacity is zero, this might not be what we want. This might slow down dom population and also this is not the best practice.

* We could conditionally apply dom but our animations will not apply ie our dom will be removed instantly before we apply animations.

## ReactTransitionGroup
* This is not an offical react package but this is developed by react community, this will allow us to smoothly animatate elements when they are added and removed from the DOM.

```jsx
npm install react-transition-group --save
```
* This react-transition-group allow us to use transition component, by using the transition component we could control the display of element especially animation.
```jsx
import Transition from "react-transition-group/Transition";

<Transition
            in={this.state.showBlock} 
            timeout={1000}
            mountOnEnter
            unmountOnExit
          >
          {
            state => (
              <div style={{
              backgroundColor: "red",
              width: 100,
              height: 100,
              margin: "auto",
              transition: 'opacity 1s ease-out',
              opacity: state === 'exiting' ? 0 : 1
              }}>
              </div>
            )
            }
          </Transition>
```
* mountOnEnter and unmountOnExit determines the dom add and remove form html... eventhough its invisible.
* refer : http://reactcommunity.org/react-transition-group/transition

* Apply this Transition in modal show/hide animation
```jsx
<Transition in={this.state.modalIsOpen} timeout={1000} mountOnEnter unmountOnExit>
    {state => 
        (
        <Modal show={state} closed={this.closeModal} />
        )
    }
</Transition>
```
* Modal.js class apply based on the Transition component state
```jsx
 const cssClasses = [
    "Modal",
    props.show=='entering' ? "ModalOpen" : props.show=='exiting' ? "ModalClosed" : null
  ];
```
## Wrapping the transition component.

* We can use this transition directlly in the modal too..
* Make sure timeout={1000} should match with the transition time else we won't see the entire animation transition.
* we could add entering and exit timing as follows
```jsx
const animationTiming = {
    enter: 400,
    exit: 1000
}
<Transition in={this.state.modalIsOpen} timeout={animationTiming} mountOnEnter unmountOnExit>
```
## Transition Event
* Sometimes we want to execute the code when the state of the execution finishes. for that we can add various call back functions ....
```jsx
<Transition 
          in={this.state.showBlock} 
          timeout={1000}
          mountOnEnter
          unmountOnExit
          onEnter={()=>console.log('onEnter')}
          onEntering={()=>console.log('onEntering')}
          onEntered={()=>console.log('onEntered')}
          onExit={()=>console.log('onExit')}
          onExiting={()=>console.log('onExiting')}
          onExited={()=>console.log('onExited')}
>
```

