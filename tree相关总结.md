## <center>tree树状图相关使用</center>

### 使用elementui中的树形控件

1. 如何获取树节点选中的key ID？
 
 在tree中绑定ref='tree',使用getCheckedKeys属性获取选中节点的key所组成的数组
 
		this.$refs.tree.getCheckedKeys(true)
		
	使用getCheckedNodes属性获取所选中节点的数据组成的数组

		this.$refs.tree.getCheckedNodes(true)	
	
2. 如何默认树节点的选中项?

	default-checked-keys属性的使用，将想要选中的节点的key值放入default-checked-keys对应的数组中即可默认选中

3. 如何获取选中的key ，以及未全部选中子节点的父节点的key ?
	https://segmentfault.com/q/1010000012309004					     
	* 获取选中的节点，节点中能找到父节点的key,将父节点存储到同一个数组，在进行去重
	*  获取选中的节点的key，组成一个数组
	*  将两个数组合并，即所有节点
					     
###  下拉框显示tree功能

   https://blog.csdn.net/weixin_45283768/article/details/100702765

1. 安装npm install --save @riophae/vue-treeselect

		<treeselect 
                   v-model="ruleForm.parentId" 
                   :multiple="false"  
                   :normalizer="normalizer"
                   :options="options"   
                   placeholder="请选择上级类目"
           />

2. :normalizer="normalizer" ，修改对应显示的id,labal,判断node为空的删除，

		normalizer(node) {
                if(node.children == null || node.children.length==0){
                    delete node.children;
                }
                return {
                    id: node.menuId,
                    label: node.menuName,
                    children: node.children,
                }
            },
 
3. multiple为true表示可以多选，为false单选，单选的时候v-model对应的数据为null,多选时可以有一个初始化的值。
            
            
            
            
            
            
            