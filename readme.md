# Cypress React Login Button component test

This example provides a React Login Button component integrated with Auth0 for user login functionality. A Cypress test ensures the button renders and triggers login correctly.


## React LoginButton Component



```bash
import { useAuth0 } from "@auth0/auth0-react";
import { Button } from "@radix-ui/themes";

const LoginButton = () => {
  const { loginWithRedirect } = useAuth0();

  return <Button onClick={() => loginWithRedirect()}>Log In</Button>;
};

export default LoginButton;
```


## Cypress Component Test



```bash
import { mount } from "cypress/react";
import LoginButton from "../../src/components/LoginButton"; // adjust path
import { Auth0Context } from "@auth0/auth0-react";

describe("<LoginButton />", () => {
  it("renders and calls loginWithRedirect on click", () => {
    const loginWithRedirectSpy = cy.stub().as("loginWithRedirectSpy");

    const auth0ContextValue = {
      isAuthenticated: false,
      loginWithRedirect: loginWithRedirectSpy,
      logout: cy.stub(),
      user: null,
    };

    mount(
      <Auth0Context.Provider value={auth0ContextValue as any}>
        <LoginButton />
      </Auth0Context.Provider>
    );

    // assert button renders
    cy.contains("Log In").should("exist");

    // click button
    cy.contains("Log In").click();

    // assert loginWithRedirect was called once
    cy.get("@loginWithRedirectSpy").should("have.been.calledOnce");
  });
});
```

![Screenshot of Logout Button Component](Screenshot%202025-09-27%20115349.png)

| Criteria                  | Justification                                                                                                                                                                                                                                              |
| :------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Isolation**             | The test isolates the `LoginButton` component by **mocking** its external dependency, the `Auth0Context`. This ensures the test only verifies the button’s behavior without depending on a real Auth0 login flow.                                          |
| **Mocking Quality**       | It uses a **Cypress stub (`cy.stub().as("loginWithRedirectSpy")`)** to replace the real `loginWithRedirect` function provided by `useAuth0`. This is the correct approach for tracking and verifying calls to external authentication logic.               |
| **Coverage**              | The test covers the main responsibilities of the component: 1) **Rendering** the button (`cy.contains("Log In").should("exist")`) and 2) **Calling the correct function** when clicked (`cy.get("@loginWithRedirectSpy").should("have.been.calledOnce")`). |
| **Readability**           | The test is clear and structured, with descriptive naming (`loginWithRedirectSpy`) and a consistent mock setup, making the test easy to follow.                                                                                                            |
| **Clarity of Assertions** | The assertion verifies that the `loginWithRedirect` function is called exactly once upon clicking, ensuring the component’s logic is correct without over-testing unrelated functionality.                                                                 |
