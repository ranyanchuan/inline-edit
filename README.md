# inline-edit

####  修改ucf-common 

1.ucf-common/src/components/RowField/NumberField.js 其他类型TextField、DateField等类似 
#####  修改前
```js
let {value,error, flag ,required,message} = this.state;
let { className, iconStyle, max, min, step, precision } = this.props;
```
#####  修改后  将 `value` 值从 this.props 中解构
```js
let {error, flag ,required,message} = this.state;
let { className, iconStyle, max, min, step, precision,value } = this.props;
```

2. 修改 FactoryComp 中的changeAllData() 方法, 根据需求添加义务代码
##### 修改前
```js
changeAllData = (field, value, index,refname) => {
        let oldData = this.oldData;
        let _sourseData = deepClone(this.props.list);
        oldData[index][field] = value;
        if(refname){
            oldData[index][field+"Name"] = refname;
        }
        //有字段修改后去同步左侧对号checkbox
        if (!_sourseData[index]['_checked']) {
            _sourseData[index]['_checked'] = true;
            actions.inlineEdit.updateState({ list: _sourseData });
        }
        oldData[index]['_checked'] = true;
        this.oldData = oldData;
    }
```
##### 修改后
```js

changeAllData = (field, value, index,refname) => {
        let oldData = this.oldData;
        let _sourseData = deepClone(this.props.list);
        oldData[index][field] = value;
        if(refname){
            oldData[index][field+"Name"] = refname;
        }
        //有字段修改后去同步左侧对号checkbox
        if (!_sourseData[index]['_checked']) {
            _sourseData[index]['_checked'] = true;
            actions.inlineEdit.updateState({ list: _sourseData });
        }


        //------------根据场景添加自己的义务代码---------------
          // 当 sex 值发生变化 将当前行为code 的值设置为 'xxxxxx'
          if(field==='sex'){
              const {isRender}=this.state;
              this.setState({isRender:!isRender});
              // 根据义务场景 自定义修改项
              _sourseData[index]['code'] = 'xxxxxxxxx';
          }
	//----------------------------------------------

        oldData[index]['_checked'] = true;
        this.oldData = oldData;
}
```









