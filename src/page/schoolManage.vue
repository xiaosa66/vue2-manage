<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                  type="index"
                  width="100">
                </el-table-column>
                <el-table-column
                  property="moment"
                  label="注册日期"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="school_name"
                  label="学校名称"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="city_ID"
                  label="注册地址">
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
        </div>
    </div>

</template>

<script>
    import headTop from '../components/headTop'
    import {getSchoolList} from '@/api/getData'

    export default {
		name: "schoolManage",
        components: {
            headTop
        },
         data(){
            return {
                tableData: []}},
        created(){
            this.returnSchoolList();
        },
        methods: {

            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                this.getUsers()
            },
            async returnSchoolList(){
                    const PostData = await getSchoolList();
                    this.tableData = [];
                    console.log(typeof PostData.data)
                    PostData.data.forEach(item => {
                    const tableData = {};
                    tableData.school_name = item.school_name;
                    tableData.moment = item.moment;
                    tableData.city_ID = item.city_ID;
                    this.tableData.push(tableData);
                })
            }
        },
	}
</script>

<style scoped>

</style>
