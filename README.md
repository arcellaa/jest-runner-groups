# jest-runner-groups 2.0.0

[![npm version](https://badge.fury.io/js/jest-runner-groups.svg)](https://badge.fury.io/js/jest-runner-groups)

A test runner that allows you to tag your tests and execute specific groups of tests with Jest.

## Instalation

```sh-session
npm i -D jest-runner-groups jest-runner jest-docblock
```

Please, make sure that you install the same versions of `jest-runner` and `jest-docblock` as your version of `jest`. In other words, if you use `jest@^24.9.0`, then you need to install `jest-runner@^24.9.0` and `jest-docblock@^24.9.0`. If you use the latest version of jest, then install runner and docblock as regular.

## Usage

To use this runner you need to tag your tests, add this runner to your jest config and update your test command to specify which groups to run.

### Tag your tests

To properly tag your tests, you need to add a docblock with the `@group` tag to every test file you have. For example, your test should look like the following to belong to the `unit/classes/foo` group:

```javascript
/**
 * Tests Foo class
 * 
 * @group unit/classes/foo
 */

import Foo from '../src/Foo';

describe( 'Foo class', () => {
    it( '...', () => {
        ...
    } );

    ...
} );
```

Your tests may have multiple groups per file:

```javascript
/**
 * Admin dashboard tests
 * 
 * @group admin/pages/dashboard
 * @group puppeteer
 * @group acceptance
 */

describe( 'Dashboard page', () => {
    ...
} );
```

### Update Jest config

To make Jest use this runner, you need to update your Jest config and add `groups` runner to it. For example, if your jest config is in the `package.json` file:

```json
{
    "name": "my-package",
    "version": "1.0.0",
    "dependencies": {
    },
    "jest": {
        "runner": "groups"
    }
}
```

Or in the `jest.config.js` file:

```javascript
module.exports = {
    ...
    runner: "groups"
};
```

### Run groups of tests

Once you update your tests and jest config, you can start running tests in groups by using `--group` argument. Just specify a group or groups that you want to run like this:

```sh-session
// using jest executable:
jest --group=unit

// or via npm:
npm test -- --group=unit
```

You can use multiple `--group` arguments to specify multiple groups to run:

```sh-session
npm test -- --group=unit/classes --group=unit/services
```

Also pay attention that if you specify a prefix of a group, then all tests that have a group that starts with that prefix will be executed. In other words, if you run `npm test -- --group=unit` command, then all tests that have a group that starts with `unit` will be executed.

### Exclude groups

If you want to exclude a subgroup from being executed, add minus character to the beginnig of its name. The following example shows how to run all tests in the `foo` group, but exclude `foo/baz` group:

```sh-session
jest --group=foo --group=-foo/baz
```

## Contribute

Want to help or have a suggestion? Open a [new ticket](https://github.com/eugene-manuilov/jest-runner-groups/issues/new) and we can discuss it or submit a pull request.

## License

MIT
