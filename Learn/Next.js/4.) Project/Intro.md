# This is the Events Project.

Setup helpers/utils.js

```js
export const getAllEvents = async () => {
  const res = await fetch(
    'https://nextjs-course-206de-default-rtdb.firebaseio.com/events.json',
  );
  const data = await res.json();

  const events = [];
  for (const key in data) {
    events.push({
      id: key,
      ...data[key],
    });
  }
  return events;
};

export const getFeaturedEvents = async () => {
  const allEvents = await getAllEvents();

  return allEvents.filter(event => event.isFeatured);
};

export const getEventById = async id => {
  const allEvents = await getAllEvents()
  return allEvents.find(event => event.id === id);
};

```

## `index.js`

```js
import EventList from '../components/events/event-list'
import{getFeaturedEvents} from '../helpers/utils'

const HomePage = (props) => {
  return (
    <div>
      <EventList items={props.events}/>
    </div>
  )
}

export const getStaticProps = async () => {
  const featuredEvents = await getFeaturedEvents()

  return {props: {events: featuredEvents}}
}

export default HomePage
```

## `pages/events/index.js`

```js
import { Fragment, useState, useEffect } from 'react';
import {useRouter} from 'next/router'
import EventList from '../../components/events/event-list';
import EventSearch from '../../components/events/events-search';
import {getAllEvents} from '../../helpers/utils'

const eventsPage = (props) => {
  const [ events, setEvents ] = useState(props.events);
  const router = useRouter()

  const findEvents = (year, month) => {
    const fullPath = `/events/${year}/${month}`;

    router.push(fullPath);
  };

  return (
    <Fragment>
      <h1>Event Page</h1>
      <EventSearch onSearch={findEvents} />
      <EventList items={events} />
    </Fragment>
  );
};

export const getStaticProps = async () => {
  const events = await getAllEvents()
  
  return {props: {events: events}}
};

export default eventsPage;
```

## `events/[eventId].js`

```js
import { Fragment } from 'react';
import { getEventById, getAllEvents } from '../../helpers/utils';
import EventSummary from '../../components/event-detail/event-summary';
import EventLogistics from '../../components/event-detail/event-logistics';
import EventContent from '../../components/event-detail/event-content';
import ErrorAlert from '../../components/error-alert/error-alert';

const event = props => {
  const event = props.event;
  if (!event) {
    return (
      <ErrorAlert>
        <h1>No Event Found! </h1>
      </ErrorAlert>
    );
  }
  return (
    <Fragment>
      <EventSummary title={event.title} />
      <EventLogistics
        date={event.date}
        address={event.location}
        image={event.image}
        imageAlt={event.imageAlt}
      />
      <EventContent>
        <p>{event.description}</p>
      </EventContent>
    </Fragment>
  );
};

export const getStaticProps = async context => {
  const id = context.params.eventId;
  const event = await getEventById(id);

  return {
    props: { event: event },
  };
};

export const getStaticPaths = async () => {
  const events = await getAllEvents()

  const paths = events.map(event =>({params: {eventId: event.id}}))

  return {
    paths: paths,
    fallback: false,
  };
};

export default event;
```

## `pages/events/[...slug]`

Here we use SeverSide rendering, but later will optimize this to be client Side Rendering.

```jsx
const slug = (props) => {
 
  if (props.hasError) {
    return (
      <Fragment>
        <ErrorAlert>
          <p>invalid filter</p>
        </ErrorAlert>
        <div className={'center'}>
          <Button link={'/events'}>Show all Events</Button>
        </div>
      </Fragment>
    );
  }

  const filteredEvents = props.events;

  if (!filteredEvents || filteredEvents.length === 0) {
    return (
      <Fragment>
        <ErrorAlert>
          <p>No Events found for filter</p>
        </ErrorAlert>
        <div className={'center'}>
          <Button link={'/events'}>Show all Events</Button>
        </div>
      </Fragment>
    );
  }

  const date = new Date(props.date.year, props.date.month);

  return (
    <Fragment>
      <ResultsTitle date={date} />
      <EventList items={filteredEvents} />
    </Fragment>
  );
};

export const getServerSideProps = async (context) => {
  const { params } = context;
  const filterData = params.slug;

  const fliteredYear = filterData[0];
  const fliteredMonth = filterData[1];

  const numYear = +fliteredYear;
  const numMonth = +fliteredMonth;

  if (
    isNaN(numYear) ||
    isNaN(numMonth) ||
    numYear > 2030 ||
    numYear < 2021 ||
    numMonth < 1 ||
    numMonth > 12
  ) {
    return {
      props: { hasError: true },
    };
  }

  const filteredEvents = await getFilteredEvents({
    year: numYear,
    month: numMonth,
  });

  return {
    props: {
      events: filteredEvents,
      date: {
        year: numYear,
        month: numMonth,
      },
    },
  };
};

export default slug;
```

