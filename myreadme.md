## Submitting a Controlled Form

We add the `onSubmit` event listener to our `form`
element:

```jsx
// src/components/Form.js
return (
  <form onSubmit={handleSubmit}>
    <input type="text" onChange={handleFirstNameChange} value={firstName} />
    <input type="text" onChange={handleLastNameChange} value={lastName} />
    <button type="submit">Submit</button>
  </form>
);
```
 To submit the form we have to callback the `handleSubmit` function so lets write it down


```jsx
function handleSubmit(event) {
  event.preventDefault();
  const formData = {
    firstName: firstName,
    lastName: lastName,
  };
  props.sendFormDataSomewhere(formData); // where info is sent
  setFirstName(""); // same as `event.target.reset()` used to reset output when done submitting
  setLastName("");
}
```

`event.preventDefault` => is used to prevent a refresh when we submit form.

## Validating Inputs

let's say we
want to require that a user enter some data into our form fields before they
can submit the form successfully.

We do this is the `handleSubmit` function ,an error message will display if field is empty.

```jsx
// add state for holding error messages
const [errors, setErrors] = useState([]);

function handleSubmit(event) {
  event.preventDefault();
  // first name is required
  if (firstName.length > 0) {
    const formData = { firstName: firstName, lastName: lastName };
    const dataArray = [...submittedData, formData];
    setSubmittedData(dataArray);
    setFirstName("");
    setLastName("");
    setErrors([]);
  } else {
    setErrors(["First name is required!"]);
  }
}
```

Then, we can display an error message to our user in the JSX:

```jsx
return (
  <div>
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleFirstNameChange} value={firstName} />
      <input type="text" onChange={handleLastNameChange} value={lastName} />
      <button type="submit">Submit</button>
    </form>
    {/* conditionally render error messages */}
    {errors.length > 0
      ? errors.map((error, index) => (
          <p key={index} style={{ color: "red" }}>
            {error}
          </p>
        ))
      : null}
    <h3>Submissions</h3>
    {listOfSubmissions}
  </div>
);
```