## angular的ng-grid的表格
一般情况下，使用angular，用到表格，那就一定要使用ng-grid。如果是正常的表格，没有什么特殊的要求，直接在[ng-grid官网](http://angular-ui.github.io/ui-grid/) 上看api就足够了。
这里主要歇一下我在ng-grid使用过程中遇到的问题；


```markdown

### 第一个问题

- 表格下方会出现滚动条
- 解决方式：一般是给.ng-row设置了border，把左右border去掉即可

### 第二个问题

- 表格需要增加一个大标题列
- 解决方式：在gridOptions中设置 showGroupPanel:true,并且window.ngGrid.i18n['en'] ={ ngGroupPanelDescription: '线下数据源-电核任务' };重新赋值即可

### 第三个问题

- 设置斑马条
- 解决方式：css中设置  .ngRow.odd{background: 色值; }

### 第四个问题

- 设置表格下border
- 解决方式：css中设置  .ngCell{border-bottom:1px solid #d4d4d4;}

### 第五个问题

- 表格增加搜索功能
- 解决方式：在gridOptions中设置showFilter:true,

### 第六个问题

- 表格中给特定的一列增加样式或者效果
- 解决方式：在gridOptions -- > columnDefs -->中设置cellClass:'noCenter',然后给这个类名增加样式就可以了

### 第七个问题

- 表格获取指定的一行
- 解决方式：使用row作为参数进行传参，使用他上面的方法和属性（在grid表格加载完之后才可以使用）

### 第八个问题

- 出现错误Cannot set property 'gridDim' of undefined
- 解决方式：在js中没有对$scope.gridOptions进行定义配置，2.浏览器缓存，重新打开

### 第九个问题

- 发现死活不出现数据，但是不报错
- 解决方式：找表格对应的controller，不能把ng-controller写在自己身上，要写在其父亲身上

### 第十个问题

- 更改每一列的边框
- 解决方式：.ngFooterPanel {border-top: 1px solid transparent;}

### 第十一个问题

- 更改每一行的border
- 解决方式：.ngHeaderScroller{ border-bottom: 1px solid #253B4B;}

### 第十二个问题

- 去掉total这一行（分页）
- 解决方式： .ngTotalSelectContainer{display: none; }

### 第十三个问题

- 复选框的出现
- 解决方式：showSelectionCheckbox:'true'，但是注意不能写enableRowSelection:false,否则第一个选择框不出现

### 第十四个问题

- 更改表格的序号
- 解决方式：{field:'id',displayName:'序号',cellTemplate:' <div><span ng-cell-text>{{row.rowIndex}}</span></div>' },

### 第十五个问题

- 从后台获取的数据，根据属性选择是否选中哪一行
- 解决方式：checkboxCellTemplate:'<div><input type="checkbox" ng-checked="{{row.entity.checked}}"></div>'

### 第十六个问题

- .出现了使用ng-grid的多选showSelectionCheckbox这个属性和表格错开一列的问题？
- 解决方式：因为使用了data-ng-include这个指令，会创建一个dom元素，最后使表格增加一行空白列，也不能删，因为是angular的ng-grid自动生成的，所以只能找到当前的那个复选框，进行位置的位移。而且表头的多选框和表身的多选框的class命不一样，所以要相应的进行更改

### 第十七个问题

- 根据后台传给的值进行判断是否显示单选还是多选还是文本框，这个时候模版进行字符串拼接，拼接之后ng-grid不认识这个语法，需要进行转义
- 解决方式：在对应的那一列的
            1.filed中写cellTemplate: '<div ng-bind-html="row|typeFormatter:row.rowIndex"></div>'// 使用过滤器进行过滤
            2.定义过滤器（根据不同的结果进行不同样式的字符串拼接）
              myMod.filter('typeFormatter', function ($sce) {
                      return function (data) {
                          var str = "";
                          var dh_type = data.getProperty('dh_type');
                          var dh_content = data.getProperty('dh_content');
                          if (dh_type == 0) {
                              str = '<input type="text" value="' + dh_content + '" ng-model="name1"/>';

                          } else if (dh_type == 1) {
                              var radios = dh_content.split('_');
                              for (var j = 0; j < radios.length; j++) {
                                  str += '<input type="radio" value="'+radios[j]+'" name="content' + data.rowIndex + '"/>' + radios[j]
                              }
                          } else if (dh_type == 2) {
                              var checkboxs = dh_content.split('_');
                              for (var i = 0; i < checkboxs.length; i++) {
                                  str += '<input type="checkbox" value="'+checkboxs[i]+'"/>' + checkboxs[i]
                              }
                          }
                          return $sce.trustAsHtml(str);  //在控制器中注入$sce这个服务，使用它上面的trustAsHtml这个方法将字符串进行转义；
                      }
                  });
   ##最后可以看一下关于row下面的属性
   ![](.png)



```

