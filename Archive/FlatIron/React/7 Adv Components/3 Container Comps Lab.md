```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import LatestMovieReviewsContainer from './components/LatestMovieReviewsContainer';
import SearchableMovieReviewsContainer from './components/SearchableMovieReviewsContainer';

ReactDOM.render(
  <div className="app">
      <SearchableMovieReviewsContainer />
    <LatestMovieReviewsContainer />
  </div>,
  document.getElementById('root')
);
```

```jsx
import React, { Component } from 'react';
import 'isomorphic-fetch';
import MovieReviews from './MovieReviews'

const NYT_API_KEY = '6x9NpOeL4UlzFsFJAK7b4w08GM1AfXmM';
const URL = 'https://api.nytimes.com/svc/movies/v2/reviews/all.json?'
            + `api-key=${NYT_API_KEY}`;

export default class LatestMovieReviewsContainer extends Component {
  state = {
    reviews: []
  }

  componentDidMount(){
    fetch(URL)
    .then((r)=> r.json())
    .then((data) => this.setState({
      reviews: data.results
    }))
  }

  render(){
    return(
        <div className="latest-movie-reviews">
        {<MovieReviews reviews={this.state.reviews} />}</div>      
    )
  }
}
```

```jsx
import React, {Component} from 'react';
import 'isomorphic-fetch';
import MovieReviews from './MovieReviews';


export default class SearchableMovieReviewsContainer extends Component {
	constructor() {
		super();
		this.state = {
			searchTerm : '',
			reviews    : [],
		};
  }

	fetchReviews = (term) => {
    console.log(term)
		fetch(`https://api.nytimes.com/svc/movies/v2/reviews/search.json?query=${term}&api-key=6x9NpOeL4UlzFsFJAK7b4w08GM1AfXmM`).then((r) => r.json()).then((data) =>
			this.setState({
				reviews : data.results
			}, ()=> console.log(data))
		);
	};

	handleChange = (e) => {
    e.preventDefault()
		this.setState({searchTerm : e.target.value});
	};

	submitHandler = (e) => {
  e.preventDefault()
  this.fetchReviews(this.state.searchTerm)
  }

	render() {
		return (
			<div className="searchable-movie-reviews" onChange={this.handleChange}>
				<form onSubmit={this.submitHandler}>
					<input type="text" placeholder="search" value={this.state.searchTerm} onChange={(event) => this.handleChange(event)} />
					<input type="submit" value="search" />
				</form>
        <MovieReviews reviews={this.state.reviews} />
			</div>
		);
  }
  
  
}
```

```jsx
import React from 'react';

const MovieReviews = (props) => (
	<div className="review-list">
		{props.reviews.map((review, idx) => {
      return (
				<div className="review" key={idx}>
					<p>By:<em> {review.byline}</em></p>
					<h3>{review.display_title}</h3>
          <img src={review.multimedia.src} alt={review.display_title}/>
				</div>
			);
		})}
	</div>
);

export default MovieReviews;
```

