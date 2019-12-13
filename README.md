## Project 2
# General Assembly Software Engineering : Flash Films

## Timeframe
2 Days

## Technologies Used
* HTML5
* CSS3
* JavaScript
* React.js

## Overview
This was a paired project that was done during a React hackerthon (Reactathon), where we had to use a third party RESTful API and incorperate it into a web application.

Flash Films is a website which uses an API called TheMovieDB (https://www.themoviedb.org/) and has been built to display films based upon either/both genre and actor selection, and will give a few details on particular films including synposis, runtime, release date, and similar films.

Website Link: https://flash-films.herokuapp.com/

## Instructions
1. Input the genre and actor you are interested in watching.
![search-screen](https://i.imgur.com/ZJyZRGc.png)
2. You will be brought to a page indexing all films based on your selection.
![index-screen](https://i.imgur.com/2x68Yo6.jpg)
3. You can click on a film which will bring you to a further page that provides details on the film selected, and gives other films you may be interested in.
4. You can also click on the movies shown in the similar films section to look at their details.
![show-page](https://i.imgur.com/k6SnpSe.jpg)
5. To go back to home, either click the lightning bolt, or "FLASH FILMS" in the top left corner.
*index page?*

## Process
This was a group project with 2 developers working together, [@Lydia Dalrymple](https://github.com/ldalrymple1) and I. The whole project was pair programmed. We both took turns in being the navigater and the driver; utilising this method, we were able to learn from each other and collaborate easily. As a group, we formulated a timetable which we continued to revised throughout the project. This allowed us to focus on priorities that directly impacted our MVP and the features that were required for it to function.

The first part of our Reactathon was to decide on our website and what API we were going to use. Once we had decided on what we were going to create, we looked into the documentation. We had to architect how exactly the searchs were going to work. This included having to format the actor name by making an actor search call to the API to give us their database ID, and having to get the selected genre ID by making a categories call to the API and filtering it out based upon the users selection.

Once we had the inital searches complete, we moved to the index page where we have the prepared IDs for either or both fields which could be called using TheMovieDBs search query. This gave us the selection of films which is shown. Furthering on this, once you select a film, the new page is created with the movie's API ID in the url which can be used in a new call to the API to provide further details which we can display.

```javascript    
handleSubmit(e) {
  e.preventDefault()
  const filteredGenre = this.filteredGenre()
  const formattedActorName = this.formatActorName()

  if (this.state.searchActor === '') return this.props.history.push(`/search/${filteredGenre}`)
  else {
    axios.get(`https://api.themoviedb.org/3/search/person?include_adult=false&query=${formattedActorName}&&page=1&language=en-US&api_key=${process.env.MOVIEDB_ACCESS_TOKEN}`)
      .then(res => {
        if (res.data.results.length === 0) {

          this.setState({ error: true, searchActor: 'Invalid search' })
          setTimeout(() => {

            return this.setState({ error: false })
          },3000)
          return
        }
        this.setState({ actorID: res.data.results.pop().id })
        this.setState({ submitting: true })
        setTimeout(() => {
          //add error if the typed name is wrong (NEED CLASSES)
          const actorIDConcat = `&with_people=${this.state.actorID}`
          this.props.history.push(`/search/${filteredGenre}${actorIDConcat}`)
        }, 800)

      })
  }
}
```
*Intial search feature.*

## Challenges
Intially, one challenge was to decide on what API(s) to use. Secondly, this was our first time using a third party API, and therefore looking through the documentation was initially a bit of a challenge. However, TheMovieDB's documentation and examples we're extensive and meant that were able to pick it up quickly after the inital learning curve.

## Wins
At the time, the React framework was still a little new to us. During the project we were able to navigate a couple of small hurdles, for example, passing information between pages based upon URL names. This gave us some more experience in troubleshooting, and creating projects React and resulted in our group becoming a lot more familiar and comfortable with the framework. This was invaluable as a learning experience, and is something we continued to build onto in our future projects.

## Further features
If we had more time, the database was extensive enough that we could have implemented a number of different features including; a TV section, a random movie button, a director field into the serach function.