examine what this `<MovieCard />` component would look like with *hardcoded* data vs. dynamic *prop* data:

**Hardcoded:**

```jsx
class MovieCard extends React.Component {
  render() {
    return (
      <div className="movie-card">
        <img src="http://image.tmdb.org/t/p/w342/kqjL17yufvn9OVLyXYpvtyrFfak.jpg" alt="Mad Max: Fury Road" />
        <h2>Mad Max: Fury Road</h2>
        <small>Genres: Action, Adventure, Science Fiction, Thriller</small>
      </div>
    )
  }
}
```

###### Dynamic with Props:

```Jsx
// assuming we are rendering a MovieCard component with the following JSX:
const title = "Mad Max"
const posterURL = "http://image.tmdb.org/t/p/w342/kqjL17yufvn9OVLyXYpvtyrFfak.jpg"
const genresArr = ["Action", "Adventure", "Science Fiction", "Thriller"]
 
<MovieCard title={title} posterSrc={posterURL} genres={genresArr} />
 --------------------------------------------------------------------------------------   
    class MovieCard extends React.Component {
  render() {
    return (
      <div className="movie-card">
        <img src={this.props.posterSrc} alt={this.props.title} />
        <h2>{this.props.title}</h2>
        <small>{this.props.genres.join(', ')}</small>
      </div>
    )
  }
}
```

## Passing in props

To pass props to a component, you add them as attributes when you render them:

```jsx
const movieTitle = "Mad Max"
<MovieCard title={movieTitle} />
```

The value of a prop is passed in through JSX curly braces. As we read before, this value can be anything: a variable, inline values, functions, etc. If your value is a hardcoded string, you can pass it in through double quotes instead:

```jsx
<MovieCard title="Mad Max" />
```

## Default values for props

we can tell our `MovieCard` component to use a default prop **if the `poster` prop was not provided**. To do this, we add the `defaultProps` property to our `MovieCard` class:

```jsx
class MovieCard extends React.Component {
  render() {
    return (
      <div className="movie-card">
        <img src={this.props.posterSrc} alt={this.props.title} />
        <h2>{this.props.title}</h2>
        <small>{this.props.genres.join(', ')}</small>
      </div>
    )
  }
}
 
MovieCard.defaultProps = {
  posterSrc: 'http://i.imgur.com/bJw8ndW.png'
}
```

Now, whenever we omit the `posterSrc` prop, or if it's undefined, the `MovieCard` component will use this default prop instead. 

**Consider** **the** **following**:

Should the parent component of `MovieCard` be responsible for managing the assignment of a default movie poster source value? In our example, we think not. It makes more sense for the component that is responsible for rendering the movie information and poster to handle missing data.

