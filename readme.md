**File Structure is Important**

- src
    - components
        - component-name
            -componentname.component.jsx
            -componentname.styles.css
       
**State**

- State is converted into a props while stepping down in to a component.       
- Whenever state changed all sub positioning (under the states position in component tree) rerendered (render method called again). This is useful when some content effects the state from inside related component.     

**Life Cycle Methods**

- componentDidMount
- componentDidUpdate
- componentWillUnmount
- shouldComponentUpdate (render oncesidir, render ve sonrasini tercihe bagli yapar)
- 

**SyntheticEvent**

React intercept the real event and make wanted changes about this event. 

- onChange is triggering synthetic event.


**Components**
 
 -  


**NPM notes**
 - npm update (package.json daki bagimliliklari update eder (onlerinde ^ isareti olanlari))
 - npm audit fix (bagimliliklar dahil vulnerabilities olanlarin son surumlerine guncellenmesini saglar)
 
