### `App`

1. App should pass a **callback** prop, `onChangeType`, to `<Filters />`. This callback needs to update `<App />`'s `state.filters.type`
2. `<Filters />` needs a **callback** prop, `onFindPetsClick`. When the `<Filters />` component calls `onFindPetsClick`, `<App />` should fetch a list of pets using `fetch()`.

- Assuming your app is up and running, you can make a fetch to this exact URL: `/api/pets` with an **optional query parameter** to get your data.
- Use `App`'s state.filters to control/update this parameter
- If the `type` is `'all'`, send a request to `/api/pets`
- If the `type` is `'cat'`, send a request to `/api/pets?type=cat`. Do the same thing for `dog` and `micropig`.
- The pet data received will include information on individual pets and their adoption status.

4. Set `<App/>`'s `state.pets` with the results of your fetch request so you can pass the pet data down as props to `<PetBrowser />`

- **Even though we're using `fetch` here, its responses have been mocked in order to make the tests work properly. That means it's important to use the \*exact\* URLs as described above, or your tests will fail!**

5. Finally, App should pass a **callback** prop, `onAdoptPet`, to `<PetBrowser />`. This callback should take in an id for a pet, find the matching pet in `state.pets` and set the `isAdopted` property to `true`.

```jsx
import React from 'react';

import Filters from './Filters';
import PetBrowser from './PetBrowser';

class App extends React.Component {
	constructor() {
		super();

		this.state = {
			pets    : [],
			filters : {
				type : 'all',
			},
			error   : null,
		};
	}
	fetchPets = () => {
    const petType = this.state.filters.type;
    const pets = []
    let url;
		if (petType != 'all') {
			url = `/api/pets?type=${petType}`;
			fetch(url).then((r) => r.json()).then((data) => {
        data.filter(pet=> pet.type === petType? pets.push(pet):null)
      });
      this.setState({
        pets : pets,
      });
		} else {
      url = '/api/pets'
			fetch(url).then((r) => r.json()).then((data) => {
				this.setState({
					pets : data,
				});
			});
		}
  };
  
	ChangeType = (event) => {
		event.persist();
		this.setState({
			filters : {
				...this.state.filters,
				type : event.target.value,
			},
		});
	};

	onAdoptPet = petId => {
    const pets = this.state.pets.map(p => {
      return p.id === petId ? { ...p, isAdopted: true } : p;
    });
    this.setState({ pets: pets });
  };

	render() {
		return (
			<div className="ui container">
				<header>
					<h1 className="ui dividing header">React Animal Shelter</h1>
				</header>
				<div className="ui container">
					<div className="ui grid">
						<div className="four wide column">
                            <!---app passses call back PROP to Filters component using method change type,-->
								<Filters onChangeType={(event) => this.ChangeType(event)} onFindPetsClick={this.fetchPets} />
                                                            <!--App passes fetchPets method to onFindPetsClick Prop-->
						</div>
						<div className="twelve wide column">
                            <!--using the result fetchPets method we pass in to pets pet a callback for Pet Browser component-->
							<PetBrowser pets={this.state.pets} onAdoptPet={this.onAdoptPet} />
<!--Above we pass in the onAdoptPet method to the onAdoptPet prop for pet Browser Component-->                           
						</div>
					</div>
				</div>
			</div>
		);
	}
}

export default App;
```



### `Filters`

1. Should receive an `onChangeType` callback prop. This callback prop gets called whenever the value of the `<select>` element changes with the **value** of the `<select>`
2. Should receive an `onFindPetsClick` callback prop. This callback prop gets called when the users clicks the 'Find pets' button.

```jsx
import React from 'react';

class Filters extends React.Component {
	constructor() {
		super();
		this.state = {
			pets : [],
		};
	}

	render() {
		return (
			<div className="ui form">
				<h3>Animal type</h3>
				<div className="field">
                    <
                    <!--onChangType is inherited through parent component, App, and filters creates onChangeType prop when clicked-->
					<select name="type" id="type" onChange={this.props.onChangeType}>
						<option value="all">All</option>
						<option value="cat">Cats</option>
						<option value="dog">Dogs</option>
						<option value="micropig">Micropigs</option>
					</select>
				</div>

				<div className="field">
                        <!--onClick listner creates prop for filters called onFindPetsClick-->
					<button className="ui secondary button" onClick={this.props.onFindPetsClick}>
						Find pets
					</button>
				</div>
			</div>
		);
	}
}

export default Filters;
```



### `PetBrowser`

1. Should receive a `pets` prop. This is an array of pets that the component uses to render `<Pet />` components. App should determine which pets to pass down as props. App should be responsible for filtering this list based on the types of pets the user wants to see.
2. Should receive an `onAdoptPet` prop. This callback prop gets passed to its `<Pet />` children components.

```jsx
import React from 'react'

import Pet from './Pet'


class PetBrowser extends React.Component {
  render() {
  return (
    <div className="ui cards">
          <!--using the result from App component method fetchPets, we create a prop that will create pets from the passed in values using Pet component-->
    {this.props.pets.map((pet)=>{
              <!--we create a prop onAdoptPet for PetBrowser component to be called back from Pet component-->
    return <Pet pet={pet} onAdoptPet={this.props.onAdoptPet} key={pet.id} />
    })}
  </div>
  )
  }
}

export default PetBrowser

```



### `Pet`

1. Should receive a `pet` prop. Use the attributes in this data to render the pet card correctly. It should show the pet's `name`, `type`, `age` and `weight`. Based on the pet's `gender`, the component also needs to contain either a male (`♂`) or female (`♀`) symbol.
2. Each `pet` *may or may not* have an `isAdopted` property set to `true`. Using this property, render the correct button in the pet's card; if the pet is adopted, show the disabled button. Otherwise, show the primary button to adopt the pet.
3. Should receive an `onAdoptPet` callback prop. This callback prop gets called with the pet's `id` when the user clicks the adopt pet button — *not* when they click the disabled button!

```jsx
import React from 'react'
//We can use this.props to generate pet props, id, name, gender, isAdopted, weight and type, the value of these props is passed in by the pet prop call back in PetBrowser component, using the the array from our fetchPet method wich returns an array of pets then map through that array using filters props to determine wich pets to pass in to PetBrowser prop callback pet to Pet component.
class Pet extends React.Component {
  render() {
    return (
      <div className="card">
        <div className="content">
          <a className="header">
            {this.props.pet.gender === 'female' ? '♀' : '♂' }
            {this.props.pet.name}
          </a>
          <div className="meta">
    <span className="date">{this.props.pet.type}</span>
          </div>
          <div className="description">
    <p>{this.props.pet.age}</p>
            <p>Weight: {this.props.pet.weight}</p>
          </div>
        </div>
        <div className="extra content">
<!--here we have a ternary op. that will render a button based on wether the isAdopted prop is true or false-->            
        {this.props.pet.isAdopted ? (<button className="ui disabled button">Already adopted</button>):
        (<button className="ui primary button" onClick={() => this.props.onAdoptPet(this.props.pet.id)}>Adopt pet</button>)}
<!--above we use the onAdoptPet method inherited from App component and pass in the prop pet key.id-->
        </div>
      </div>
      
    )
  }
}

export default Pet

```

