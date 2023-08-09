# CrossClassify React SDK
The CrossClassify SDK for react apps, which helps to collect metadata of users activity like page navigation and form interactions.

### **Prerequisites**
-   node 16 (or later)

## **SDK Integration Guide**

First, install `xc-sdk-react` as a dependency:

```shell
npm install --save xc-sdk-react
```

follow the steps below to integrate with your app or [skip to a complete example](#Complete-Example)

### Step 1: Loading and initializing XC package

1. Setup the page view tracker using initializer in one of these two ways:
    - Option A: Call `initXC(your_site_id, your_api_key)` in the `index.js` or `app.js` once. (Recommended)
    - Option B: Call `initXC(your_site_id, your_api_key)` in the `ComponentDidMount` in any component that you want to track its form.

2. Pass your `site_id` and `api_key` to `initXC(your_site_id, your_api_key)` function.
  - Get `site_id` and `api_key` from [app.crossclassify.com](app.crossclassify.com) by creating a new web app in a project.


```javascript
import { initXC } from "xc-sdk-react";

// Class Components
componentDidMount(){
   initXC(your_site_id_from_app.crossclassify.com, your_api_key_from_app.crossclassify.com);
};
```

```javascript
import { initXC } from "xc-sdk-react";

// Functional Components
useEffect(() => {
  initXC(your_site_id_from_app.crossclassify.com, your_api_key_from_app.crossclassify.com);
}, []);
```

### Step 2: Track the form
For each page that contains a form (e.g. signup, login) do the following instructions.
  #### **Step 2.1:** Specify the form

  1. Add property "name" to your form tag.
      - Account Opening Service:
        - Form `name` must contains `"signup"` substring, examples: "signup-form", "my-signup", etc.
      - Account Takeover Service:
        - Form `name` must contains `"login"` substring, examples: "login-form", "my-login", etc.

  2. Add custom-attribute="include-form-tracking" to the form which you want to track.

      ```html
      <form
        name="must-contain-signup-or-login-substring"
        custom-attribute="include-form-tracking"
      >
      </form>
      ```

  #### **Step 2.2:** Specify the form fields
  - Add custom-attribute="include-content-tracking" to the input which you want to track its content.
  
    > **Important:** 
    > Signup (or login) form must include an input with the `name="email"`.

    > **Important:**
    > Must set `custom-attribute="include-content-tracking"` for email input.

    > **Note:** 
    > Sending field contents increases the accuracy of the CrossClassify fraud detection algorithm. Hence, content tracking is HIGHLY RECOMMENDED on all non-confidential fields.

    ```html
    <input
      name="email"
      custom-attribute="include-content-tracking"
      type="text"
      onChange={handleChange}
    />
    ```

  #### **Step 2.3:** Specify submit button 
  - Add custom-attribute="form-submit" or type="submit" to the submit button.

    > **Important:**
    > Use onSubmit={submitHandler} for you button NOT onClick={clickHandler}.

    ```html
    <button custom-attribute="form-submit">submit</button>
    ```

### Complete Example

#### **Example 1:**
```javascript
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
      // 1- Your <form> name attribute must contain "signup" or "login" substring, examples: "signup-form", "my-login", ... .
      // 2- Add custom-attribute="include-form-tracking" to the form tag which you want to track.
      <form
        name="must-contain-signup"
        onSubmit={handleSubmit}
        custom-attribute="include-form-tracking"
      >
        // 1- Signup form must include an input with the name="email".
        // 2- Must add custom-attribute="include-content-tracking" for the email input
        // note: Sending field contents increases the accuracy of the CrossClassify fraud detection algorithm. Hence, content tracking is HIGHLY RECOMMENDED on all non-confidential fields.

        <label>Name: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="name"
          onChange={handleChange}
        />

        <label>Family Name: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="family_name"
          onChange={handleChange}
        />

        <label>Username: </label>
        <input
          custom-attribute="include-content-tracking"
          type="text"
          name="username"
          onChange={handleChange}
        />

        <label>email: </label>

        <input
          custom-attribute="include-content-tracking"
          type="email"
          name="email"
          onChange={handleChange}
        />

        // Add custom-attribute="form-submit" or type="submit" to the submit button.
        // Use onSubmit={submitHandler} for you button NOT onClick={clickHandler}.
        <button custom-attribute="form-submit">submit</button>
      </form>
    </div>
  );
};

export default Form;
```

#### **Example 2:** Material UI (MUI) "Form"

- If you are not using pure HTML tags (form, input and button), consider adding the mentioned attributes to the `rendered HTML tags`. like using "inputProps".
  Pay attention to the following example of Matrial UI (MUI) "TextField".

```javascript
   //simple MUI "Form" tracking integration
  <Box

    // Name attribute must contain "signup" or "login" substring
    name="signup-form" 
    component="form"
  >
     <TextField
      // Adding properties to the HTML <input> tag using inputProps
      inputProps={{
        "custom-attribute": "include-content-tracking",
        name: "email",
      }}
      type="text"
      value={email}
      onChange={handleChange}
      label="email"
    />
    <Button
      id="signup-btn"
      // must use onSubmit
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
Congratulations! You're done!

## Steps of Troubleshoot:
#### Attribute:
  - Make sure all mentioned attributes in the [form integration section](#step-2-track-the-form) assigned to the raw HTML tags using browser's inspect dev tools.
    - Some times libraries like MUI does assign attributes to the parent div tag! (which mentioned to use `inputProps`)

#### Network:
  - Make sure that data transfers to CrossClassify with status 200 OK, on both page navigation and form submission.
    - Sometimes api-key missed to pass to `initXC` function and raised for 403!
    - Make sure the form submit signal triggered when clicking on the submit button.
    - Sometimes, form submit event are not fired by clicking on the form submit button.As mentioned before, use `onSubmit` attribute.

