React is an open-source framework for building reusable UI components and apps. It is used by thousands of developers around the world to create complex and multifaceted applications. In this article, we will discuss unit testing in React and how to implement it using the Jest and Enzyme frameworks.

Unit testing:-

It is an essential and integral part of the software development process that helps us ensure the product’s stability.
It is the level of testing at which every component of the software is tested.
Testing is required for the effective performance of a software application or product.

Significance of unit testing :-

It helps us improve the quality of code.
In a world of changing requirements, it helps us identify future breaks.

DISADVANTAGES #
You need to write more code, as well as debug and maintain.
Non-critical test failures might cause the app to be rejected in terms of continuous integration.

Introduction To Jest #
Jest is a delightful JavaScript testing framework with a focus on simplicity. It can be installed with npm or Yarn. Jest fits into a broader category of utilities known as test runners. It works great for React applications, but it also works great outside of React applications.

Enzyme is a library that is used to test React applications. It’s designed to test components, and it makes it possible to write assertions that simulate actions that confirm whether the UI is working correctly.

New Version Jest

it or test :- You would pass a function to this method, and the test runner would execute that function as a block of tests.

describe :- his optional method is for grouping any number of it or test statements.

expect :- This is the condition that the test needs to pass. It compares the received parameter to the matcher.

shallow:- This renders only the individual components that we are testing. It does not render child components. This enables us to test components in isolation.

it("renders without crashing", () => {
shallow(<App />);
});

it("renders Account header", () => {
const wrapper = shallow(<App />);
const welcome = <h1>Display Active Users Account Details</h1>;
expect(wrapper.contains(welcome)).toEqual(true);
});

Old Version (using builder)

Given — This is where you define the state of your application up to this point. You provide any data or context. Usually, this involves setting up mocks or stubbing methods.

When — This is the part of your application you are testing. Usually, this is a method call.

Then — This is where you verify a particular behaviour or outcome. What did you expect to happen?

File.tsx

import React from "react";

import {
Box,
Input,
InputLabel,
} from "@material-ui/core";

// Customizable Area Start

import FileController, {
Props,
configJSON,
} from "./FileController";

export default class File extends FileController {
constructor(props: Props) {
super(props);
}

render() {
return (

            <Box sx={webStyle.inputStyle}>
              <InputLabel id="service-shop-name">
                This is the reviced value:{this.state.txtSavedValue}
              </InputLabel>
              <Input
                data-test-id={"txtInput"}
                value={this.state.txtInputValue}
                onChange={(e) => this.setInputValue(e.target.value)}
              />
            </Box>
            <Box
              data-test-id="btnAddExample"
              onClick={() => this.doButtonPressed()}
            >
            </Box>
    );

}
}

File-scenario.feature

    Scenario: User navigates to File
        Given I am a User loading File
        When I navigate to the File
        Then File will load with out errors
        And I can enter text with out errors
        And I can select the button with with out errors
        And I can leave the screen with out errors

File.steps.tsx

import { defineFeature, loadFeature } from "jest-cucumber";
import { shallow, ShallowWrapper } from "enzyme";
import React from "react";
import File from "./File.tsx";
const navigation = require("react-navigation");

const screenProps = {
navigation: navigation,
id: "File",
};

const feature = loadFeature(
"./**tests**/features/File-scenario.web.feature"
);

defineFeature(feature, (test) => {

test("User navigates to File", ({ given, when, then }) => {
let exampleBlockA: ShallowWrapper;
let instance: File;

    given("I am a User loading File", () => {
      exampleBlockA = shallow(<File {...screenProps} />);
    });

    when("I navigate to the File", () => {
      instance = exampleBlockA.instance() as File;
    });

    then("File will load with out errors", () => {
      expect(exampleBlockA).toBeTruthy();
    });

    then("I can enter text with out errors", () => {
      let textInputComponent = exampleBlockA.findWhere(
        (node) => node.prop("data-test-id") === "txtInput"
      );
      const event = {
        preventDefault() {},
        target: { value: "hello@aol.com" },
      };
      textInputComponent.simulate("change", event);
    });

    then("I can select the button with with out errors", () => {
      let buttonComponent = exampleBlockA.findWhere(
        (node) => node.prop("data-test-id") === "btnAddExample"
      );
      buttonComponent.simulate("press");
      expect(exampleBlockA).toBeTruthy();
    });

    then("I can leave the screen with out errors", () => {
      instance.componentWillUnmount();
      expect(exampleBlockA).toBeTruthy();
    });

});
});
