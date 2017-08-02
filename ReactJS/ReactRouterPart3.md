# Creating a Dynamic App with a Parent and Child Detail page.  

Often times in our programs, we will need to be concerned with having dynamic URLs for our user's experience. It would be impossible to for some applications to manually create a route for every endpoint the application would have, yet it is important that we generate endpoints that make sense in terms of being RESTful.

The goal is to have endpoints that follow a hierarchy that allow users to understand where they are in the website and make sure we consistently structure our URLs that way. Before we even begin talking about pull data from other RESTful APIs (Application Program Interfaces) and using that data to generate our URLs - let's simplify everything with a little bit of hard coded data and demonstrate how we can dynamically generate a URL endpoint for a detail element page on a parent element page. Let's continue using React Router 4 to do this!

Confusing? Well let's talk through what that all means as we go through.

## The Set Up  

We need data - so we have created a simple JSON object in a data file that we will use to essentially simulate what we would receive from an API. In this case we have will have an array of actor objects. **It's not important that you read every detail of this data, just to know that it exists and to see the overall structure it has.**

```jsx
//############ actors.js (in data folder we created) ###########
const actors =  [{
    firstName: "Bill",
    lastName: "Murray",
    suffix: "",
    birthName: "William James Murray",
    id: "a1",
    birthday: "September 21, 1950",
    birthplace: "Wilmette, Illinois",
    credits: "84 acting credits",
    firstMovie: "The Hat Act - 1973",
    bio: " Orion's sword brain is the seed of intelligence tingling of the spine light years paroxysm of global death intelligent beings, Vangelis, the carbon in our apple pies white dwarf globular star cluster, extraordinary claims require extraordinary evidence qui dolorem ipsum quia dolor sit amet across the centuries! Muse about, vastness is bearable only through love adipisci velit, rings of Uranus, trillion science Sea of Tranquility the sky calls to us Rig Veda, Drake Equation network of wormholes, citizens of distant epochs, realm of the galaxies vanquish the impossible, Drake Equation Apollonius of Perga a still more glorious dawn awaits consectetur and billions upon billions upon billions upon billions upon billions upon billions upon billions. ",
    imgThumb: "https://s-media-cache-ak0.pinimg.com/originals/f3/1c/42/f31c423ec0f01e1394c73e6827ff459c.jpg",
    imgFull:"http://fillmurray.com/300/300"
  }, {
      firstName: "Steven",
      lastName: "Seagal",
      suffix: "",
      birthName: "Steven Frederic Seagal",
      id: "a2",
      birthday: "April 10, 1952",
      birthplace: "Lansing, Michigan",
      credits: "55 acting credits",
      firstMovie: "Above the Law - 1988",
      bio: " Beardfish barbeled dragonfish inanga tarpon, trunkfish eelblenny, flatfish duckbill eel ghost fish dogfish shark, dwarf loach. Discus anchovy North American darter lumpsucker carpetshark, devil ray; glowlight danio African lungfish yellow weaver. Celebes rainbowfish long-finned pike anchovy squeaker mora, cisco; sea toad ground shark northern squawfish dragonet, Hatchetfish plunderfish flat loach smooth dogfish manefish, damselfish banjo catfish slipmouth long-whiskered catfish hairtail, armoured catfish saury.Platyfish Australian grayling electric ray Black scabbardfish common carp milkfish lagena common tunny manefish pelican eel redmouth whalefish. Milkfish, knifejaw chum salmon ridgehead clown triggerfish nase damselfish king-of-the-salmon turbot squeaker sarcastic fringehead. ",
      imgThumb: "https://s-media-cache-ak0.pinimg.com/originals/6e/21/3c/6e213cf0c258b72f2681d86f8027e819.jpg",
      imgFull:"http://stevensegallery.com/300/300"

  },
  {
      firstName: "Robert",
      lastName: "Downey",
      suffix: "Jr.",
      birthName: "Robert John Downey Jr",
      id: "a3",
      birthday: "4 April 1965",
      birthplace: "Manhattan, New York",
      credits: "87 acting credits",
      firstMovie: "Pound - 1970",
      bio: " Ancient alien gods petroglyph ancient civilization I know it sounds crazy..., UFO Mayan Easter island Puma Punku mercury Indian texts, Sumerian texts foo fighter pyramids Mayan Indian texts Annunaki. Puma Punku aircraft earth mound stonehenge mystery star gates anti-gravity Giorgio, gods Easter island Giorgio the vedas, I know it sounds crazy... Pyramids Nazca lines Machu Picchu. Von Daniken golden disk, I know it sounds crazy... Portal weightless kachina doll, DNA manipulation alien Giorgio sanskrit gods.",
      imgThumb: "http://i.ebayimg.com/00/s/NTAwWDQ1MA==/z/dPQAAMXQVT9S-z2g/$_3.JPG?set_id=2",
      imgFull:"http://rdjpg.com/300/300"
  }];

  export default actors;
```

## Root JavaScript File Set Up  

