```jsx
import React from 'react'
import GifListContainer from '../containers/GifListContainer'
import NavBar from './NavBar'

// the App component should render out the GifListContainer component 

const App = () => {
  return (
    <div>
      < NavBar color='black' title="Giphy Search" />
      <GifListContainer />
    </div>
  )
}

export default App

```

```jsx
import React from 'react'

function GifList(props) {
    let gifArray = props.gifs.map((gifObj) => <li><img src={gifObj.images.original.url} /></li>)
    console.log("gif array: ", gifArray)
    return (
        <ul>
            {gifArray}
        </ul>
    )
}

export default GifList
```

```jsx
import React from 'react'

class GifSearch extends React.Component {

    state = {
        searchTerm: ""
    }

    changeHandler = (e) => {
        this.setState({ searchTerm: e.target.value }, ()=> console.log(e))
    }

    submitHandler = (e) => {
        e.preventDefault()
        this.props.submitHandler(this.state.searchTerm)
        this.setState({ searchTerm: "" })
    }
    render() {
        return (

            <form onSubmit={this.submitHandler}>
                <input type="text" placeholder="search" value={this.state.searchTerm} onChange={event => this.changeHandler(event)} />
                <input type="submit" value="search" />
            </form>
        )
    }
}


export default GifSearch
```

```jsx
import React from 'react'

function NavBar(props){
  const colors = {
    black: 'navbar-inverse',
    white: 'navbar-default'
  }
  
  return (
    <nav className={`navbar ${colors[props.color]}`}>
      <div className='container-fluid'>
        <div className='navbar-header'>
          <a className='navbar-brand'>
            { props.title }
          </a>
        </div>
      </div>
    </nav>
  )
}

export default NavBar

```

```jsx
import React, { Component } from 'react'
import GifList from '../components/GifList'
import GifSearch from '../components/GifSearch'

class GifListContainer extends Component {

    state = {
        gifs: []
    }

    componentDidMount() {
        this.fetchGifs()
    }

    // componentDidUpdate() {

    //     console.log("Good job your search worked!")
    // }

    fetchGifs = (term = "dolphins") => {
        fetch(`https://api.giphy.com/v1/gifs/search?q=${term}&api_key=La2DSibqRdXcn3Dl1TkHEK0hnEta3cQJ&rating=pg&limit=3`)
            .then(resp => resp.json())
            .then(data => this.setState({ gifs: data.data }))
    }

    submitHandler = (searchTerm) => {
        this.fetchGifs(searchTerm)
    }

    render() {
        return (
            <React.Fragment>
                <GifSearch submitHandler={this.submitHandler} />
                <GifList gifs={this.state.gifs} />
            </React.Fragment>
        )
    }
}

export default GifListContainer
```

api.giphy.com/v1/gifs/search

| **GIF**                                                      | Sticker URL                      |      |
| ------------------------------------------------------------ | -------------------------------- | ---- |
| api.giphy.com/v1/gifs/search                                 | api.giphy.com/v1/stickers/search |      |
| https://api.giphy.com/v1/gifs/search?q=monkey&api_key=La2DSibqRdXcn3Dl1TkHEK0hnEta3cQJ |                                  |      |
|                                                              |                                  |      |

| Request Parameters:             | Example:                           | Description:                                                 |
| :------------------------------ | :--------------------------------- | :----------------------------------------------------------- |
| **api_key:** *string(required)* | La2DSibqRdXcn3Dl1TkHEK0hnEta3cQJ   | GIPHY API Key.                                               |
| **q:** *string(required)*       | `cheeseburgers`                    | Search query term or phrase.                                 |
| **limit:** *integer (int32)*    | `20`                               | The maximum number of objects to return. (Default: “25”)     |
| **offset:** *integer (int32)*   | `5`                                | Specifies the starting position of the results. Defaults to 0. |
| **rating:** *string*            | `g`                                | Filters results by [specified rating](https://developers.giphy.com/docs/optional-settings#rating). Acceptable values include g, pg, pg-13, r. If you do not specify a rating, you will receive results from all possible ratings. |
| **lang:** *string*              | `en`                               | Specify [default language](https://developers.giphy.com/docs/optional-settings#language-support) for regional content; use a 2-letter ISO 639-1 language code. |
| **random_id:** *string*         | `e826c9fc5c929e0d6c6d423841a282aa` | An ID/proxy for a specific user.                             |

