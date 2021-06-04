## .Top

`App.js`

```jsx
import React, { Component } from 'react';
import RestaurantsContainer from './containers/RestaurantsContainer';

class App extends Component {
  render() {
    return (
      <div className="App">
        <RestaurantsContainer />
      </div>
    );
  }
};

export default App;
```

`index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import manageRestaurant from './reducers/manageRestaurant'

import { Provider } from 'react-redux';
import { createStore } from 'redux';

const store = createStore(manageRestaurant)


ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
document.getElementById('root')
)
```

## Components

**`/restaurants`**

`Restraunt.js`

```jsx
import React, { Component } from 'react';
import ReviewsContainer from '../../containers/ReviewsContainer';

class Restaurant extends Component {


  render() {
    const { restaurant } = this.props;
    return (
      <div>
        <li>
          {restaurant.text}
          <button onClick={() =>  this.props.deleteRestaurant(this.props.restaurant.id)}> X </button>
          <ReviewsContainer restaurant={restaurant}/>
        </li>
      </div>
    );
  }
};

export default Restaurant;
```

`RestrauntInput.js`

```jsx
import React, { Component } from 'react';

class RestaurantInput extends Component {
  state = {
    text: ''
  }
  
  handleOnChange = event => {
    this.setState({
      text: event.target.value,
    })
  }
  
  handleOnSubmit = event => {
    event.preventDefault()
    this.props.addRestaurant(this.state.text)
    this.setState({text: '' })
  }

  render() {
    return (
      <div>
        <form onSubmit={event => this.handleOnSubmit(event)}>
          <label>Restaurant Name: </label>
          <input type="text" value={this.state.text} onChange={(event) => this.handleOnChange(event)}  />
          <input type="submit" />
        </form>
      </div>
    );
  }
};

export default RestaurantInput;
```

`Restraunts.js`

```jsx
import React, { Component } from 'react';
import Restaurant from './Restaurant'

class Restaurants extends Component {
  render() {
    return(
      <ul>
          {this.props.restaurants.map(restaurant => {
         return <Restaurant key={restaurant.id} restaurant={restaurant} deleteRestaurant={this.props.deleteRestaurant}/>
        })}
      </ul>
    );
  }
};

export default Restaurants;
```

****

**`/Reviews`**

`Review.js`

```jsx
import React, { Component } from 'react';

class Review extends Component {

  render() {
    return (
      <div>
        <li>
          {this.props.review.text}
        </li>
        <button onClick={() => this.props.deleteReview(this.props.review.id)}> X </button>
      </div>
    );
  }

};

export default Review;

```

`ReviewInput.js`

```jsx
import React, {Component} from 'react';

class ReviewInput extends Component {
	state = {
		text : '',
	};

	handleOnChange = (event) => {
		this.setState({
			text : event.target.value,
		});
	};

	handleOnSubmit = (event) => {
		event.preventDefault();
		this.props.addReview({text: this.state.text, restaurantId: this.props.restaurantId});
		this.setState({
			text : '',
		});
	};

	render() {
		return (
			<div>
				<form type="submit" onSubmit={this.handleOnSubmit}>
					<input type="text" onChange={this.handleOnChange} value={this.state.content} />
					<input type="submit" />
				</form>
			</div>
		);
	}
}

export default ReviewInput;
```

`Reviews.js`

```jsx
import React, { Component } from 'react';
import Review from './Review';

class Reviews extends Component {
  render() {

    const { reviews, restaurantId, deleteReview } = this.props;
    const associatedReviews = reviews.filter(review => review.restaurantId === restaurantId);

    const reviewList = associatedReviews.map((review, index) => {
      return <Review key={index} review={review} deleteReview={deleteReview} />
    })

    return (
      <div>
        <ul>
          {reviewList}
        </ul>
      </div>
    );
  }
};

export default Reviews;
```

## Containers

`RestrauntsContainer.js`

```jsx
import React, { Component } from 'react';
import RestaurantInput from '../components/restaurants/RestaurantInput';
import Restaurants from '../components/restaurants/Restaurants';
import { connect } from 'react-redux'

class RestaurantsContainer extends Component {
  render() {
    return (
      <div>
        <RestaurantInput addRestaurant={this.props.addRestaurant} />
        <Restaurants 
        restaurants={this.props.restaurants} 
        deleteRestaurant={this.props.deleteRestaurant} />
      </div>
    )
  }
}

const mapStateToProps = (state) => ({restaurants: state.restaurants})

const mapToDispatchProps = dispatch => ({
  addRestaurant: text => dispatch({type: "ADD_RESTAURANT", text}),
  deleteRestaurant: id => dispatch({type: "DELETE_RESTAURANT", id})
})

export default connect(mapStateToProps, mapToDispatchProps)(RestaurantsContainer);

```

`ReviewsContainer.js`

```jsx
import React, { Component } from 'react'
import ReviewInput from '../components/reviews/ReviewInput'
import Reviews from '../components/reviews/Reviews'
import { connect } from 'react-redux'

class ReviewsContainer extends Component {

  render() {
    return (
      <div>
        <ReviewInput
          addReview={this.props.addReview}
          restaurantId={this.props.restaurant.id}
        />
        <Reviews
          reviews={this.props.reviews}
          restaurantId={this.props.restaurant.id}
          deleteReview={this.props.deleteReview}
        />
      </div>
    )
  }
}

const mapStateToProps = ({ reviews }) => {
  return { reviews }
}

const mapDispatchToProps = dispatch => ({
  addReview: review => dispatch({ type: 'ADD_REVIEW', review }),
  deleteReview: id => dispatch({ type: 'DELETE_REVIEW', id })
})

export default connect(mapStateToProps, mapDispatchToProps)(ReviewsContainer)
```

## reducers

`ManageRestaurants.js`

```jsx
import cuid from 'cuid';
export const cuidFn = cuid;

export default function manageRestaurants(
	state = {
		restaurants : [],
		reviews     : [],
	},
	action,
){
	switch (action.type) {
		case 'ADD_RESTAURANT':
			const restaurant = {text: action.text, id: cuidFn()};
			return {
				...state,
				restaurants : [...state.restaurants, restaurant],
			};

		case 'DELETE_RESTAURANT':
			const restaurants = state.restaurants.filter((restaurant) => restaurant.id !== action.id);
			return {...state, restaurants};

		case 'ADD_REVIEW':
			const review = {text: action.review.text, restaurantId: action.review.restaurantId, id: cuidFn()};
			return {
				...state,
				reviews : [...state.reviews, review],
			};

		case 'DELETE_REVIEW':
			const reviews = state.reviews.filter((review) => review.id !== action.id);
			return {...state, reviews};

		default:
			return state;
	}
}
```



