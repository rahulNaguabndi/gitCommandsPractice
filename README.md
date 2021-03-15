# Horizon - the Parallax UI

## Technology

- Based on [Create React App][create_react_app] with [Typescript][typescript_handbook]
- Dynamic web routing with [React Router][react_router]
- GraphQL with [Apollo Client][apollo_client]
- Includes the [Blueprint JS][blueprint_js] components
- Styles with [SASS][sass_css] and [CSS Modules][css_modules]
- Testing with [Jest][jest_js] and [React Testing Library][react_testing_library]
- Packages managed with [Yarn][yarn_pkg_mgr]

## Getting Started

### Installing Dependencies

```
yarn
```

### Starting the Local Web Server

Executing the start command will start a local web server on `localhost:3000`

```
yarn start
```

## Unit Tests

Unit tests are executed via Yarn and Jest. See [Jest's CLI options][jest_cli_options] for more command options.

### Execute All Unit Tests

```
yarn test
```

### Execute Test by File

```
yarn test <file-regex>
```

Example:

```
yarn test AppNameByRoute
```

### Execture Tests and Watch for Changes

Executes unit tests in watch-mode. Re-executes test when changes are detected.

```
yarn test-watch

```

### Test Coverage

```
yarn test-coverage
```

or

```
yarn --coverage
```

## Files and Structure

### Core

The `src/core` directory contains the web application frame that renders all views and core features of the main application frame. The `core` directory is reserved strictly for core infrastructure functionality.

### Common

The `src/common` directory contains common source that can be utilized by all sub-applications and components.

### Important Notes about Common

- Files in common **must not import or depend on files, utils or components outside of the `common` directory**
- Common code is intentded for high re-use and must be **well-documented with JS Docs, easy to use and thoroughly tested**

## Apps

The `src/apps` directory is where all sub-applications inside of the Horizon application live. Each sub-application must be contained within its own directory.

As an example, `src/apps/sandbox` is where the sandbox application lives inside of the `style-guide-with-sandbox` branch.

## Style Guide and Sandbox

The Horizon application has a rough style guide and sandbox with example applications, components and tests. The sandbox and style guide live in a special branch: `style-guide-with-sandbox`. To experiment with the sandbox or style guide, checkout the style guide/sandbox branch and start the app.

To checkout the style guide and sandbox

```
git origin/style-guide-with-sandbox && yarn start
```

### Keep the Sandbox Clean

The sandbox will be kept in a clean state to ensure developers will be able to quickly and easily experiment with code and features. If you would like to save your sandbox examples, create a branch from the style-guide/sandbox branch and edit away!

If you would like to save your sandbox examples, fork the Horizon repository, add the fork to your local Git remotes and push as many sandboxes, examples or experiments as you like to your Horizon fork. Forks for personal branches keeps the Horizon repository clean of excess branches while allowing your source to live securly on a remote.

## GraphQL and Generated Types

This application utilizes a GraphQL backend and uses Apollo Client for making GraphQL requests and generating GraphQL types. All GraphQL queries must have corresponding, generated types for query responses.

**Important:**

- GraphQL API Calls must use _generated_ types
- No manually created types for GraphQL query responses

### Generating GraphQL Types

GraphQL API types for configured APIs can be generated using the following command:

```bash
yarn generate:types
```

### Type Generation Workflow

1. write a GraphQL query
2. run the `generate:types` command
3. find the generated type under &lt;target-api&gt;/types"
4. import and use the generated type(s)
5. commit generated types

As an example, for the Carina API, the following query could be written:

```typescript
import { gql } from "@apollo/client";

const getAllRulesetDetailsQuery = gql`
  query AllRulesetDetails($name: String!, $version: String!) {
    ruleset(name: $name, version: $version) {
      id
      name
      version
      description
      kind
      created
      relationset {
        id
        name
        version
        created_by
        relations {
          id
          raw_relation
          created_by
        }
      }
    }
  }
`;

export default getAllRulesetDetailsQuery;
```

Next, the yarn command to generate types would be executed

```bash
yarn generate:types
```

A generated type for `AllRulesetDetails` should now exist in the Carina API folder under '/types' that looks like this:

```typescript
/* tslint:disable */
/* eslint-disable */
// @generated
// This file was automatically generated and should not be edited.

// ====================================================
// GraphQL query operation: AllRulesetDetails
// ====================================================

export interface AllRulesetDetails_ruleset_relationset_relations {
  __typename: "RelationForm";
  id: number;
  raw_relation: string;
  created_by: string | null;
}

export interface AllRulesetDetails_ruleset_relationset {
  __typename: "RelationsetForm";
  id: number;
  name: string;
  version: string;
  created_by: string | null;
  /**
   * the relations associated with this relationset
   */
  relations: AllRulesetDetails_ruleset_relationset_relations[];
}

export interface AllRulesetDetails_ruleset {
  __typename: "RulesetForm";
  id: number;
  name: string;
  version: string;
  description: string | null;
  kind: string | null;
  created: any | null;
  relationset: AllRulesetDetails_ruleset_relationset | null;
}

export interface AllRulesetDetails {
  /**
   * Returns a specific ruleset given a ruleset name, and version, e.g. "tigerconnect","v0.0.25". The version string "latest" returns the latest version for rulesets with the given name
   */
  ruleset: AllRulesetDetails_ruleset[];
}

export interface AllRulesetDetailsVariables {
  name: string;
  version: string;
}
```

Import the generated type to be used with API calls.

```typescript
import { AllRulesetDetails } from 'common/apis/carina/types/AllRulesetDetails'

/* .. */

const { data, error, loading, } = useQuery<
  AllRulesetDetails,
  QueryObjectType,
>(getAllRulesetDetailsQuery, {...})

```

[apollo_client]: https://www.apollographql.com/docs/react/
[create_react_app]: https://reactjs.org/docs/create-a-new-react-app.html
[blueprint_js]: https://blueprintjs.com/docs/#blueprint
[jest_js]: https://jestjs.io/
[jest_cli_options]: https://jestjs.io/docs/en/cli#using-with-yarn
[react_testing_library]: https://testing-library.com/docs/react-testing-library/intro/
[typescript_handbook]: https://www.typescriptlang.org/docs/handbook/intro.html
[sass_css]: https://sass-lang.com/
[css_modules]: https://github.com/css-modules/css-modules
[yarn_pkg_mgr]: https://yarnpkg.com/
[react_router]: https://reactrouter.com/web/guides/quick-start
