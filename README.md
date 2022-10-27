## Usage

First, install `xc-sdk-react` as a dependency:

```shell
npm install --save xc-sdk-react
```

[Skip to Complete example](#Complete-Example)

### Loading and initializing CC package

1. Call initXC(your_site_id, your_api_key) in the ComponentDidMount for any component which you want to track its form.
   - initXC only loads and initialize our library, for adding trackers follow bellow steps
2. You can call initXC(your_site_id, your_api_key) in the index.js or app.js once.
3. Pass your site_id and api_key to initXC(your_site_id, your_api_key).
   - You can get your_site_id and your_api_key from app.crossclassify.com after creating a new app in your project.

```react
import { initXC } from "xc-sdk-react";

// Class Components
componentDidMount(){
   initXC(your_site_id_from_app.crossclassify.com, your_api_key_from_app.crossclassify.com);
};
```

```react
// Functional Components
useEffect(() => {
  initXC(your_site_id_from_app.crossclassify.com, your_api_key_from_app.crossclassify.com);
}, []);
```

### Track the form

4. Create your form.
5. Add property "name" to your <form> tag.
   - Note: Your form name must contains "signup" word, examples: "signup-form", "my-signup", ... .
   - Note: DO NOT USE SPACE OR DASH FOR "SIGNUP" (sign-up and sign up are not allowed)
6. Add custom-attribute="include-form-tracking" to the form which you want to track.

```
<form
  name="must-contain-signup"
  custom-attribute="include-form-tracking"
>
</form>
```

7. Add custom-attribute="include-content-tracking" to the input which you want to track its content.
   - NOTE: Your signup form must include an input with the name="email".
   - NOTE: You must add custom-attribute="include-content-tracking" for the email input
   - NOTE: Its ok to not including content of private fields like password, etc.

```
<input
  custom-attribute="include-content-tracking"
  type="text"
  onChange={handleChange}
/>
```

8. Add custom-attribute="form-submit" or type="submit" to the submit button.
9. Use onSubmit={submitHandler} for you button NOT onClick={clickHandler}.

```
<button custom-attribute="form-submit">submit</button>
```

### Complete Example

```
// Sample usage of the package
import { useState, useEffect } from "react";
import { initXC } from "xc-sdk-react";

const initialState = {
  name: "",
  family_name: "",
  username: "",
  email: "",
};

const Form = () => {
  const [state, setState] = useState(initialState);

   useEffect(() => {
    // get your_site_id and your_api_key from app.crossclassify.com, after creating an app in a project
    initXC(your_site_id, your_api_key);
   }, []);

  const handleChange = (event) => {
    let name = event.target.name;
    let value = event.target.value;
    setState((prevState) => ({
      ...prevState,
      [name]: value,
    }));
  };

  const handleSubmit = (event) => {
    event.preventDefault();
  };

  return (
    <div>
      // Your <form> name must contains "signup" word, examples: "signup-form", "my-signup", ... .
      // DO NOT USE SPACE OR DASH FOR "SIGNUP" (sign-up and sign up are not allowed)
      // Add custom-attribute="include-form-tracking" to the form which you want to track.
      <form
        name="must-contain-signup"
        onSubmit={handleSubmit}
        custom-attribute="include-form-tracking"
      >
        // Your signup form must include an input with the name="email".
        // You must add custom-attribute="include-content-tracking" for the email input
        // Also use custom-attribute="include-content-tracking" to track other field's content.
        // Its ok to not including content of private fields like password, etc.

        <label>Name: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="name"
          onChange={handleChange}
        />

        <div style={{ width: "100%" }}></div>

        <label>Family Name: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="family_name"
          onChange={handleChange}
        />

        <div style={{ width: "100%" }}></div>

        <label>Username: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="username"
          onChange={handleChange}
        />

        <div style={{ width: "100%" }}></div>

        <label>email: </label>

        <input
          custom-attribute="include-content-tracking"
          type="email"
          name="email"
          onChange={handleChange}
        />
        <div style={{ width: "100%" }}></div>

        // Add custom-attribute="form-submit" or type="submit" to the submit button.
        // Use onSubmit={submitHandler} for you button NOT onClick={clickHandler}.
        <button custom-attribute="form-submit">submit</button>
      </form>
    </div>
  );
};

export default Form;
```

### An example with Material UI (MUI) "Form"

- If you are not using pure HTML tags (form, input and button), consider adding the mentioned attributes to the rendered HTML tags like using "inputProps".
  Pay attention to the following example of Matrial UI (MUI) "TextField".

```
   //simple MUI "Form" tracking integration
  <Box
    sx={{
      "& .MuiTextField-root": {maxWidth: "100%" },
    }}
    name="signup-form" // Name attribute must contain "signup"
    component="form"
  >
     <TextField
      ///// Adding properties to the HTML input tag using inputProps
      inputProps={{
        "custom-attribute": "include-content-tracking",
        name: "email",
      }}
      /////
      type="text"
      value={email}
      onChange={handleChange}
      label="email"
    />
    <Button
    sx={{ m: 1 }}
    id="signup-btn"
    // onSubmit={onSignup}
    onSubmit={onSignup}
    variant="contained"
    size="large"
    type="submit"
    >
    Signup
    </Button>
  </Box>

```

Now go to dashboard and check if your signup submits are shown up in the "Registrations" menu.
Congratulations! your done.
