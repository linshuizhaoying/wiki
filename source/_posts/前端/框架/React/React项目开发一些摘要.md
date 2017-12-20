---
title: React项目开发一些摘要
toc: true
date: 2017-08-22  14:30:41
tags: [前端,React,技术栈]
---

# React动画

### input css demo

```

.input {
  transition: width .35s linear;
  outline: none;
  border: none;
  border-radius: 4px;
  padding: 10px;
  font-size: 20px;
  width: 150px;
  background-color: #dddddd;
}

.input-focused {
  width: 240px;
}

```


```

class App extends Component {
  state = {
    focused: false
  }
  componentDidMount() {
    this.input.addEventListener('focus', this.focus);
    this.input.addEventListener('blur', this.focus);
  }
  focus = () => {
    this.setState((state) => ({ focused: !state.focused }))
  }
  render() {
    return (
      <div className="App">
        <div className="container">
          <input
            ref={input => this.input = input}
            className={['input', this.state.focused && 'input-focused'].join(' ')}
          />
        </div>
      </div>
    );
  }
}

```

#### input js demo

```

class App extends Component {
  state = {
    disabled: true,
  }
  onChange = (e) => {
    const length = e.target.value.length;
    if (length >= 4) {
      this.setState(() => ({ disabled: false }))
    } else if (!this.state.disabled) {
      this.setState(() => ({ disabled: true }))
    }
  }
  render() {
    const label = this.state.disabled ? 'Disabled' : 'Submit';
    return (
      <div className="App">
        <button
          style={Object.assign({}, styles.button, !this.state.disabled && styles.buttonEnabled)}
          disabled={this.state.disabled}
        >{label}</button>
        <input
          style={styles.input}
          onChange={this.onChange}
        />
      </div>
    );
  }
}

const styles = {
  input: {
    width: 200,
    outline: 'none',
    fontSize: 20,
    padding: 10,
    border: 'none',
    backgroundColor: '#ddd',
    marginTop: 10,
  },
  button: {
    width: 180,
    height: 50,
    border: 'none',
    borderRadius: 4,
    fontSize: 20,
    cursor: 'pointer',
    transition: '.25s all',
  },
  buttonEnabled: {
    backgroundColor: '#ffc107',
    width: 220,
  }
}

```

#### 一些库和其他

[react-animations](https://medium.com/react-native-training/react-animations-in-depth-433e2b3f0e8e)

