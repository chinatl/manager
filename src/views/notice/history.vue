<template>
  <div class="layout">
    <div class="filter-container">
      <el-button class="filter-item" style="margin-left: 10px;" @click="add_item" type="primary" icon="el-icon-edit">添加</el-button>
    </div>
<!-- 表格 -->
   
    <el-table :key='tableKey' :data="list" border fit highlight-current-row
      style="width: 100%;margin-top:20px">
			<el-table-column align="center" label="序号" width="80">
					<template slot-scope="scope">
					  <span>{{scope.$index+1}}</span>
					</template>
</el-table-column>
<el-table-column align="center" label="公告标题">
	<template slot-scope="scope">
					  <span>{{scope.row.notice}}</span>
					</template>
</el-table-column>
<el-table-column align="center" label="公告内容">
	<template slot-scope="scope">
		  <el-button @click='open(scope.row.noticenr)' size='small' type='success'>查看详情</el-button>
	</template>
</el-table-column>
<el-table-column align="center" label="操作" class-name="small-padding" width="200">
	<template slot-scope="scope">
		  <el-button v-if="scope.row.status!='deleted'" size="mini" type="danger" @click="del_item(scope.row)">删除
		  </el-button>
	</template>
</el-table-column>
</el-table>
<!-- 弹出层 -->
<el-dialog title='公告内容' :visible.sync='cons'>
	<div v-html='notie_con'></div>
</el-dialog>
<el-dialog :title="title" :visible.sync="show_dialog">
	<el-form ref="dataForm" label-position="left" label-width="90px" style='max-width: 900px; margin-left:50px;'>
		<el-form-item label="公告标题：" prop="type">
			<el-input v-model='form.notice'></el-input>
		</el-form-item>
		<el-form-item label="公告内容：" prop="type">
			<vue-editor v-model="content" ></vue-editor>
		</el-form-item>

	</el-form>
	<div slot="footer" class="dialog-footer">
		<el-button @click="cancel_click">取 消</el-button>
		<el-button type="primary" @click="agree_click">确 定</el-button>
	</div>
</el-dialog>
<div class="pagination-container page_size">
	<el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page.sync="listQuery.pageNo" :page-sizes="[5, 10, 15, 20]" :page-size="listQuery.pageSize" layout="total, sizes, prev, pager, next, jumper" :total="total">
	</el-pagination>
</div>
</div>
</template>

<script>
	import waves from '@/directive/waves' // 水波纹指令
	import {
		VueEditor
	} from 'vue2-editor'
	import {
		parseTime
	} from '@/utils'
	import Get from '@/api/get.js'
	import Post from '@/api/post.js'
	export default {
		name: 'complexTable',
		directives: {
			waves
		},
		components: {
			VueEditor
		},
		data() {
			return {
				content: '',
				notie_con:'',
				options: [],
				cons: false,
				form: {
					vName: '',
					shortName: '',
					description: '',
					currencyType: '',
					decimalPlaces: '',
					vStatus: '0',
					ip: '',
					port: '',
					startTime: "",
					endTime: ''
				},
				type: 'add',
				title: '数据添加',
				show_dialog: false,
				tableKey: 0,
				list: [],
				total: null,
				listQuery: {
					pageNo: 1,
					pageSize: 5,
					isStart: '2',
					loginName: '',
					id: ''
				},
				fileList: [],
				value1: '',
			}
		},
		created() {
			this.init()
		},
		methods: {
			open(str) {
				console.log(str)
				this.cons = true;
				this.notie_con = str
			},
			handleRemove(file, fileList) {
				console.log(file, fileList);
			},
			handlePreview(file) {
				console.log(file);
			},
			init() {
				Get({
					url: 'notice/findAll',
					data: {
						notice: this.listQuery.notice,
						pageSize: this.listQuery.pageSize,
					},
					success: res => {
						this.list = res.data;
					}
				})
			},
			cancel_click() {
				this.init_form();
			},
			agree_click() {
				Post({
					url: 'notice/addNotice',
					data: {
						notice: this.form.notice,
						noticenr: this.content,
					},
					success: res => {
						this.show_dialog = false;
						this.$notify({
							title: '成功',
							message: '增加成功',
							type: 'success',
							duration: 2000
						})
						this.init_form();
						this.init();
					}
				})
			}, //点击弹出框确认的事件；
			select_item() {
				this.listQuery.page = 1
				this.init();
			},
			handleSizeChange(val) {
				this.listQuery.pageSize = val
				this.init();
			},
			handleCurrentChange(val) {
				this.listQuery.pageNo = val
				this.init();
			},
			del_item(row, status) {
				Post({
					url: 'notice/deleteNotice',
					data: {
						id: row.id
					},
					success: res => {
						this.$notify({
							title: '成功',
							message: '删除成功！',
							type: 'success',
							duration: 2000
						})
						this.init_form();
						this.init();
					}
				})
			},
			add_item() {
				this.show_dialog = true;
				this.type = 'add';
				this.title = '添加';
			},
			init_form() {
				this.show_dialog = false;
				this.form = {
					loginName: '', //登录名
					loginPassword: '', //登录密码
					organize: '', //分组
					phone: '', //电话
					email: '', //邮箱
					isStart: '0', //状态（0：开启；1：关闭）
					id: null
				}

			}

		}
	}

</script>

<style>
	.el-dialog__body {
		padding: 10px 20px
	}
</style>
