#react #state
State immutability in React refers to the practice of **not modifying the existing state object directly but instead creating a new copy** of the state with the desired changes. This is important because React uses state immutability to detect changes and efficiently update the user interface.

Key point:
	1. React uses **shallow comparison** to determine if the state has changed. If the reference to the state object changes, React knows an update is required.
	2. Do not mutate state directly , eg. `state.value = 10` 
	3.  Use setter, update function from [[useState]] hook.

### Safe way to update state: (always create a new copy) 
https://state-updates.vercel.app/