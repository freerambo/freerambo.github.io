---
layout:     post
title:      "ReactJS training1"
subtitle:   ""
date:       2019-01-14 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - startup
    
---



　　
## ReactJS

1. render can only return one object, must have a root 
2. each tag must have a end /
3. for attributed must be replaced by htmlFor
4. className instead of class

method variable is not accessable for other method. We need to move to a class variable.
and Add this.variable to use in method.

bind method.


```javascript

class Greeter extends React.Component{
			greetMsg = "[Default Great Message]";

// option 1
			constructor(){
			    super();
			    this.onBtnClick = this.onBtnClick.bind(this);
			}
            onBtnClick(){
                let username = prompt("enter username : ");
                this.greetMsg = `Hi ${username}, Have a nice day`;
                console.log("button clicked! ", this.greetMsg);

            }
// option 2
           onBtnClick = () => {
                let username = prompt("enter username : ");
                this.greetMsg = `Hi ${username}, Have a nice day`;
                console.log("button clicked! ", this.greetMsg);
            }
		    render(){
		        return (
					<div>
						<h1>Greater</h1>
						<hr/>
						<label htmlFor="username"> user name</label>
						<input type="text" id="" name=""/>
						//option 3
						<input type="button" value="Greeter" onClick={this.onBtnClick.bind(this)}/>
						<div className="highlight">
							{this.greetMsg}
						</div>
					</div>
				);
			}
		}

        ReactDOM.render(<Greeter/>, document.getElementById('root'));

```


control elements 
1. defaultValue 
2. Must have onChange, otherwise the control becomes readonly
3. value={model.get("attributed")}


---

### END

