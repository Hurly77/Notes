

## top

`index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import manageBand from './reducers/manageBand'
import { Provider } from 'react-redux';
import { createStore } from 'redux';

const store = createStore(manageBand)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
document.getElementById('root')
)

```

`App.js`

```jsx
import React, { Component } from 'react';
import BandsContainer from './components/BandsContainer'


class App extends Component {
  render() {
    return (
      <div className="App">
        <BandsContainer />
        
      </div>
    );
  }
};

export default App;
```

## Components

`Band.js`

```jsx
import React, { Component } from 'react';

class Band extends Component {

  render() {
    return(
      <div>
        <li>{this.props.band.name}</li>
        <button onClick={() => this.props.delete(this.props.band.id)}>Delete</button>
      </div>
    );
  }
};

export default Band;
```

`Bands.js`

```jsx
import React, {Component} from 'react'
import Band from './Band'

export default class Bands extends Component {

  render(){
    return (
      <div>
        <ul>
        {this.props.bands.map(band => {
          return <Band delete={this.props.delete} key={band.id} band={band}/>
        })}
        </ul>
      </div>
    )
  }
}
```

`BandInput.js`

```jsx
import React, { Component } from 'react';

class BandInput extends Component {

  constructor(){
    super()
    this.state = {
      bandName: ''
    }
  }
  

  handleOnChange(event) {
    this.setState({
      bandName: event.target.value,
    });
  }

  handleOnSubmit(event) {
    event.preventDefault();
    this.props.addBand(this.state.bandName);
    this.setState({
      bandName: '',
    });
  }

  render() {
    return (
      <div>
        <form onSubmit={(event) => this.handleOnSubmit(event)}>
          <input
            type="text"
            value={this.state.bandName}
            onChange={(event) => this.handleOnChange(event)} />
          <input type="submit" />
        </form>
      </div>
    );
  }
};

export default BandInput;
```

## Containers

`BandsContainer.js`

```jsx
import React, { Component } from 'react'
import Bands from './Bands'
import BandInput from './BandInput';
import { connect } from 'react-redux'

class BandsContainer extends Component {
  render() {
    return (
      <div>
        <BandInput addBand={this.props.addBand}  />
        <Bands bands={this.props.bands} delete={this.props.deleteBand}/>
      </div>
    )
  }
}

const mapStateToProps = ({ bands }) => ({ bands })

const mapDispatchToProps = dispatch => ({
  addBand: name => dispatch({ type: "ADD_BAND", name }),
  deleteBand: id => dispatch({ type: "DELETE_BAND", id })
})

export default connect(mapStateToProps, mapDispatchToProps)(BandsContainer)
```

## reducers

`MangeBand.js`

```jsx
import uuid from 'uuid'

export default function manageBand(state = {bands : []}, action){
	switch (action.type) {
		case 'ADD_BAND':
			const band = {
				id: uuid(),
				name: action.name
			}
			return {bands: state.bands.concat(band)}

		case 'DELETE_BAND':
			return {bands: state.bands.filter(band=> band.id !== action.id)}

		default:
			return state;
	}
}
```

