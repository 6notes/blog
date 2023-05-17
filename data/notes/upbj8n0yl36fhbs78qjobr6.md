
# The number sizes in props

- There are number sizes in the style props that can be used instead of pixel
  size. For example:

  ```
  function example1(){
    return <Box height={6}>
  }
  function example2(){
    return <Box height={"60px"}>
  }
  ```

- In
  [the docs for default theming](https://chakra-ui.com/docs/styled-system/theme#spacing),
  they explain what the numbers mean. For example, `0.5` refers to `'0.125rem'`.
