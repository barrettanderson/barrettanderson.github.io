---
layout: post
title:      "Last, but not least.  "
date:       2020-10-16 22:23:53 +0000
permalink:  last_but_not_least
---

**A commentary on the efficacy of, and a writer's growing admiration for, redux.**

Initially getting into React, it just looked wrong.  As I was looking at the beginning code, I saw, what I thought at the time, a smattering and combination of javascript and html smashed together in some unholy matrimony (not to mention css as the icing on the cake."  Example below:

```
export class CopingSkillList extends Component {
    render() {
        const coping_skills = this.props.coping_skills.map((coping_skill, i) => <CopingSkillCard key={i} coping_skill={coping_skill}/>)
        return (
            <div className="container-fluid bg-success">
                <div className="card-header"><h2>Coping Skills</h2></div>
                    <div className="card-body">
                        <h5 className="card-title">When I am mad, sad, or upset I manage by:</h5>
                        <ul>
                            { coping_skills }
                        </ul>
                        <br />
                        <h3>"New" Cope Sequence</h3>
                        <p><u>C</u>alm: I can calm by?</p>
                        <p><u>O</u>ptions: My options/choices are?</p>
                        <p><u>P</u>erform: My best options are?</p>
                        <p><u>E</u>valuate: How did my option work?</p>
                        <br />
                        <h3>Add Coping Skill</h3>
                        <CopingSkillForm />
                    </div>
            </div>
        )
    }
}
```
If you are not thoroughly disgusted by that, then you have my deep admiration.  It looked so odd just a few weeks ago, these different languages mixed together.  My project was to digitize a Trigger Card, a mindfulness tool used while I worked in psych to be aware of our triggers, warning signs, and coping skills.  This tool was used constantly by both patients and staff as we helped them to build more healthy coping strategies and skills (and certainly learned a lot along the way).

Fast forward a couple years, I am now wrapping up in my last module at Flatiron, and on a walk trying to figure out how I wanted to form a project using React-Redux.  All of a sudden, it clicked.  How cool would it be to use React-Redux, persisting data into a rails API, in order to take my now well worn half sheet of card stock paper, to be a computer program.

So what is this?  React, released originally in 2013, according to a quick [google](http://www.google.com)search takes you to their [documentation page](https://reactjs.org/).  They are a > A JavaScript library for building user interfaces> .  The design is component-based in order to create interactive UIs.  Redux, entering our world 2 years later in 2015, works alongside React.  Again, a quick google adventure takes you to their [documentation page](https://redux.js.org/) which feels comically similar to the layout of the React documentation page.  Take a look for yourself.  What Redux is designed for is to create a centralized store that components in the app would have access to (and edit it) without having to pass it through props (especially when the file that needs it is the great-great-grandchild of when this state is initially set).

The first couple weeks learning React I felt I had a pretty good handle at how things were working... at least once I could wrap my head around props and state.  But once we had integrated Redux, it felt I would never get a handle on it (as most new coding ideas feel for me).  But as I worked on this project, I realized how awesome and intuitive it became.

For example, and this just happens to be the biggest challenge (I think) in this app is that, in my code, I have a button that lets the user delete an object (because let's be honest, 50% of the time when we are asked what is this, a javascript object is a pretty good guess, the other 50 it is probably a function, but I digress).  The button's onclick function is 
```
handleClick = () => {
        this.props.deleteCopingSkill(this.props.coping_skill.id)
    }
```

I only have to pass props one child down from its parent, my CopingskillCard which is a presentational component which renders the data of each object, saved in store.  To update this info in the button component, one only has to do two things.  First, to connect to this store, make sure to `import { connect } from 'react-redux';` and then on export, we do `export default connect(null, { deleteCopingSkill })(CopingSkillButtons)`.  This connect function we import from react-redux, and it takes in two arguments.  The first argument is `mapStateToProps`, used to pull information from the store.  The second argument here is mapDispatchToProps, I am passing in some delicious syntactic sugar the object `deleteCopingSkill`.  This allows me to pass in this function to delete a specific object within the store.

I don't even NEED to write the `mapDispatchToProps` function.  Otherwise, it would look like this
```
const mapDispatchToProps = dispatch => {
    return {
		    deleteCopingSkill: id => {
				dispatch(deleteCopingSkill(id))
		 }
		}
}
``` 
but what I can do instead is to just pass in the object { deleteCopingSkill } and it knows that this would be this mapDispatchToProps function.

The handleClick function above is what was giving me the most issues.  I know that I would be importing the action, which would dispatch an action type "DELETE_COPING_SKILL", but I had a hard time getting to what would need to get passed in.  In order to do this, I would need to make sure some info is passed down to this child from it's parent.  And because this is getting persisted into rails api backend, it has an id with the object, and voila! `this.props.coping_skill.id `ensures that the action with the correct object is dispatched to the reducer, thus updating my views, and the user realizes that that coping skill they had before was not cutting it, and needed to be deleted.

In review, Redux creates a centralized store.  An action is dispatched when a user interacts with the applicaiton (in this project, opening up the page dispatches actions to display my data).  The root reducer is then called with the current state and dispatched action, and the store notifies the views by invoking their callback functions.

Looking back at these 5 months, it feels cool to see how I am just starting to get a handle on the technical understanding of what is going on, and I look forward to whatever is coming how I can fuse and bring my other interests and passions and use these languages as tools to better provide accessibility and functionality.

