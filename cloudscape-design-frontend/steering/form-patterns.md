# Cloudscape Form Patterns

Forms, inputs, validation, and selection components.

## TypeScript Types

Common types for form components:

```tsx
import { SelectProps, InputProps, NonCancelableCustomEvent } from '@cloudscape-design/components';

// Select option type
type SelectOption = SelectProps.Option;

// Form data interface
interface FormData {
  name: string;
  email: string;
  instanceType: SelectOption | null;
}

// Event handler types
type InputChangeHandler = (event: NonCancelableCustomEvent<InputProps.ChangeDetail>) => void;
type SelectChangeHandler = (event: NonCancelableCustomEvent<SelectProps.ChangeDetail>) => void;
```

## Form Structure

```tsx
import { useState } from 'react';
import { Form, Container, Header, SpaceBetween, Button, FormField, Input } from '@cloudscape-design/components';

interface FormData {
  name: string;
  email: string;
}

function BasicForm() {
  const [formData, setFormData] = useState<FormData>({ name: '', email: '' });

  return (
    <Form
      header={<Header variant="h1">Create Resource</Header>}
      actions={
        <SpaceBetween direction="horizontal" size="xs">
          <Button formAction="none" variant="link">Cancel</Button>
          <Button variant="primary">Submit</Button>
        </SpaceBetween>
      }
    >
      <Container>
        <SpaceBetween size="l">
          <FormField label="Name" description="Enter resource name">
            <Input
              value={formData.name}
              onChange={({ detail }) => setFormData(prev => ({ ...prev, name: detail.value }))}
            />
          </FormField>
          <FormField label="Email">
            <Input type="email" value={formData.email}
              onChange={({ detail }) => setFormData(prev => ({ ...prev, email: detail.value }))} />
          </FormField>
        </SpaceBetween>
      </Container>
    </Form>
  );
}
```

## FormField

```jsx
<FormField
  label="Email address"
  description="We'll never share your email"
  constraintText="Must be a valid email format"
  errorText={errors.email}
  info={<Link variant="info">Info</Link>}
>
  <Input type="email" value={email} invalid={!!errors.email}
    onChange={({ detail }) => setEmail(detail.value)} />
</FormField>
```

## Input Types

```jsx
<Input value={text} onChange={({ detail }) => setText(detail.value)} />
<Input type="password" value={password} onChange={({ detail }) => setPassword(detail.value)} />
<Input type="email" value={email} onChange={({ detail }) => setEmail(detail.value)} />
<Input type="number" value={num} step={1} onChange={({ detail }) => setNum(detail.value)} />
<Textarea value={text} rows={4} onChange={({ detail }) => setText(detail.value)} />
```

## Select

```tsx
import { useState } from 'react';
import { Select, SelectProps } from '@cloudscape-design/components';

type SelectOption = SelectProps.Option;

const [selected, setSelected] = useState<SelectOption | null>(null);

<Select
  selectedOption={selected}
  onChange={({ detail }) => setSelected(detail.selectedOption)}
  options={[
    { label: 't2.micro', value: 't2.micro', description: '1 vCPU, 1 GiB' },
    { label: 't2.small', value: 't2.small', description: '1 vCPU, 2 GiB' },
  ]}
  placeholder="Choose instance type"
  filteringType="auto"  // Enable search for many options
/>
```

## Multiselect

```tsx
import { useState } from 'react';
import { Multiselect, MultiselectProps } from '@cloudscape-design/components';

type MultiselectOption = MultiselectProps.Option;

const [selectedOptions, setSelectedOptions] = useState<readonly MultiselectOption[]>([]);

<Multiselect
  selectedOptions={selectedOptions}
  onChange={({ detail }) => setSelectedOptions(detail.selectedOptions)}
  options={options}
  placeholder="Choose options"
  tokenLimit={2}
/>
```

## RadioGroup

```jsx
const [value, setValue] = useState('option1');

<FormField label="Select edition">
  <RadioGroup
    value={value}
    onChange={({ detail }) => setValue(detail.value)}
    items={[
      { value: 'mysql56', label: 'MySQL 5.6', description: 'Legacy' },
      { value: 'mysql57', label: 'MySQL 5.7', description: 'Recommended' },
    ]}
  />
</FormField>
```

## Checkbox & Toggle

```jsx
// Checkbox - for agreement/selection
<Checkbox checked={agreed} onChange={({ detail }) => setAgreed(detail.checked)}>
  I agree to the terms
</Checkbox>

// Toggle - for immediate on/off effect
<Toggle checked={enabled} onChange={({ detail }) => setEnabled(detail.checked)}>
  Enable feature
</Toggle>
```

## DatePicker

```jsx
<FormField label="Start date">
  <DatePicker
    value={date}
    onChange={({ detail }) => setDate(detail.value)}
    placeholder="YYYY/MM/DD"
  />
</FormField>
```

## Form Validation

```tsx
import { useState } from 'react';
import { Form, FormField, Input, Button, SpaceBetween } from '@cloudscape-design/components';

interface FormData {
  email: string;
  password: string;
}

interface FormErrors {
  email?: string;
  password?: string;
}

function ValidatedForm() {
  const [formData, setFormData] = useState<FormData>({ email: '', password: '' });
  const [errors, setErrors] = useState<FormErrors>({});

  const validate = (): boolean => {
    const newErrors: FormErrors = {};
    if (!formData.email) newErrors.email = 'Email is required';
    else if (!/\S+@\S+\.\S+/.test(formData.email)) newErrors.email = 'Invalid email';
    if (!formData.password) newErrors.password = 'Password is required';
    else if (formData.password.length < 8) newErrors.password = 'Min 8 characters';
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = () => {
    if (validate()) {
      // Submit form
    }
  };

  return (
    <Form
      actions={<Button variant="primary" onClick={handleSubmit}>Submit</Button>}
      errorText={Object.keys(errors).length ? 'Please fix errors above' : ''}
    >
      <SpaceBetween size="l">
        <FormField label="Email" errorText={errors.email}>
          <Input type="email" value={formData.email} invalid={!!errors.email}
            onChange={({ detail }) => {
              setFormData(prev => ({ ...prev, email: detail.value }));
              if (errors.email) setErrors(prev => ({ ...prev, email: undefined }));
            }} />
        </FormField>
        <FormField label="Password" errorText={errors.password}>
          <Input type="password" value={formData.password} invalid={!!errors.password}
            onChange={({ detail }) => {
              setFormData(prev => ({ ...prev, password: detail.value }));
              if (errors.password) setErrors(prev => ({ ...prev, password: undefined }));
            }} />
        </FormField>
      </SpaceBetween>
    </Form>
  );
}
```

## Best Practices

1. Always wrap inputs with `FormField` for proper labeling
2. Use `SpaceBetween size="l"` between form fields
3. Group related fields in `Container` components
4. Use `constraintText` for format requirements
5. Show validation errors inline with `errorText`
6. Clear field errors when user starts typing
7. Use `formAction="none"` on Cancel buttons
8. Disable controls during submission
