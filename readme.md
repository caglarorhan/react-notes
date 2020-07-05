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
- shouldComponentUpdate (render oncesidir, render ve sonrasini tercihe bagli hle getirir (return true dondururse render devam eder, return false olursa render olmaz))
- 

**SyntheticEvent**

React intercept the real event and make wanted changes about this event. 

- onChange is triggering synthetic event.


**Components**
 
 -  


**NPM notes**
 - npm update (package.json daki bagimliliklari update eder (onlerinde ^ isareti olanlari))
 - npm audit fix (bagimliliklar dahil vulnerabilities olanlarin son surumlerine guncellenmesini saglar)
 

**GIT Notes**

- to clone an existing repo

    - go to the folder location you want to work on the terminal you use

         `git clone git@github.com:USERNAME/REPONAME.git LOCALFOLDERNAME`
- when you clone a repo to your local, your local 'remote' (target) value is the copied location, and it is called 'origin'.

    `git remote` returns `origin`    
    
    to change this first remove the origin
    `git remote remove origin`
   
   now `git remote` returns nothing ` `
    
   If you already have a repo or create new one, take it's SSH key address 
     `git@github.com:USERNAME/REPONAME.git`
   
   and to add new origin value
   `git remote add origin git@github.com:USERNAME/REPONAME.git`
   
   - remote is just a pointer to some repository that is not in your device
   
   now you can `push` or `pull` to `origin` and branch `master`
   
   `git push origin master`
   
   - after pulling a repository    run `npm install` to install dependencies, etc.
   
    
