# MUI with React Hook Form

To use React Hook Form with MUI we can use the `Controller` component

```tsx
import MuiTextField, { TextFieldProps } from "@mui/material/TextField";
import { Controller, Control, FieldValues } from "react-hook-form";

type Props = {
  control: Control<FieldValues, object>;
  name: string;
  [x: string]: any;
};

// Wrap the TextField component in a Controller
const TextField = ({ control, name, ...rest }: Props) => {
  return (
    <Controller
      control={control}
      name={name}
      render={({ field }) => (
        <MuiTextField {...field} {...(rest as TextFieldProps)} />
      )}
    />
  );
};

export default TextField;
```
