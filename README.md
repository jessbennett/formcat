<p align="center"><img src="./logo.jpg" width="550px"/></p>

A simple and easy way to control forms in React using the [React Context API](https://reactjs.org/docs/context.html)


[![Build Status](https://travis-ci.com/guilouro/formcat.svg?token=ypLGJBT7BX8hXAJnirC4&branch=master)](https://travis-ci.com/guilouro/formcat)
[![codecov.io](https://codecov.io/gh/guilouro/formcat/branch/master/graph/badge.svg)](https://codecov.io/gh/guilouro/formcat)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)



## Getting Started

## Install

With npm

```
npm install --save formcat
```

With yarn

```
yarn add formcat
```

## How to use

First of all, we need to create a Field using the [HOC](https://facebook.github.io/react/docs/higher-order-components.html) `withContextForm` as the example below:

```js
/* InputField.js */

import { withContextForm } from 'formcat'

const InputField = ({ error, ...input }) => (
  <input {...input} />
)

export default withContextForm(InputField)
```

Now we can use this component inside the Form:

```js
import { Form } from 'react-contex-form'
import InputField from './InputField'

function Main () {
  return (
    <Form>
      <InputField name="whatever" />
    </Form>
  )
}
```

# API

### Form

| Props | Type | Default value | Description |
| ----- | ---- | ------------- | ----------- |
| **keyUpValidation** | Boolean | `false` | When `true` the validations Field works with `keyUp` |
| **clearAfterSubmit** | Boolean | `false` | When `true` the form will be reset after submit |
| **onFormChange** | Function | `undefined` | A callback that returns an object with `name`, `type` and `value`<br> of the current change.  |
| **onSubmit** | Function | `undefined` | A callback that returns an object with status and values.  |

### Submit

For an easy `submit` process we can use the HOC `withSubmit` and create a Button that will be controlled by Form, or using the Submit component that already exists.

```js
// Creating
import { withSubmit } from 'formcat'

const Submit = withSubmit(props => <button {...props} />)

//or

import { Submit } from 'formcat'

// Using

...
  render (
    <Form>
      ...
      <Submit> Button Text </Submit>
    </Form>
  )
...
```

*Obs: This `button` will be enabled when Form is valid and disabled when is not valid*


### Custom Field

It's a field created with `withContextForm`.

| Props | Type | Default value | Description |
| ----- | ---- | ------------- | ----------- |
| **error** | Boolean | `false` | A flag that controls the field validation |
| **validations** | Array | `[]` | A list with functions validation |
| **required** | Boolean | `false` | Set required validation for this field |
| **defaultValue** | String | `""` | Set initial text value |
| **defaultChecked** | String | `""` | Set initial checked for field |

## Using validations

We can use many validations per field using the props `validations`. All we need to do is create a pure function that returns `true` or `false` following the example below:

```js
import { Form, withFormContext } from 'formcat'

const Field = withFormContext(({ error, ...input }) => (
  <input {...input} />
))


const Main = () => {

  // Validate if username is @guilouro
  const usernameValidation = value => (
    value === '@guilouro'
  )

  return (
    <Form>
      <Field
        name="username"
        validations={[usernameValidation]}
      />
    </Form>
  )
}
```

A validation function has two params `value` and `state`:

```js
function validationName (value, state) {}
```

| Param | Type | Description |
| ----- | ---- | ------------- |
| **value** | String  | Current field value |
| **state** | Object  | An object with all fields value |

### Set values

We can set values out of Form using `Ref` and `updateFieldValue` as the example below:

| Param | Type | Description |
| ----- | ---- | ------------- |
| **name** | String | `null`  | Field name |
| **text** | String  | A new value for this field |

```js
import { Form, withFormContext } from 'formcat'

const Field = withFormContext(({ error, ...input }) => {}(
  <input {...input} />
))


const Main = () => {
  const form = useRef(null)

  const setValue = () => {
    form.current.updateFieldValue('username', '@guilouro')
  }

  return (
    <>
      <Form ref={ref}>
        <Field name="username" />
      </Form>

      <button onClick={setValue}>
        Set @guilouro as value
      </button>
    </>
  )
}

```

## Fields we can use

There are some simple field created with `withContexForm` we can use in project or use as a reference for create a new custon field

### InputField

A simple `input` field

```js
import { InputField } from 'formcat/fields'

...
<InputField
  label="My Input"
  name="my-select"
/>
...
```

| Param | Type | Default value | Description |
| ----- | ---- | ------------- | --------------|
| **name** | String | `null`  | Field name |
| **label** | String  | `''` | A label for this field |
| **type** | String  | `text` | A type for this input |

*Obs: And all common props*

### CheckboxField

A `input` `checkbox` field

```js
import { CheckboxField } from 'formcat/fields'

...
<CheckboxField
  label="My Input"
  name="my-select"
/>
...
```

| Param | Type | Default value | Description |
| ----- | ---- | ------------- | --------------|
| **name** | String | `null`  | Field name |
| **label** | String  | `''` | A label for this field |
| **defaultChecked** | Boolean  | `false` | A flag to define the initial status |

*Obs: And all common props*

### RadiosField

A simple `input` `radio` field

```js
import { RadiosField } from 'formcat/fields'

...
<RadiosField
  label="My Select"
  name="my-select"
  options={[
    { label: 'Item 1', value: 1 },
    { label: 'Item 2', value: 2, checked: true }
  ]}
/>
...
```

| Param | Type | Default value | Description |
| ----- | ---- | ------------- | --------------|
| **name** | String | `null`  | Field name |
| **label** | String  | `''` | A label for this field |
| **options** | Array  | `[]` | A list of objects with `label`, `value` and `checked` |

*Obs: And all common props*


### SelectField

A simple `select` field

```js
import { SelectField } from 'formcat/fields'

...
<SelectField
  label="My Select"
  name="my-select"
  options={[
    { label: 'Item 1', value: 1 }
  ]}
/>
...
```

| Param | Type | Default value | Description |
| ----- | ---- | ------------- | --------------|
| **name** | String | `null`  | Field name |
| **label** | String  | `''` | A label for this field |
| **options** | Array  | `[]` | A list of objects with `label` and `value` |

*Obs: And all common props*

### TextField

A simple `textarea` field

```js
import { TextareaField } from 'formcat/fields'

...
<TextareaField
  label="My Text"
  name="my-text"
/>
...
```

| Param | Type | Default value | Description |
| ----- | ---- | ------------- | --------------|
| **name** | String | `null`  | Field name |
| **label** | String  | `''` | A label for this field |

*Obs: And all common props*

### Error styles

Invalid fields will receive a class: `className="formcat-error"`


## Examples

### Full form

A example with all fields, validation and a populate button

[![Edit Formcat](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/93465kmow?fontsize=14)

### Creating a custom fields

A example of the how create a custom field

[![Edit Creating custom fields](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/3rpxomlm15?fontsize=14)

---

## Contributing

If you want to contribute with this component:
[Contributing Documentation](https://github.com/guilouro/formcat/blob/master/.github/CONTRIBUTING.md).
