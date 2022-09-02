# Styles 样式

makeStyles

返回值：一个hook，调用该hook，返回一个地址不变的对象。

默认情况下，`@material-ui/core/styles` 生成的类名是**不是固定值**， 所以您不能指望它们保持不变。

```tsx
import React from 'react';
import { makeStyles } from '@material-ui/core/styles';
import Button from '@material-ui/core/Button';

const useStyles = makeStyles({
  root: {
    background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
    border: 0,
    borderRadius: 3,
    boxShadow: '0 3px 5px 2px rgba(255, 105, 135, .3)',
    color: 'white',
    height: 48,
    padding: '0 30px',
  },
});

export default function Hook() {
  const classes = useStyles();
  return <Button className={classes.root}>Styled with Hook API</Button>;
}

```

