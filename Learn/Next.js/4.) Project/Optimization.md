Optimizing [...slug] route to use CSF, CSR for better optimization.

```jsx
import { Fragment, useEffect, useState } from 'react';
import { useRouter } from 'next/router';
import EventList from '../../components/events/event-list';
import ResultsTitle from '../../components/results-title/results-title';
import Button from '../../components/ui/button';
import ErrorAlert from '../../components/error-alert/error-alert';
import useSwr from 'swr';

const slug = () => {
	const router = useRouter();
	const filterData = router.query.slug;
	const [events, setEvents] = useState();

	const { data, error } = useSwr(
		'https://nextjs-course-206de-default-rtdb.firebaseio.com/events.json',
	);

	useEffect(
		() => {
			if (data) {
				const events = [];
				for (const key in data) {
					events.push({
						id: key,
						...data[key],
					});
				}
				setEvents(events);
				return events;
			}
		},
		[data],
	);

	if (!events) {
		return <p className='center'>Loading...</p>;
	}

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
		numMonth > 12 ||
		error
	) {
		return (
			<Fragment>
				<ErrorAlert>
					<p>Invalid filter Please adjust your values!</p>
				</ErrorAlert>
				<div className={'center'}>
					<Button link={'/events'}>Show all Events</Button>
				</div>
			</Fragment>
		);
	}

	const filteredEvents = events.filter((event) => {
		const eventDate = new Date(event.date);
		return (
			eventDate.getFullYear() === numYear &&
			eventDate.getMonth() === numMonth - 1
		);
	});

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

	const date = new Date(numYear, numMonth);

	return (
		<Fragment>
			<ResultsTitle date={date} />
			<EventList items={filteredEvents} />
		</Fragment>
	);
};

export default slug;
```

