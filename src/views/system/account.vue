<template>
  <div class="container">
  	<!-- <v-search>
  		
  	</v-search> -->
  	<v-table :options="tableOption">
			<template v-slot:gender="{ data }">
				{{ genders[data.gender] }}
			</template>
			<template v-slot:roles="{ data }">
				{{ data.roles.name }}
			</template>
			<template v-slot:status="{ data }">
				<el-tag :type="isEnable[data.status].type">{{ isEnable[data.status].label }}</el-tag>
			</template>
  		<template v-slot:operation="{ data }">
  			<el-button type="primary" size="mini" @click="handleEdit(data)">编辑</el-button>
  			<el-button type="danger" size="mini" @click="handleDelete(data)">删除</el-button>
  		</template>
  	</v-table>
  	<v-dialog :visible.sync="isShow" :title="title">
  		<v-form :form-item="formItem" :formData="formData" :visible.sync="isShow" @submit="submit" />
  	</v-dialog>
  </div>
</template>
<script>
import { mapGetters, mapState } from 'vuex'
export default {
	data() {
		return {
			tableOption: {
				operates: [
					{
						label: '添加',
						event: () => this.handleAdd()
					}
				],
				tHead: [
					{ label: '后台账号', field: 'username' },
					{ label: '账号角色', field: 'roles', slotName: 'roles' },
					{ label: '真实姓名', field: 'name' },
					{ label: '性别', field: 'gender', slotName: 'gender' },
          { label: '手机号码', field: 'phone' },
					{ label: '状态', field: 'status', slotName: 'status' },
					{ label: '操作', field: 'operation',slotName: 'operation', width: '160px' }
				],
				tableList: [],
				loading: false
			},
			isEnable: {
				true: { label: '启用', type: 'success' },
				false: { label: '禁用', type: 'danger' },
			},
			curd: '',
			title: '',
			isShow: false,
			formItem: [
				{ type: 'input', label: '后台账号', field: 'username', required: true },
				{ type: 'input', label: '后台密码', field: 'password', required: true },
				{ type: 'select', label: '账号角色', field: 'roles', required: true, data: 'roles', props: {
					label: 'name',
					value: '_id'
				} },
				{ type: 'input', label: '真实姓名', field: 'name', required: true },
        { type: 'radio', label: '性别', field: 'gender', option: [
					{ label: '男', value: 'male' },
					{ label: '女', value: 'woman' }
				] },
				{ type: 'input', label: '手机号码', field: 'phone', required: true },
				{ type: 'radio', label: '状态', field: 'status', option: [
					{ label: '启用', value: true },
					{ label: '禁用', value: false }
				]}
			],
			formData: {
				name: '',
        gender: 'male',
        phone: '',
        roles: '',
        status: true
			}
		}
	},
	computed: {
		...mapGetters(['addroutes']),
		...mapState({
			genders: state => state.global.genders
		})
	},
	mounted() {
		this.getTableList()
	},
	methods: {
		// 表格列表
		async getTableList() {
      const table = this.tableOption
      table.loading = true
      const result = await this.$http.get('/rest/query/account')
      table.loading = false
      if (!result) return 
			const { data } = result.data
      table.tableList = data
    },
		// 添加
    handleAdd() {
    	this.curd = 'create'
    	this.title = '添加账号'
    	this.isShow = true
    },
		// 编辑
    handleEdit(rowData) {
    	this.curd = 'update'
    	this.title = '编辑账号'
			this.$nextTick(() => {
				this.formData = Object.assign({}, this.formData, rowData)
				this.formData.roles = rowData.roles._id
			})
    	this.isShow = true
    },
		// 删除
    handleDelete() {
    	this.curd = 'create'
    	this.title = '删除账号'
    	this.isShow = true
    },
		// 提交
		async submit() {
			const result = await this.$http.post(`/rest/${this.curd}/account`, this.formData)
			if (!result) return
			this.isShow = false
			this.getTableList()
		},
		showTree(data) {
			const arr = []
			data.forEach(item => {
				item.children = []
				data.forEach(child => {
					if (item._id === child.pid) {
						item.children.push(child)
					}
				})
				arr.push(item)
			})
			return arr.filter(item => !item.pid)
		}
	}
}
</script>