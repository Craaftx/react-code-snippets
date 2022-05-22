# String to HEX Color

This function can be used to generate an HEX color from a string. It can be usefull to generate default profile colors.

```js
const stringToHexColor = (string: string) => {
  let hexColor = "#";
  let hash = 0;
  let i;

  for (i = 0; i < string.length; i += 1) {
    hash = string.charCodeAt(i) + ((hash << 5) - hash);
  }

  for (i = 0; i < 3; i += 1) {
    const value = (hash >> (i * 8)) & 0xff;
    hexColor += `00${value.toString(16)}`.substr(-2);
  }

  return hexColor;
};

export default stringToHexColor;
```
