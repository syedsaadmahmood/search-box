# DC Search Box Component

## Description
This project is a search box component built using Vue.js. The component searches for programming tools from the provided API and displays the results in a user-friendly interface.

## Thinking Process

### Understanding Requirements
- Develop a search box component using Vue.js.
- Fetch data from the provided API.
- Split the code into logical reusable components.
- Use at least three universal components: SearchBar, TagList, and ResultItem.
- Styling could be done using any CSS framework or preprocessor, with a preference for Tailwind CSS and Sass.

### Suggestions
Unit Testing: Adding unit tests for each component using a testing framework like Jest to ensure the components work as expected and to catch potential issues early.
State management: The test doesn't include state management using Vuex or Pinia, which are crucial for evaluating a candidate's ability to handle the store in large-scale projects. Including these technologies would provide a better assessment of their skills in managing complex state and ensuring efficient data flow throughout the application.

## Components
- **SearchBar**: A component for the search input field.
- **TagList**: A component to display tags.
- **ResultItem**: A component to display individual search results.
- **Loader**: A component to display loader.

## Features
- Search functionality to filter programming tools.
- Display search results in a structured format.
- Responsive design using Tailwind CSS.

## Installation and Starting the project
1. Install the dependencies:
   npm install
2. Run the project:
   npm run dev