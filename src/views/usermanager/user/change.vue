<template>
	<tl-table 
		:headData ='head_data'
		:formData ='form_data'
		:tableData ='table_data'
		:urlData ='url'
	></tl-table>
</template>

<script>
	import tlTable from '@/components/Table';
	export default {
		components: {
			tlTable
		},
		data() {
			return {
				head_data: [{
					label: '用户名',
					prop: 'nickName',
				}, {
					label: '操作类型',
					prop: 'isStart',
					type: 'select',
					options: [{
						id: '开启',
						value: '0'
					}, {
						id: '禁用',
						value: "1"
					}, {
						id: '全部',
						value: '2'
					}]
				}, {
					type: 'search'
				}],
				form_data: [{
					label: '登录名',
					prop: 'loginName',
				}, {
					label: '登录密码',
					prop: 'loginPassword',
				}, {
					label: '分组',
					prop: 'organize',
				}, {
					label: '电话',
					prop: 'phone',
				}, {
					label: '邮箱',
					prop: 'email',
				}, {
					label: '状态',
					prop: 'isStart',
					type: 'select',
					default: '0',
					options: [{
						id: '启用',
						value: '0'
					}, {
						id: '关闭',
						value: "1"
					}]
				}],
				table_data: [{
					label: '用户名',
					prop: 'nickName',
					width: 120
				}, {
					label: '币种名称',
					prop: 'shortName'
				}, {
					label: '地址',
					prop: 'walletAddress'
				}, {
					label: '添加时间',
					prop: 'addTime',
					type: 'date'
				}, {
					label: '状态',
					prop: 'isStart',
					type: 'status',
					status:true,
					options: [{
						id: '启用',
						value: '0'
					}, {
						id: '关闭',
						value: "1"
					}]
				}],
				url: {
					search: {
						url: 'userWalletAddress/findAll',
						params: {
							isStart: '2', //状态（0：启用；1：关闭）
							nickName: '' //用户名},
						}
					},
					add: {
						url: 'vip/addvipuser',
						params: [{
							name: 'viplevel',
						}, {
							name: 'socre',
						}, {
							name: 'tradingPoundageDiscount',
						}, {
							name: 'reflectPoundageDiscount',
						}]
					},
					update: {
						url: 'admin/updateAdmin',
						params: [{
							name: 'viplevel',
						}, {
							name: 'vipid',
						}, {
							name: 'socre',
						}, {
							name: 'tradingPoundageDiscount',
						}, {
							name: 'reflectPoundageDiscount',
						}]
					},
					update_status: {
						url: 'admin/updateisStart',
						params: [{
							name: 'id',
						}, {
							name: 'isStart',
						}],
						query: 'isStart'
					},
					del: {
						url: 'vip/deletevipuserbyid',
						params: [{
							name: 'vipid1',
						}]
					},
					find: {
						url: 'adminLog/findAllById',
						params: [{
							name: 'nickName',
						}],
						data: 'id',
						query: 'userId',
					}
				}
			}
		}
	}

</script>