In our root JavaScript file we need to set up our program to connect to the DOM - this should be a comfortable step by now using ReactDOM and React to pass the component we wish to render inside of the `<div>` we designated in our HTML file.

```jsx
//########### index.js ##############
//import React
import React from 'react';
import ReactDOM from 'react-dom';

//import Styles
import './styles/index.css';

//import Components
import App from './scripts/components/App';

ReactDOM.render(
  <App />,
  document.getElementById('root'));
```
  
Nothing new there. The next step is to really start digging into our program and look at how we will generate our routes dynamically and set up our browser router to handle them.

## Router Set Up  

Our `<BrowserRouter />` is going to be set up in our "App.js" file - this absolutely could have been done in our index.js file too - but this is just an example that's been expanded out to help simplify the ideas behind dynamic URLs.

```jsx
//######## App.js #########
import React, { Component } from 'react';
import '../App.css';

//import React Router
import { BrowserRouter, Route, Switch } from 'react-router-dom';

//component imports
import HomeMenu from './home';
import BaseLayout from './base_layout';
import PeopleMenu from './people';
import ActorInfo from './actor_info';

class App extends Component {
  render() {
    return (
      <BrowserRouter>
        <BaseLayout>
          <Switch>
            <Route exact path="/" component={HomeMenu} />
            <Route path="/people/:actor" component={ActorInfo} />
            <Route path="/people" component={PeopleMenu} />
          </Switch>
        </BaseLayout>
      </BrowserRouter>
    );
  }
}

export default App;
```

Something to note about this set up is that we have used the path `path="/people"` twice to render two different components. There are a couple of important things to note about this. We use our most specific router `path="/people/:actor"` first, because the Router reads downward and will match the first thing to meet the criteria - we don't want our `<PeopleMenu>` component to render unless we are specifically on the '`/people`' path.

We could also use the `exact path=` to acheive the same result, as we did with the `/` path.

Secondly we pass in a dynamic route using the `:` + `name of route` (in this case we used `/people/:actor`). We can name our route anything we want to, but it is wise to name it in a way that will make sense to anyone following our code. We could have called it :coolGuy or :person and these endpoints would still make sense in the context of our route. The `:actor` path allows us to pass anything in to this portion of the URL and it will still render the component associated with it.

`/people` is our parent page route. `/people/:actor` is the child detail page route.

## Setting our Components up Dynamically  

So, we see the router set up, but how do we generate a dynamic route. Lets take a top down approach and talk through each piece of the puzzle as we move into the application. Our first stop is going to be the `<PeopleMenu>` component.

`<PeopleMenu />`  

We have a file "people.js" that houses our `<PeopleMenu />` component. This will be our parent page, and we will render this on the path `/people`. This will give us the options to click on and then view details of (with the child component). Let's walk through the code:

```jsx
//############ people.js ############

import React, { Component } from 'react';
import { NavLink, Link } from 'react-router-dom';

//import JSON object
import actors from "./data/actors.js";

export default class PeopleMenu extends Component {

  render() {
    //get access to the match object
    let match = this.props.match;
    //map through our JSON object array and create a NavLink for each object inside of the JSON array.
    let NavPeeps = actors.map((actor)=>{
      return (
        <div key={actor.id} className={actor.firstName}>
          <NavLink activeClassName="selected" className="navlink" to={`${match.url}/${actor.firstName}`}><img src={actor.imgFull} alt={actor.firstName}/>
          </NavLink>
        </div>
    )});
    return (
      <div className="pictures">
        <Link className="btn btn-large btn-primary" to="/">Back To Home</Link>
        {NavPeeps}
      </div>

    );
  }
}
```

We first create a component `PeopleMenu`, and then render that component based on a few specific instructions. We want our application to be able to accept an array of objects as large or small as we want it to be. This is power behind a dynamic application. Our JSON object array could be 3 actors/actresses or 30 in length and we will still render correctly.

The first step is get access to the match object. The match object has a lot of powerful tools that allow us to utilize the data from the URL and parameters being passed in to the URL and path name.

In this instance we create a variable `match` using the ES6 `let` syntax. This just shortens the amount we have to type to access the `props.match.url` object.

We then map over our JSON data and create a `<div>` that holds a `<NavLink>` component for each item in the array. If you will recall, the `<NavLink>` gives us access to a special `<Link>` component that can be styled with active styles easily. We could definitely substitue `<Link>` for `<NavLink>` and not lose the functionality of our program.

We make our `<NavLink>` dynamic with a template literal in the `to=` property. Template literals are new to ES6 syntax and allow us to use JavaScript inline with strings. This prevents us from having to use a lot of concatenation needed in ES5. We begin a template literal with using "`` ` back-ticks ` ``" (the same key as the tilde: "~" ).

We then begin our template literal variable with a dollar sign `$` and brackets `{}`. So when completed we've got something like this:

```jsx
<NavLink to={`${match.url}/${actor.firstName}`} />
```

The above code essentially translates to the following :

"find the url that brought me here + "/" + **add the actor's first name to the end of it**}"

