<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                ref="multipleTable"
                :data="tableData"
                highlight-current-row
                style="width: 100%" @selection-change="handleSelectionChange">

                <el-table-column type="selection" width="55"></el-table-column>
                <el-table-column
                    type="index"
                    width="100">
                </el-table-column>
                <el-table-column
                    property="school_ID"
                    label="学校ID"
                    width="220">
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

            <div style="margin-top: 20px">
                <el-button @click="deleteSchool()">删除选中学校</el-button>
            </div>

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
    import {getSchoolList, deleteSchool} from '@/api/getData'

    export default {
        name: "schoolManage",
        components: {
            headTop
        },
        data() {
            return {
                tableData: [],
                multipleSelection: []
            }
        },
        created() {
            this.returnSchoolList();
        },
        methods: {
            toggleSelection(rows) {
                if (rows) {
                    rows.forEach(row => {
                        this.$refs.multipleTable.toggleRowSelection(row);
                    });
                } else {
                    this.$refs.multipleTable.clearSelection();
                }
            },
            async deleteSchool() {
                let deleteSchoolArr = [];
                this.multipleSelection.forEach(item => {
                    deleteSchoolArr.push(item.school_ID);
                })
                console.log('deleteSchool:', deleteSchoolArr);
                const PostData = await deleteSchool(deleteSchoolArr);
                console.log(PostData);

            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1) * this.limit;
                this.getUsers()
            },
            async returnSchoolList() {
                const PostData = await getSchoolList();
                this.tableData = [];
                console.log(typeof PostData.data)
                PostData.data.forEach(item => {
                    const tableData = {};
                    tableData.school_name = item.school_name;
                    tableData.moment = item.moment;
                    tableData.city_ID = item.city_ID;
                    tableData.school_ID = item.school_ID;
                    this.tableData.push(tableData);
                })
            }
        },
    }
</script>

<style scoped>

</style>
