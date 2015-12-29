# List Selector

- order: 0

---

````jsx

import { ListWrap, ListHeader, ListBody, ListItem, ListSelector, Modal } from 'antm';
import { Router, Route } from 'react-router';
const PropTypes = React.PropTypes;

const SelectorData = [
  {
    name:'浙江',
    pinyin:'zhejiang',
    py:'zj',
    id:'zj'
  }, {
    name:'上海',
    pinyin:'shanghai',
    py:'sh',
    id:'sh'
  }, {
    name:'江苏',
    pinyin:'jiangsu',
    py:'js',
    id:'js'
  }, {
    name:'福建',
    pinyin:'fujian',
    py:'fj',
    id:'fj'
  }, {
    name:'山东',
    pinyin:'shandong',
    py:'sd',
    id:'sd'
  }, {
    name:'安徽',
    pinyin:'anhui',
    py:'ah',
    id:'ah'
  }];

let ListSelectorExample = React.createClass({
  propTypes: {
    value: PropTypes.array,
    data: PropTypes.array,
    onClick: PropTypes.func,
    needSearch: PropTypes.bool
  },
  render(props) {
    return (
      <Modal>
        <ListSelector
          value={this.props.value}
          data={this.props.data}
          onClick={this.props.onClick}
          needSearch={this.props.needSearch}
        />
      </Modal>
    );
  }
});

const ListSelectorPicker = React.createClass({
  propTypes: {
    childForm: PropTypes.object,
    onChange: PropTypes.func,
    value: PropTypes.array,
    data: PropTypes.array,
    needSearch: PropTypes.bool
  },
  onClick() {
    window.location.hash = '/listselector';
  },
  _handleClick(el) {
    this.props.onChange(el);
    this.setState({
      value: [el]
    });
  },
  render() {
    const { childForm, value } = this.props;
    const valueEl = value.length > 0 ?
      <ListItem
        extra={value[0].name}
        arrow="horizontal"
        onClick={this.onClick}
      >选择城市</ListItem> :
      <ListItem
        extra="请选择"
        arrow="horizontal"
        onClick={this.onClick}
      >选择城市</ListItem>;
    return (<div>
      {valueEl}
      {childForm ? React.cloneElement(childForm, {
        data: this.props.data,
        needSearch: true,
        value: value || {},
        onClick: this._handleClick
      }) : null}
    </div>);
  },
});

let pageForm = React.createClass({
  propTypes: {
    initialValue: PropTypes.array,
    children: PropTypes.any,
  },
  getDefaultProps() {
    return {
      initialValue: [],
      data: SelectorData,
    };
  },
  getInitialState() {
    return {
      value: this.props.initialValue,
    };
  },
  onChange(value) {
    console.log(value);
    this.setState({
      value:[value]
    });
  },
  render() {
    return (
      <ListWrap>
        <ListHeader>列表单选</ListHeader>
        <ListBody>
          <ListSelectorPicker
            childForm={this.props.children}
            value={this.state.value}
            data={this.props.data}
            onChange={this.onChange}
          />
        </ListBody>
      </ListWrap>
    );
  }
});

ReactDOM.render(<Router>
  <Route path="/" component={pageForm}>
    <Route path="/listselector" component={ListSelectorExample}/>
  </Route>
</Router>, document.getElementById('components-list-selector-demo-basic'));
````