So in the case of how we set our routes up in the Router we get "/people/*firstName*" or for a real example of our application "`/people/Bill`". So as we map over our JSON, we create a navigation link to each actor using an endpoint /people/(their name). Cool.. that's dynamic!

We fill in some other data (like a picture for our link) with data from our JSON opject being mapped over and then return each NavLink in our return statement for our render method.

In our case this yields something along the lines of:

![PeopleMenu](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/PeopleMenu.png)

With each picture being a link to the child detail page of that actor!

Now it's time to look at the child detail page...

`<ActorInfo />` 

Stepping down the chain of hierarchy we come to our `ActorInfo` component. We've created this component inside of a file "actor_info.js". Let's look at this code and work out what is going on with it as we do.

```jsx
//############# actor_info.js #################

import React, { Component } from 'react';
import { Link } from 'react-router-dom';

//import our JSON object
import actors from "./data/actors.js";

//import Components
import Actor from './actor.js';

//create the class ActorInfo
export default class ActorInfo extends Component {

  render() {

    //accessing the name of the actor from the match object using params and the ":actor" endpoint
    const {actor} = this.props.match.params;

    //map through all actors
    let myPeeps = actors.map((celeb) => {

      //if actor matches the name of the URL endpoint, render <Actor>
      if (celeb.firstName === actor) {
        return (
              <Actor key={celeb.id} data={celeb} />
            );
          }
        }
      );

    return (
      <div>
        <Link className="btn btn-large btn-danger" to="/people">Back To Celeb Menu</Link>
        {myPeeps}

      </div>

    );
  }
}
```

The first is getting access to the endpoint of our URL, so that we can be sure to render the correct actor depending on the endpoint. This of course could all be hard coded, but setting it up dynamically allows us to reuse this code no matter how many people we have on our JSON data.

We can use some more ES6 syntax with our React Router match object to gain access to the `:actor` part of our URL. We can create a `const` variable and access the `params` method on the `match` object. Params allow us to use the wildcard (dynamically created endpoints) part of our route.

If we were to `console.log(this.props.match.params)` we would return an Object that had a key "`actor`" with the value "`Bill`". If we had more than one dynamic endpoint, we would have access to the whole chain. For instance if `/people/:actor/:movies` was our route, we would then have an object containing the movies passed into the `:movies` path variable. By using the `{actor}` portion of our declaration, we simply have accessed the actor part of the object, returning Bill as our variable.

```jsx
const {actor} = this.props.match.params;
// actor = "Bill" if we were on /people/Bill route
```

What this allows us to do is to match our dynamically created `<Actor>` component to the actor information from our endpoint. Simply put, if we have `/people/Bill` then we want to see Bill's information on the page.

We do that with a simply `if` statement in our render method to make sure we have the right information.

```jsx
let myPeeps = actors.map((celeb) => {

  //if actor matches the name of the URL endpoint, render <Actor>
  if (celeb.firstName === actor) {
    return (
          <Actor key={celeb.id} data={celeb} />
        );
      }
    }
  );
```

So when the endpoint matches the first name of the actor in our JSON data, we will render an `<Actor>` component with that actor's information.

The `<Actor>` component is in another file, but receives it's data from the `data=` property being passed in the value of our mapped array `{celeb}`. This really concludes how the dynamic routes are created, but just for closure, let's take a look at the `<Actor>` component in our "actor.js" file.

`<Actor />`  

Inside of our `<Actor>` component, we process the information being passed in as data and render it to the page to complete the child detail view.

```jsx
//########## actor.js ###########
import React from 'react';


const Actor = (props) => {
  //create a variable "actor" with the data passed in via props.
  let actor = props.data;
  return (

    <div className="actor-stuff">
      <ul className="actor_bio_ul">
        <li className="actor_content" key={actor.id}>
          <img className="actor-image" src={actor.imgThumb} alt="actor" />
          <div className="actor-info">
            <h3>{actor.name}</h3>
            <h5>Birthname: {actor.birthName}</h5>
            <h5>Birthday: {actor.birthday}</h5>
            <h6>Birth Place: {actor.birthplace}</h6>
            <h6>{actor.credits}</h6>
            <h6>First Movie: {actor.firstMovie}</h6>
          </div>
          <div className="actor-bio">
            <p><strong>Biography:</strong> {actor.bio}</p>
          </div>
        </li>
      </ul>
    </div>
  );

};

export default Actor;
```

We simply use props to pass the data down to the component via `data` and then create our component to display that data! We have now created a parent component `PeopleMenu` and a child detail component `ActorInfo` that are generated using dynamic routes!

Let's see it in action:

![dynamicURL-1](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/dynamicURL-1.gif)

![dynamicURL-2](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/dynamicURL-2.gif)

## Conclusion  

* Dynamic routes allow for our application to display RESTful endpoints with any data that way may take in and process.

* Using a RESTful means to create endpoints helps the user follow along with where they are inside of the application. A much better experience for them over all.

### References  

* [MDN](https://reacttraining.com/react-router/web/example/basic)
