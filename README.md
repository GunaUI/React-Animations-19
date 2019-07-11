## CSS Transition
* Here in CSS Transition we will add classNames which determines which classes we should add to this wrapped (modal) element whcih depends on the state of the transition.

* For classNames alone we have to give the base class name and then we followed by other class name based on the base classNames
```jsx
 <CSSTransition in={this.state.modalIsOpen} timeout={animationTiming} mountOnEnter unmountOnExit classNames="fade-slide">

    <Modal show={state} closed={this.closeModal} />
    
</CSSTransition>
```
```jsx
.fade-slide-enter {

}

.fade-slide-enter-active {
    animation: openModal 0.4s ease-out forwards;
}

.fade-slide-exit {

}

.fade-slide-exit-active {
    animation: closeModal 1s ease-out forwards;
}
```
* CSSTransition applies a pair of class names during the appear, enter, and exit states of the transition. The first class is applied and then a second  *-active class in order to activate the CSS transition. After the transition, matching *-done class names are applied to persist the transition state.

* Refer http://reactcommunity.org/react-transition-group/css-transition

## Customizing CSS Classnames
```jsx
<CSSTransition in={this.state.modalIsOpen} timeout={animationTiming} mountOnEnter unmountOnExit classNames={{
    enter: '',
    enterActive: 'ModalOpen',
    exit: '',
    exitActive: 'ModalClosed'
}}>

    <Modal show={state} closed={this.closeModal} />
    
</CSSTransition>
```
## Animate List - TransitionGroup

 *  How to add animation to list we can't do with Transition and CSSTransition alone.. this wouldn't work, instead we have to additional transitional component TransitionGroup
 * TransitionGroup can be used where your output lists.. 
 ```jsx
 import TransitionGroup from "react-transition-group/TransitionGroup";
<TransitionGroup className="List" component="ul">
    {listItems}
</TransitionGroup>
 ```
 * TransitionGroup will work only if we chunck with Transition or CSSTransition
 ```jsx
    <CSSTransition key={index} classNames="fade" timeout={300} >
        <li 
        className="ListItem" 
        onClick={() => this.removeItemHandler(index)}>

        {item}
        </li>
    </CSSTransition>
 ```
 * Note here in CSSTransition we didn't use "in" property on wrapped CSSTransition component ofcourse we can't use in property since we are using dynamic list
 * For CSSTransition we need to apply animation css class as follows.
 ```jsx
 .fade-enter {
    opacity: 0;
}

.fade-enter-active {
    opacity: 1;
    transition: opacity 0.3s ease-out;
}

.fade-exit {
    opacity: 1;
}

.fade-exit-active {
    opacity: 0;
    transition: opacity 0.3s ease-out;
}
 ```
 * There are alternative to this package one popular alternatibe is react motion refer : https://github.com/chenglou/react-motion
 * Another alternative react move refer : https://react-move.js.org/#/getting-started/features
 * Finally react-router-transition refer : https://github.com/maisano/react-router-transition


